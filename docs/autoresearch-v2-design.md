# AutoResearch v2 — Design Document

> Baseado nos princípios de Andrej Karpathy (AutoResearch + LLM Wiki)
> Data: 2026-07-17
> Autor: Omega ♾️
> Status: DRAFT → aguardando aprovação do Denalth

---

## TL;DR

Sistema de experimentação autônoma com **loop fechado real**: cada mudança é uma hipótese testável, medida contra métricas concretas, e só sobrevive se provar melhoria. Caso contrário, é revertida.

---

## 1. PRINCÍPIOS KARPATHY (fundação)

### 1.1 Ground Truth
Toda mudança precisa de uma métrica objetiva que diga "melhorou" ou "piorou".
Sem métrica = sem experimento. Ponto.

### 1.2 Loop Fechado
```
HIPÓTESE → MUDA → MEDE → COMPARA → COMMIT ou REVERT
```
Nunca commit sem evidência. Nunca reportar "sucesso" sem antes/depois.

### 1.3 Source-backed Claims
Toda afirmação em relatório deve citar:
- Arquivo/linha específica examinada
- Métrica numérica antes vs depois
- Fonte da decisão (não "acho que")

### 1.4 Escopo Cirúrgico
Uma hipótese por run. Não "otimize tudo". Uma coisa específica, testável, reversível.

---

## 2. ARQUITETURA V2

### 2.1 Estrutura de Diretórios
```
/root/.openclaw/autoresearch-v2/
├── metrics/
│   ├── baseline.json          # Snapshot atual (atualizado a cada run)
│   └── history.jsonl          # Histórico de todas as medições
├── experiments/
│   ├── YYYY-MM-DD/
│   │   ├── hypothesis.md      # O que foi testado
│   │   ├── before.json        # Métricas antes
│   │   ├── after.json         # Métricas depois
│   │   └── verdict.md         # Resultado + decisão (commit/revert)
├── templates/
│   ├── experiment.md          # Template de hipótese
│   └── metrics-collect.sh     # Script de coleta de métricas
└── config.toml                # Configuração dos agentes
```

### 2.2 Metric Collector (metrics-collect.sh)

Script shell que coleta métricas objetivas em formato JSON. Roda antes e depois de cada experimento.

**Métricas coletadas:**

```json
{
  "timestamp": "2026-07-17T22:00:00-03:00",
  "memory": {
    "total_files": 299,
    "total_size_kb": 7168,
    "duplicates_by_date": 23,
    "orphan_files": 5,
    "broken_links_in_memory_md": 0,
    "empty_dirs": 3,
    "files_over_30_days": 45,
    "oldest_file_date": "2026-05-16"
  },
  "wiki": {
    "total_pages": 27,
    "orphan_pages": 0,
    "contradictions": 0,
    "avg_page_size_bytes": 16000,
    "pages_without_sources": 12,
    "broken_links": 2,
    "stale_pages_days": 15
  },
  "system": {
    "disk_usage_openclaw_mb": 6963,
    "session_count": 15,
    "cron_error_count": 1,
    "log_errors_24h": 3,
    "npm_cache_mb": 537
  }
}
```

### 2.3 O Loop (execução por run)

Cada run do cron executa EXATAMENTE estes passos:

```python
# 1. COLHE BASELINE (antes)
before = run("metrics-collect.sh")

# 2. PROPOE HIPÓTESE (uma só, específica)
hypothesis = {
    "change": "Consolidar 9 arquivos duplicados do dia 2026-06-30",
    "expected": "Reduz files_over_30_days e duplicates_by_date sem perder informação",
    "metric": "duplicates_by_date deve diminuir",
    "reversible": True
}

# 3. EXECUTA MUDANÇA
git stash  # checkpoint pra revert
apply_change(hypothesis)

# 4. COLHE MÉTRICAS (depois)
after = run("metrics-collect.sh")

# 5. COMPARA
result = compare(before, after)

# 6. DECIDE
if result.improved(hypothesis.metric):
    git commit("experiment: " + hypothesis.change)
    verdict = "COMMIT"
else:
    git checkout .  # REVERT
    verdict = "REVERT"

# 7. REGISTRA
save_experiment(hypothesis, before, after, result, verdict)
```

### 2.4 Schedule de Rotação (uma tarefa por dia)

Em vez de 3 agentes genéricos todo dia, **um agente com escopo rotativo**:

| Dia | Domínio | Hipótese típica |
|-----|---------|-----------------|
| Seg | Memory dedup | Consolidar arquivos com mesmo prefixo de data |
| Ter | Wiki health | Eliminar páginas órfãs, corrigir broken links |
| Qua | Stale cleanup | Remover arquivos >30 dias sem referência |
| Qui | Disk/cache | Limpar caches, npm, logs antigos |
| Sex | Config audit | Revisar crons com erro, configs obsoletas |
| Sáb | Cross-refs | Criar links faltantes entre memory e wiki |
| Dom | OFF | Sem experimentos |

---

## 3. MÉTRICAS DE OUTCOME (não de output)

### 3.1 O que NÃO conta como métrica
- "Removi 7 arquivos" → e daí?
- "Reduzi 2.3%" → importa se melhorou recall?
- "Criei diretório" → não é melhoria, é ação

### 3.2 O que CONTA como métrica

**Memory Health Score (0-100):**
```
score = 100
  - (duplicates_by_date * 2)       # cada duplicata pesa 2pts
  - (orphan_files * 3)              # cada órfão pesa 3pts
  - (broken_links * 5)              # cada link quebrado pesa 5pts
  - (empty_dirs * 1)                # cada dir vazio pesa 1pt
  - (files_over_30_days_unreferenced * 0.5)
```
Target: manter score > 80.

**Wiki Health Score (0-100):**
```
score = 100
  - (orphan_pages * 4)
  - (contradictions * 5)
  - (broken_links * 3)
  - (pages_without_sources * 1)
```
Target: manter score > 75.

**Disk Efficiency Score:**
```
score = disk_usage_openclaw_mb / total_files
(número baixo = melhor)
```

### 3.3 Regra de Decisão
- Metric **melhorou** → COMMIT
- Metric **igual ou pior** → REVERT
- Metric **não conseguiu medir** → REVERT + flag "inconclusive"

---

## 4. CONFIGURAÇÃO DO CRON

### 4.1 Agente Único (não 3)

Um cron só, rodando às 03:00 com escopo do dia da semana:

```yaml
name: omega:autoresearch-v2
schedule: "0 3 * * *"  # 03:00 diário
model: zai/glm-4.7    # NÃO flash — precisa raciocinar
timeout: 600           # 10 min
payload:
  message: |
    Você é o AutoResearch Agent v2.
    
    PASSOS OBRIGATÓRIOS (não pule nenhum):
    
    1. IDENTIFIQUE O DIA DA SEMANA e seu domínio:
       - Seg=memory-dedup, Ter=wiki-health, Qua=stale-cleanup,
         Qui=disk-cache, Sex=config-audit, Sáb=cross-refs, Dom=skip
    
    2. COLEHE BASELINE
       Execute: bash /root/.openclaw/autoresearch-v2/templates/metrics-collect.sh
       Salve output como before.json
    
    3. PROPOE UMA HIPÓTESE (só uma)
       - Específica, testável, reversível
       - Exemplo: "Consolidar 9 arquivos do dia 2026-06-30 em 1 arquivo"
       - Defina qual métrica deve melhorar
    
    4. SALVE STASHPOINT
       Execute: cd /root/.openclaw && git stash
    
    5. EXECUTE A MUDANÇA
    
    6. COLEHE MÉTRICAS DEPOIS
       Execute: bash /root/.openclaw/autoresearch-v2/templates/metrics-collect.sh
       Salve output como after.json
    
    7. COMPARE before vs after
    
    8. DECIDA:
       - Melhorou → git add + git commit
       - Piorou/igual → git checkout . (REVERT)
       - Inconclusivo → REVERT + reportar
    
    9. REGISTRE experimento em:
       /root/.openclaw/autoresearch-v2/experiments/YYYY-MM-DD/
    
    REGRA: Nunca reporte "sucesso" sem mostrar before vs after
    da métrica. Se não pode medir, não pode afirmar que melhorou.
```

### 4.2 Custo estimado
- 1 run/dia ao invés de 3
- ~400k tokens/run (glm-4.7 não-flash)
- ~12M tokens/mês
- vs v1: ~950k tokens/dia = ~28M tokens/mês
- **Economia: ~57%** com mais qualidade

---

## 5. CRITÉRIOS PARA EXPANSÃO (outros servidores)

A v2 só será promovedida pra outros servidores quando:

1. **7 dias consecutivos sem REVERT injustificado** (pelo menos 5 COMMITs com melhoria real)
2. **Memory Health Score > 80** sustentado por 7 dias
3. **Wiki Health Score > 75** sustentado por 7 dias
4. **Zero efeitos colaterais** (nenhum arquivo importante removido por engano)
5. **Custo-benefício positivo** (ganhos medíveis > custo de tokens)

Checklist de expansão:
- [ ] v2 rodando 7 dias no Omega
- [ ] 5+ COMMITs justificados
- [ ] Memory Score > 80 (7 dias)
- [ ] Wiki Score > 75 (7 dias)
- [ ] Documentação completa
- [ ] Template adaptável (config.toml por servidor)
- [ ] Denalth aprova

---

## 6. DIFERENÇAS V1 vs V2

| Aspecto | v1 | v2 |
|---------|----|----|
| Agentes | 3 (memory, agents, tools) | 1 com rotação |
| Modelo | glm-4.7-flash (grátis) | glm-4.7 (pago, melhor) |
| Escopo | "Otimize tudo" | 1 hipótese por run |
| Métrica | Output (-X arquivos) | Outcome (Health Score) |
| Loop | Muda → reporta sucesso | Muda → mede → commit/revert |
| Reversão | Nunca reverte | Reverte se não melhorar |
| Custo/dia | ~950k tokens | ~400k tokens |
| Claims | "Acho que melhorou" | before.json vs after.json |
| Schedule | 3 horários | 1 horário (03:00) |

---

## 7. RISCOS E MITIGAÇÕES

| Risco | Probabilidade | Mitigação |
|-------|--------------|-----------|
| Agente remove arquivo importante | Média | git stash antes + git revert automático |
| Métrica melhora mas conteúdo é perdido | Baixa | Wiki Lint valida contradições |
| Custo de tokens do glm-4.7 | Baixa | 1 run/dia, ~400k tokens é controlável |
| Agente não consegue medir | Média | REVERT automático se inconclusivo |
| Falso positivo (métrica gaming) | Baixa | Múltiplas métricas, não uma só |

---

## 8. PRÓXIMOS PASSOS

1. Criar `metrics-collect.sh` (script shell)
2. Testar manualmente o loop completo (1 experimento real)
3. Criar o cron v2
4. Desativar os 3 crons v1
5. Rodar 7 dias
6. Avaliar critérios de expansão

---

*Ω · Denalth — 2026-07-17*
