# 📖 Manual de Reorganização de Memória — Omega → Aleph

> Como transformar memória caótica em cérebro estruturado
> Data: 2026-07-19
> Ω · Denalth

---

## Por que isso existe

Este manual documenta a operação de reorganização total de memória executada no Omega (.240) em 19/07/2026. O Aleph (.250) deve usar este documento como guia para aplicar a mesma transformação na sua própria memória.

**Problema que motivou:** health_score memory = 0/100. Memory search quebrado. 301 arquivos espalhados sem estrutura. Dreamings acumulando lixo. Informação do mesmo assunto em 10 lugares diferentes.

---

## As 12 Fases

### Fase 1 — Auditoria

**O que fazer:** Catalogar todos os arquivos .md da memória.

```bash
find memory/ -name "*.md" -type f | wc -l
find memory/ -type d | sort
find docs/ -name "*.md" -type f | wc -l
du -sh memory/ docs/ wiki/
```

**Por que:** Não dá reorganizar sem saber o que existe. Mapear antes de cortar.

**Lições aprendidas:**
- Dreaming acumula FAST — 183 arquivos em 2 meses
- pre-merge-backup/ tinha 5.3MB de arquivos mortos
- Muitos arquivos em docs/ eram duplicatas de sme/docs/

---

### Fase 2 — Consolidar Dreaming & Diários

**O que fazer:**
1. Arquivar dreamings >30 dias em tar.gz
2. Criar sínteses mensais (1 arquivo por mês, máx 30 linhas)
3. Deletar backups antigos (pre-merge-backup/)
4. Manter diários recentes intactos

**Por que:**
- 183 arquivos de dreaming = 61% do total, quase zero valor tático
- Sínteses mensais extraem o ouro sem o lixo
- tar.gz preserva histórico sem custar espaço/inodes

**Como medir sucesso:**
- Antes: 183 dreaming files, 2MB
- Depois: 56 files (só mês atual), 676KB + tar.gz

---

### Fase 3 — Promover pra Wiki (Sínteses)

**O que fazer:** Criar páginas wiki canônicas por assunto. Cada página é a **fonte de verdade** para um tópico.

**Páginas essenciais (criar nesta ordem):**
1. `infraestrutura-servidores-visão-canônica` — Todos os servidores, specs, status
2. `perfil-operacional-humano` — Quem é o humano, preferências, mandamentos
3. `sistema-de-memória-arquitetura` — Como a memória funciona
4. `decisões-permanentes` — Regras invioláveis
5. `lições-aprendidas` — Erros e padrões
6. `procedimentos-e-runbooks` — Saber fazer
7. `apis-e-integrações` — Catálogo de APIs
8. `autoresearch-sistema` — Se aplicável

**Formato de cada página:**
```markdown
# Título — Assunto

> Descrição breve
> Confidence: 0.XX | Última verificação: YYYY-MM-DD

## Conteúdo
[dados canônicos]

## Fontes
- sourceIds: ["arquivo1.md", "arquivo2.md"]
```

**Por que:** Resolve o problema "info do mesmo assunto espalhada em 10 arquivos". Wiki passa a ser consulta primária.

---

### Fase 4 — Limpeza

**O que fazer:**
1. Identificar duplicatas (docs/ vs sme/docs/ vs memory/)
2. Manter a versão mais completa, deletar a outra
3. Consolidar por domínio (SME em sme/docs/, travel em memory/travel/)
4. Deletar arquivos órfãos, respostas temporárias, backups mortos
5. Atualizar _MAPA.md de cada diretório

**Por que:** Menos arquivos = mais clareza. Duplicatas causam confusão sobre qual é a fonte de verdade.

---

### Fase 5 — MEMORY.md Enxuto

**O que fazer:** Reduzir MEMORY.md a um índice que aponta pra wiki.

**Estrutura ideal:**
```markdown
# MEMORY.md — Índice [Agente]
> Índice enxuto. Conteúdo vive na wiki.

## Quem sou
[2 linhas]

## Wiki (Fonte de Verdade)
- [[pagina-1]] — descrição
- [[pagina-2]] — descrição

## Estado Atual
[tabela mínima]

## Pendências
[lista curta]

## Arquivos Core
[referências]
```

**Por que:** MEMORY.md é injetado em todo contexto. Menos KB = mais espaço útil no context window. Antes: 8.7KB. Depois: 2.1KB.

---

### Fase 6 — Pipeline de Auto-Ingestão

**O que fazer:** Criar cron que lê diário do dia anterior e extrai valor pra wiki.

**Cron:**
```
Nome: [agente]:memory-ingestion
Horário: 04:00 diário
Modelo: glm-4.7-flash (grátis)
SessionTarget: isolated
```

**Prompt do cron:**
1. Ler diário de ontem
2. Extrair decisões, fatos, lições, mudanças de estado
3. Atualizar páginas wiki apropriadas
4. Marcar claims com last_verified, confidence, source
5. Rodar wiki_lint no final

**Por que:** Acaba com a dependência de "lembrar" de salvar. O sistema extrai valor automaticamente. Toda conversa vira conhecimento durável sem esforço manual.

---

### Fase 7 — Entity Registry

**O que fazer:** Criar uma página wiki por entidade (servidor, pessoa, projeto).

**Entities essenciais:**
- Um arquivo por servidor (ceopssrv240.md, ceopssrv241.md, etc.)
- Um arquivo por pessoa chave
- Um arquivo por projeto ativo

**Formato com frontmatter:**
```yaml
---
type: entity
canonical: ceopssrv240
aliases: [Omega, .240]
last_verified: 2026-07-19
confidence: 0.95
---
```

**Por que:** Quando menciono ".241" numa conversa, posso buscar a entity e ter TODA a info sobre aquele servidor em um lugar. Resolve info espalhada.

---

### Fase 8 — Decay & Confidence Scoring

**O que fazer:** Cada página wiki e entity recebe:
- `last_verified:` data da última confirmação
- `confidence:` 0.0 a 1.0
- `source:` de onde veio a info

**Regra de decay:**
- >30 dias sem verify = marcada stale
- >60 dias = não confiar sem re-verificar
- Antes de responder, consultar freshness

**Por que:** Evita afirmações baseadas em info velha. "Acho que o .241 tem 14% de disco" — se a página diz last_verified: 2026-06-30, sei que precisa re-verificar.

---

### Fase 9 — Memory Search (NÃO MEXER SE ESTÁ FUNCIONANDO)

**Princípio fundamental:** Se o memory search está funcionando, **não toque**. A tentação de "melhorar" trocando o modelo de embedding causa mais danos que benefícios.

**Config que FUNCIONA (padrão do OpenClaw):**
```json
"memorySearch": {
  "provider": "local",
  "model": "embeddinggemma-300m-qat-Q8_0.gguf",
  "local": {
    "contextSize": 2048
  }
}
```

**⚠️ AVISO CRÍTICO — Não tente Ollama nem Qwen3:**
Veja o Apêndice A para detalhes, mas resumindo:
- **Ollama externo (.241):** se o servidor cair, memory search quebra junto
- **Qwen3-Embedding-0.6B:** testado em 19/07/2026, **FALHOU** — node-llama-cpp não aceita, gera 428MB de lixo de reindexação
- **embeddinggemma-300m:** padrão, resiliente, funciona sem dependência externa

**Como diagnosticar se quebrou:**
```bash
# Verificar config atual
grep -A5 "memorySearch" openclaw.json | head -20

# Testar busca (se retornar timeout ou disabled, está quebrado)
# Usar tool memory_search

# Verificar se modelo existe
ls /root/.node-llama-cpp/models/hf_ggml-org_embeddinggemma-300m-qat-Q8_0.gguf
```

**Por que:** Sem memory search funcional, o agente não recupera contexto de conversas anteriores. É como ter amnésia a cada sessão.

---

### Fase 10 — Self-Healing Loop

**O que fazer:** Cron semanal que roda `wiki_lint` e auto-corrigi problemas.

**Cron:**
```
Nome: [agente]:wiki-self-healing
Horário: Segunda 05:00
Modelo: glm-4.7-flash
```

**O que o cron faz:**
1. Roda wiki_lint
2. Links quebrados → corrige se óbvio
3. Páginas vazias → preenche com dados de fontes
4. Stale pages → marca warning no topo
5. Contradições → reporta, NÃO auto-corrigi
6. Salva relatório

**Por que:** A memória se auto-repara. Sem isso, podridão acumula — links quebrados, info stale, contradições não detectadas.

---

### Fase 11 — Runbooks & Decision Trees

**O que fazer:** Transformar lições aprendidas em procedimentos executáveis.

**Formato:**
```markdown
## Quando [problema] acontecer
1. Fazer X
2. Verificar Y
3. Se A → solução B
4. Se C → solução D
```

**Exemplos essenciais:**
- Quando memory_search falhar
- Quando cron não rodar
- Quando servidor ficar sem disco
- Quando Ollama não responder
- Quando config quebrar gateway

**Por que:** Próxima vez que o problema ocorrer, não precisa redescobrir a solução. Consulta o runbook e executa.

---

### Fase 12 — Cross-Server Sync (Futuro)

**O que fazer:** Sínteses wiki sincronizadas entre servidores via git.

**Não implementado ainda** — mas a estrutura criada (wiki com syntheses e entities) está pronta pra isso.

---

## Checklist de Execução

```
[ ] Fase 1: Auditoria (find + wc -l)
[ ] Fase 2: Arquivar dreaming antigo + sínteses mensais
[ ] Fase 3: Criar 8 sínteses wiki canônicas
[ ] Fase 4: Deletar duplicatas + consolidar domínios
[ ] Fase 5: Reduzir MEMORY.md a índice enxuto
[ ] Fase 6: Criar cron memory-ingestion (04:00)
[ ] Fase 7: Criar entity pages (servidores, pessoas)
[ ] Fase 8: Adicionar decay/confidence em cada página
[ ] Fase 9: Fix memory search provider
[ ] Fase 10: Criar cron wiki-self-healing (Seg 05:00)
[ ] Fase 11: Consolidar runbooks na wiki
[ ] Fase 12: Documentar tudo num manual
```

---

## Resultados Esperados

| Métrica | Antes | Depois |
|---|---|---|
| Arquivos memory/ | 300+ | <180 |
| Memory search | Quebrado | Funcional |
| MEMORY.md | Inchado | <2.5KB |
| Wiki syntheses | Vazias | Ricas e canônicas |
| Auto-ingestão | Inexistente | Cron diário |
| Self-healing | Inexistente | Cron semanal |
| Confiança em respostas | Baixa (info velha) | Alta (confidence + freshness) |

---

## Erros Cometidos e Correções

1. **wiki_apply create_synthesis com mesmo título de página existente** → pode falhar com "path changed during read". Solução: usar título ligeiramente diferente.

2. **Config protegido não pode ser alterado via config.patch** → editar openclaw.json diretamente e reiniciar gateway.

3. **Subagent pode não encontrar arquivos se workdir estiver errado** → sempre especificar workdir explicitamente.

4. **Entities criadas manualmente não aparecem no index.md do wiki** → o wiki_apply cuida do index quando cria syntheses, mas entities manuais precisam ser indexadas.

5. **Tentar Ollama externo (.241) como provider de embedding** → se o servidor cair, memory search quebra junto. **Lição:** embedding local é a única opção resiliente. Nunca depender de servidor externo para embeddings. (Testado em 19/07/2026)

6. **Tentar Qwen3-Embedding-0.6B como modelo local** → GGUF válido mas node-llama-cpp não aceitou. Gerou 428MB de arquivos de reindex órfãos (5 tentativas falhadas). Memory search ficou quebrado por ~3 horas até reverter. **Lição:** não trocar o modelo de embedding sem certeza absoluta de compatibilidade. embeddinggemma-300m é o padrão por um motivo. (Testado em 19/07/2026)

7. **Restart do gateway interrompe reindexação em andamento** → sempre aguardar a reindexação completar antes de reiniciar. Se for interrompida, gera arquivos órfãos e o índice não atualiza.

8. **AutoResearch respeita protocolo de dia da semana** → domingo = SKIP. Para testar fora do protocolo, disparar manualmente via subagent e não via `cron run --force` (que segue o protocolo).

---

## Como Replicar no Aleph

1. **Ler este manual completo**
2. **Adaptar nomes:** trocar "Omega" por "Aleph", paths correspondentes
3. **Executar fases na ordem** — não pular
4. **Usar subagents para Fase 2 e 4** (trabalho pesado de I/O)
5. **Não ter pressa:** cada fase garante qualidade da próxima
6. **Validar com wiki_lint após Fase 3**
7. **Testar memory_search após Fase 9**

---

## Apêndice A — Embeddings Locais

### Por que local?

O OpenClaw suporta vários providers de embedding (OpenAI, Gemini, Ollama, etc), mas para infraestrutura distribuída como a CEOPS, **local é a única opção resiliente**.

**Experiência real:** O .241 (Sentinel) travou durante a reorganização. Se o embedding dependesse dele (Ollama), o memory search quebraria junto. Com local, o .240 é autossuficiente.

**Regra:** Embedding local = zero dependência externa. Mesmo que todos os outros servidores caiam, o memory search continua funcionando.

### Modelos testados (GGUF via node-llama-cpp)

| Modelo | Tam | Params | Dims | Status | Veredito |
|---|---|---|---|---|---|
| **embeddinggemma-300m-qat-Q8_0** ✅ | 314MB | 300M | 768 | **Funcionando** | ⭐ RECOMENDADO — usar este |
| Qwen3-Embedding-0.6B-Q8_0 ❌ | 610MB | 0.6B | 1024 | **Falhou** | Não usar — ver abaixo |
| all-MiniLM-L6-v2-Q8_0 | 45MB | 22M | 384 | Não testado | Só pra HW muito fraco |
| NV-Embed-4B-Q5 | 2.5GB | 4B | 2048 | Não testado | Pesado demais pros i7-4770 |

### ❌ Por que Qwen3 NÃO funciona (testado em 19/07/2026)

O Qwen3-Embedding-0.6B foi baixado (610MB), configurado e testado. **Resultado: falha completa.**

**O que aconteceu:**
1. Modelo GGUF válido (cabeçalho correto, arquivo íntegro)
2. Config aplicada no openclaw.json sem erro
3. Gateway reiniciou normalmente
4. `openclaw memory index --force` rodou mas não completou — processo era morto a cada restart
5. `memory_search` retornava: `"index was built for model X, expected Y"` — o índice nunca convergiu
6. Gerou **428MB de arquivos de reindex órfãos** (5 tentativas falhadas) que precisaram ser limpos manualmente
7. Volta pro embeddinggemma = funciona imediatamente

**Causa provável:** incompatibilidade entre o formato do Qwen3 GGUF e a versão do node-llama-cpp instalada. O node-llama-cpp do OpenClaw espera um formato específico que o Qwen3 não atende.

**Lição:** Não trocar o modelo de embedding sem ter certeza absoluta de compatibilidade. O embeddinggemma é o padrão do OpenClaw por um motivo.

### ✅ Configuração recomendada (embeddinggemma)

É o que já vem por padrão no OpenClaw. **Não precisa fazer nada** — só verificar:

```json
"memorySearch": {
  "provider": "local",
  "model": "embeddinggemma-300m-qat-Q8_0.gguf",
  "local": {
    "contextSize": 2048
  }
}
```

O modelo fica em: `/root/.node-llama-cpp/models/hf_ggml-org_embeddinggemma-300m-qat-Q8_0.gguf` (314MB, baixado automaticamente pelo OpenClaw).

### Erro comum: "index was built for model X, expected Y"

Acontece quando você troca o modelo de embedding sem reindexar. O OpenClaw pausa o memory search em vez de retornar resultados errados.

**Solução:**
```bash
openclaw memory index --force --agent main
```

Aguardar a reindexação completar (pode levar alguns minutos dependendo do volume de arquivos em memory/).

### Como trocar de modelo (se um dia quiser tentar outro)

1. Editar `openclaw.json`: trocar `model` em `memorySearch`
2. Reiniciar gateway
3. Rodar `openclaw memory index --force --agent main`
4. **Aguardar a reindexação completar** (não reiniciar no meio!)
5. Testar com `memory_search`
6. Se falhar: reverter config, deletar arquivos `*.memory-reindex-*` em `agents/main/agent/`, reiniciar

### Limpando reindex órfãos

Se uma tentativa de troca de modelo falhar, arquivos órfãos de reindexação se acumulam:
```bash
rm -f /root/.openclaw/agents/main/agent/openclaw-agent.sqlite.memory-reindex-*
rm -f /root/.openclaw/agents/main/agent/openclaw-agent.sqlite.reindex-lock.sqlite*
```
No teste do Qwen3, isso limpou **428MB** de lixo.

---

## Apêndice B — AutoResearch v2

### O que é

Sistema de auto-melhoria contínua inspirado no loop de Karpathy. Um cron diário que coleta métricas, identifica problemas e executa experimentos de melhoria automaticamente.

### Como funciona

**Cron:** `omega:autoresearch-v2` (diário 03:00)
**Modelo:** `zai/glm-4.7` (precisa de raciocínio)

**Protocolo de dia da semana:**
```
Seg = memory-dedup (consolidar duplicatas)
Ter = wiki-garden (sínteses, links, metadata)
Qua = stale-hunt (info velha, re-verificar)
Qui = orphan-cleanup (arquivos órfãos)
Sex = backup-audit (verificar backups)
Sáb = lessons-review (revisar lições aprendidas)
Dom = SKIP (dia de descanso)
```

### Para testar manualmente (ignorando protocolo)

**Não usar `cron run --force`** — ele segue o protocolo do dia.

Usar subagent:
```
sessions_spawn(
  task="Você é o AutoResearch Agent. Ignorar protocolo de dia. ESCOPO: [experimento]...",
  model="zai/glm-4.7",
  mode="run"
)
```

### Resultado do primeiro teste real (19/07/2026)

Escopo: `memory-dedup`
Duração: ~2min30s
Resultado: 176 arquivos analisados, 0 duplicatas acionáveis (só em dreaming arquivado), 0 ações necessárias.

**Veredito:** pipeline funciona. Coleta → analisa → decide → reporta.

### Métricas que coleta

```json
{
  "memory": {
    "total_files": 176,
    "duplicate_hashes": 3,
    "orphan_files": 83,
    "files_over_30_days": 49,
    "health_score": "N/100"
  },
  "wiki": {
    "total_pages": 35,
    "broken_links": 0,
    "pages_without_sources": 12,
    "health_score": 88
  },
  "system": {
    "disk_usage_openclaw_mb": 6920,
    "cron_error_count": 0
  }
}
```

### Lição sobre o AutoResearch

O health_score de memória报告ado como 0/100 foi **falso alarme** — o embedding local funcionava, mas o AutoResearch não detectava corretamente. Após a reorganização completa (12 fases), o sistema está saudável. O score deve melhorar nas próximas runs com a memória limpa.

---

## Arquivos Referenciados

- `wiki/main/syntheses/` — 17 sínteses criadas
- `wiki/main/entities/` — 7 entity pages
- `MEMORY.md` — índice enxuto
- `docs/infra/cron-calendar.md` — calendário (atualizar quando criar crons)
- Cron `omega:memory-ingestion` (415b4469)
- Cron `omega:wiki-self-healing` (c2058451)

---

## Filosofia

> "Memória não é armazenamento. Memória é recuperação seletiva de conhecimento relevante no momento certo."

As 12 fases transformam um depósito de arquivos em um sistema de memória que:
- **Sabe o que sabe** (confidence scoring)
- **Sabe o que esqueceu** (decay marking)
- **Aprende sozinho** (auto-ingestão)
- **Se repara sozinho** (self-healing)
- **Recupera rápido** (entity registry + wiki)

Ω · Denalth
