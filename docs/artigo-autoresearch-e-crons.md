# Omega ♾️ — AutoResearch & Sistema de Crons

> Artigo técnico completo sobre o sistema de auto-pesquisa e automações do Omega
> Autor: Ω · Denalth
> Data: 2026-07-23
> Host: ceopssrv240 (172.23.39.240)

---

## Índice

1. [Visão Geral](#1-visão-geral)
2. [AutoResearch — O Que É](#2-autoresearch--o-que-é)
3. [Como o AutoResearch Funciona](#3-como-o-autoresearch-funciona)
4. [Domínios de Otimização](#4-domínios-de-otimização)
5. [Evolução: v1 → v2](#5-evolução-v1--v2)
6. [Resultados Esperados](#6-resultados-esperados)
7. [Sistema de Crons — Visão Geral](#7-sistema-de-crons--visão-geral)
8. [Crons Ativas — Detalhamento Completo](#8-crons-ativas--detalhamento-completo)
9. [Crons Desativadas — Histórico](#9-crons-desativadas--histórico)
10. [Cascata de Modelos](#10-cascata-de-modelos)
11. [Status Atual e Próximos Passos](#11-status-atual-e-próximos-passos)

---

## 1. Visão Geral

O Omega opera como assistente de infraestrutura 24/7 no servidor ceopssrv240. Para manter-se útil sem intervenção humana constante, possui dois sistemas principais de automação:

- **AutoResearch:** Sistema de experimentação com loop fechado que testa hipóteses de otimização, mede resultados e preserva apenas melhorias comprovadas.
- **Cron Jobs:** 11 automações ativas que rodam em horários específicos, cobrindo backup, memória, wiki, segurança e auto-melhoria.

Juntos, esses sistemas formam um **ciclo de melhoria contínua autônomo**: o AutoResearch propõe e testa mudanças, as crons mantêm o sistema rodando, e a auto-melhoria semanal consolida aprendizados.

---

## 2. AutoResearch — O Que É

### Inspiração

O AutoResearch é baseado no padrão **AutoResearch de Andrej Karpathy** (github.com/karpathy/autoresearch) — um framework onde um LLM atua como cientista: formula hipóteses, executa experimentos, mede resultados e decide o que manter.

### Definição

> AutoResearch é um sistema de **otimização autônoma com loop fechado** que:
> 1. Coleta métricas do estado atual (baseline)
> 2. Propõe uma hipótese de melhoria
> 3. Executa a mudança
> 4. Mede o resultado
> 5. **Commit se melhorou, revert se piorou**
> 6. Registra tudo para análise

### Princípio Fundamental

**"Se não pode medir, não pode afirmar que melhorou."**

Cada experimento precisa ter:
- Métrica **antes** (baseline)
- Métrica **depois** (resultado)
- Comparação objetiva
- Decisão baseada em dados

---

## 3. Como o AutoResearch Funciona

### O Loop Universal

Cada execução do AutoResearch segue estes passos:

```
┌─────────────────────────────────────────────────────────┐
│                 AUTORESEARCH — LOOP                       │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  PASSO 1: COLETAR BASELINE                              │
│  ├── Métricas atuais do sistema                         │
│  ├── Estado dos arquivos, configs, crons                │
│  └── Salva como "antes"                                 │
│                                                         │
│  PASSO 2: PROPOR HIPÓTESE                               │
│  ├── Analisa métricas com raciocínio de LLM             │
│  ├── Formula UMA hipótese específica e testável         │
│  └── Exemplo: "Consolidar 3 arquivos duplicados         │
│      reduz duplicates_by_date"                          │
│                                                         │
│  PASSO 3: CHECKPOINT                                    │
│  ├── git stash (permite reverter)                       │
│  └── Ponto seguro de retorno                            │
│                                                         │
│  PASSO 4: EXECUTAR MUDANÇA                              │
│  ├── Aplica a alteração real nos arquivos               │
│  └── Escopo cirúrgico (uma coisa por vez)               │
│                                                         │
│  PASSO 5: MEDIR DEPOIS                                  │
│  ├── Coleta as mesmas métricas                          │
│  └── Salva como "depois"                                │
│                                                         │
│  PASSO 6: COMPARAR                                      │
│  ├── Métrica melhorou? → COMMIT                         │
│  ├── Métrica piorou? → REVERT                           │
│  └── Inconclusivo? → REVERT + registrar                 │
│                                                         │
│  PASSO 7: REGISTRAR EXPERIMENTO                         │
│  ├── hypothesis.md com antes/depois/verdict             │
│  ├── Log em CSV para análise histórica                  │
│  └── Git commit com mensagem descritiva                 │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### Segurança

- **Uma hipótese por run** — escopo cirúrgico
- **Git versiona tudo** — qualquer mudança é reversível
- **Se piorar, reverte automaticamente**
- **Checkpoint antes de cada experimento**
- **Modelos nunca usam flashx** (removido do allowlist)

---

## 4. Domínios de Otimização

O AutoResearch opera em três frentes independentes:

### Memory AutoResearch 🧠
**O que otimiza:**
- Chunk size (512 vs 1024 vs 2048)
- Embedding model (text-embedding-3-small vs nomic-embed-text)
- Retrieval strategy (semantic vs hybrid vs keyword)
- Compression ratio

**Métricas:**
- LoCoMo score (alvo: >90%)
- LongMemEval score (alvo: >93%)
- Token savings (alvo: >80%)
- Maintenance time (alvo: <5s)

### Agents AutoResearch 🤖
**O que otimiza:**
- Configurações de crons (prompts, modelos, horários)
- Limpeza de sessões órfãs
- Otimização de workspace
- Redistribuição de horários conflitantes

**Métricas:**
- Task success rate (alvo: >95%)
- Crons com erro (alvo: 0)
- Workspace size (alvo: <50MB)
- Latência de resposta (alvo: <10s)

### Tools AutoResearch 🔧
**O que otimiza:**
- Scripts e skills
- Limpeza de caches
- Documentação de skills
- Config subótima

**Métricas:**
- Tool success rate (alvo: >95%)
- Latency (alvo: <10s)
- Error recovery rate (alvo: >90%)
- Disk usage (alvo: estável)

---

## 5. Evolução: v1 → v2

### v1 (13/07/2026) — Runners Shell

A primeira versão usava scripts shell (`memory.sh`, `agents.sh`, `tools.sh`) que:
- Geravam hipóteses com variação controlada
- Rodavam em git branches separadas
- Faziam commit/revert automático
- Registravam métricas em CSV

**Resultado dos testes iniciais:**

| Runner | Baseline | Resultado | Variação | Ação |
|---|---|---|---|---|
| Memory | 92% | 96% | +4% | ✅ Commit |
| Agents | 94% | 92% | -2% | ❌ Revert |
| Tools | 93% | 93% | 0% | ❌ Revert |

Os três funcionaram corretamente — melhorias foram preservadas, regressões foram revertidas.

### v1 Cron (Agent-based)

Depois da v1 shell, foi criada a versão **cron-based** com três agentes:
- `omega:autoresearch-memory` — 03:00 diário
- `omega:autoresearch-agents` — 04:00 diário
- `omega:autoresearch-tools` — 05:00 diário

Estes usavam LLM para propor hipóteses mais inteligentes (não apenas variação aleatória).

### v2 (15/07/2026) — Escopo por Dia da Semana

A v2 unificou tudo em uma única cron `omega:autoresearch-v2` com escopo rotativo:

| Dia | Escopo |
|---|---|
| Segunda | memory-dedup (consolidar duplicados) |
| Terça | wiki-health (links quebrados, pages sem source) |
| Quarta | stale-cleanup (arquivos >30 dias sem referência) |
| Quinta | disk-cache (npm cache, logs antigos, sessões órfãs) |
| Sexta | config-audit (crons com erro, configs obsoletas) |
| Sábado | cross-refs (links faltantes entre memory e wiki) |
| Domingo | SKIP (dia de descanso) |

### O Que a v2 Melhorou e Por Quê

A v1 funcionava, mas tinha problemas práticos que a v2 resolveu:

**1. Métricas padronizadas via script único (`metrics-collect.sh`)**
- **Problema v1:** Cada runner (memory.sh, agents.sh, tools.sh) coletava métricas de forma diferente, com scripts separados e inconsistentes. Comparar resultados entre domínios era difícil.
- **Solução v2:** Um único script coleta todas as métricas relevantes em formato JSON padronizado. Antes e depois usam o mesmo formato → comparação confiável.
- **Impacto:** Resultados comparáveis e auditáveis.

**2. Checkpoint com `git stash` antes de cada experimento**
- **Problema v1:** O revert era feito via `git revert` (cria novo commit desfazendo). Se múltiplos experimentos rodassem na mesma noite, os reverts se acumulavam e poluíam o histórico.
- **Solução v2:** `git stash` antes de experimentar. Se piorou, `git checkout .` volta ao estado exato anterior sem criar commits de revert.
- **Impacto:** Histórico git limpo, reversão garantida e atômica.

**3. Escopo por dia da semana (em vez de 3 runs/night)**
- **Problema v1:** Três crons rodando toda noite (03:00, 04:00, 05:00) consumiam 3 chamadas de LLM por dia na madrugada — justo no pico de uso asiático (14:00-18:00 Pequim). Frequentemente batiam rate limit.
- **Solução v2:** Uma única cron por dia, cada dia com um foco diferente (Seg=memory, Ter=wiki, Qua=stale, Qui=disk, Sex=config, Sáb=cross-refs, Dom=skip). Mesmo trabalho semanal, mas distribuído.
- **Impacto:** 70% menos chamadas por noite, sem rate limit.

**4. Decisão baseada em before vs after real (não estimada)**
- **Problema v1:** Os runners shell geravam números semi-aleatórios para simular métricas (ex: LoCoMo subiu de 92% para 96%). Não eram métricas reais do sistema.
- **Solução v2:** O script `metrics-collect.sh` mede o estado REAL do sistema (conta arquivos, mede tamanhos, verifica erros). A hipótese é testada contra dados reais.
- **Impacto:** Experimentos têm validade real. "Consolidar 3 arquivos duplicados" realmente reduz a contagem de duplicados — mensurável.

**5. Registro estruturado em `experiments/YYYY-MM-DD/hypothesis.md`**
- **Problema v1:** Logs em CSV e `.log` planos. Difícil saber qual experimento gerou qual resultado. Sem contexto da hipótese.
- **Solução v2:** Cada experimento tem seu próprio diretório com `hypothesis.md` contendo: data, escopo, hipótese, before.json, after.json, verdict e justificativa.
- **Impacto:** Auditoria completa — qualquer experimento pode ser revisitado.

### Estado Atual

As três crons v1 estão **desativadas** (foram substituídas pela v2). A v2 também está **temporariamente desativada** devido a erros de lifecycle claim (conflito de sessão) — precisa ajuste técnico.

---

## 6. Resultados Esperados

### Curto Prazo (7 dias)
- 3 execuções por noite (v1) ou 6 por semana (v2)
- ~21 experimentos/semana
- Git branches para cada experimento
- Métricas CSV atualizadas diariamente

### Médio Prazo (1 mês)
- ~90 experimentos acumulados
- Histórico rico de otimizações
- Padrões identificados (o que funciona vs o que não)
- Melhorias acumuladas no ecossistema

### Longo Prazo (3 meses)
- ~270 experimentos
- Sistema auto-otimizado continuamente
- Knowledge base de melhores configurações
- Performance otimizada em todos os domínios

### Meta Final

Um sistema que **se mantém saudável sem intervenção humana**, onde:
- Crons falhando são auto-corrigidas
- Workspace cresce controlado
- Wiki stays atualizada e sem contradições
- Memória é curada e promovida automaticamente
- Configs obsoletas são detectadas e corrigidas

---

## 7. Sistema de Crons — Visão Geral

O Omega possui **11 crons ativas** que rodam automaticamente. Elas estão divididas em categorias:

| Categoria | Crons | Função |
|---|---|---|
| **Backup** | 2 | Git autosave + docs sync |
| **Memória** | 3 | Ingestão, dreaming, wiki intelligence |
| **Wiki** | 2 | Indexação incremental + self-healing |
| **Manutenção** | 2 | Auto-melhoria + docs review |
| **Segurança** | 1 | Audit quinzenal |
| **AutoResearch** | 1 | Experimentação (desativada) |

### Coordenação Entre Servidores

O Omega (.240) compartilha a API key ZAI com Aleph (.250), Voyager (.251) e SME (.249). Para evitar rate limits:
- Mínimo 20min entre crons de servidores diferentes
- Calendário unificado em `docs/cron-calendar.md`
- Cascata de modelos distribui carga

---

## 8. Crons Ativas — Detalhamento Completo

### 8.1 — omega:git-autosave 📦

| Campo | Valor |
|---|---|
| **Horário** | 08:50 e 22:50 (2x/dia) |
| **Modelo** | 4.5-flash (grátis) |
| **Fallback** | 4.7 |
| **Timeout** | 300s |
| **Delivery** | Silencioso |

**O que faz:**
Executa `git add -A && git commit && git push` no repositório `/root/.openclaw/`. Se não houver mudanças, apenas reporta "no changes".

**Objetivo:**
Garantir que todo o workspace, configurações e memórias sejam versionados e enviados para o GitHub backup duas vezes ao dia.

**Resultado esperado:**
- Zero perda de dados em caso de falha de hardware
- Histórico versionado de todas as mudanças
- Tags semanais para recuperação pontual

**Status:** ✅ Funcionando

---

### 8.2 — omega:docs-sync 📄

| Campo | Valor |
|---|---|
| **Horário** | 08:50 e 22:50 (2x/dia) |
| **Modelo** | 4.5-flash (grátis) |
| **Fallback** | 4.7 |
| **Timeout** | 120s |
| **Tools** | exec apenas |
| **Delivery** | Silencioso |

**O que faz:**
Executa o script `/root/.openclaw/workspace/scripts/sync-omega-docs.sh` que sincroniza documentos entre o workspace e o repositório de backup.

**Objetivo:**
Manter docs/ sincronizado e atualizado, garantindo que mapas, procedimentos e referências estejam sempre no backup.

**Resultado esperado:**
- Docs sempre atualizadas no GitHub
- Sem documentação órfã ou desatualizada

**Status:** ✅ Funcionando

---

### 8.3 — Incremental Wiki Indexer (Midday) 📋

| Campo | Valor |
|---|---|
| **Horário** | 08:50, 13:50 e 18:50 (3x/dia) |
| **Modelo** | 4.5-flash (grátis) |
| **Thinking** | low |
| **Timeout** | padrão |
| **Delivery** | Silencioso |

**O que faz:**
Passo leve de indexação wiki:
1. Lê o daily note do dia (`memory/YYYY-MM-DD.md`)
2. Verifica `memory/context/`
3. Extrai fatos novos e aplica na wiki com confidence score
4. Checa contradições rápidas
5. Atualiza `decisions.md` e `pending.md` se necessário

**Objetivo:**
Manter a wiki viva e atualizada ao longo do dia, capturando mudanças assim que acontecem — sem esperar o passo pesado noturno.

**Resultado esperado:**
- Wiki com claims atualizadas em tempo quase real
- Contradições detectadas cedo
- Contexto sempre fresco para próximas conversas

**Status:** ⚠️ Última run interrompida por gateway restart

---

### 8.4 — Wiki Intelligence — Daily Pass 🧠

| Campo | Valor |
|---|---|
| **Horário** | 01:31 diário |
| **Modelo** | 4.6 (pago, Tier 3) |
| **Thinking** | low |
| **Timeout** | padrão |
| **Delivery** | Silencioso |

**O que faz:**
Passo profundo de inteligência wiki — o "cérebro noturno" do sistema de memória:
1. **Indexação incremental:** lê daily notes de ontem e hoje, extrai fatos/decisões/lições/mudanças de estado
2. **Detecção de contradições:** roda `wiki_lint`, busca claims contraditórios, marca conflitos
3. **Scoring de confiança:** 1.0 (confirmado) → 0.8-0.9 (logs) → 0.6-0.7 (inferido) → 0.4-0.5 (incerto)
4. **Decaimento temporal:** claims não acessados em 30+ dias perdem 0.1 por 30 dias (mínimo 0.2)
5. **Auto-configuração:** atualiza MEMORY.md, decisions.md, pending.md se houver mudanças significativas

**Objetivo:**
Manter a wiki como fonte canônica de conhecimento, com qualidade de dados controlada e decaimento natural de informação obsoleta.

**Resultado esperado:**
- Wiki rica, atualizada e sem contradições
- Confiança calibrada por evidência
- Informação obsoleta decai naturalmente
- MEMORY.md enxuto (<100 linhas)

**Status:** ✅ Funcionando (última run: 119s, sucesso)

---

### 8.5 — omega:memory-ingestion 📥

| Campo | Valor |
|---|---|
| **Horário** | 02:51 diário |
| **Modelo** | 4.7-flash (grátis) |
| **Fallback** | 4.5-flash |
| **Timeout** | 600s |
| **Delivery** | Anuncia no Telegram |

**O que faz:**
Pipeline de auto-ingestão de memória:
1. Lê o diário do dia anterior
2. Extrai: decisões importantes, fatos novos, lições aprendidas, mudanças de estado
3. Para cada item com valor durável, atualiza a wiki correspondente:
   - Infra → `infraestrutura-ceops-visão-canônica`
   - Decisão → `decisões-permanentes-omega`
   - Lição → `lições-aprendidas-omega`
   - Procedimento → `procedimentos-e-runbooks-omega`
4. Marca cada claim com `last_verified`, `confidence`, `source`
5. Detecta contradições e roda `wiki_lint`
6. Reporta o que foi promovido

**Objetivo:**
Transformar o fluxo diário de informações brutas em conhecimento estruturado e durável na wiki. É a ponte entre "o que aconteceu" e "o que o sistema sabe".

**Resultado esperado:**
- Daily notes processadas todas as noites
- Conhecimento promovido para wiki canônica
- Decisões e lições preservadas com evidência
- Zero perda de contexto importante

**Status:** ❌ Falhando (5 erros consecutivos — timeout nos modelos 4.7-flash e 4.5-flash durante madrugada)

---

### 8.6 — Memory Dreaming Promotion 💭

| Campo | Valor |
|---|---|
| **Horário** | 03:00 diário |
| **Modelo** | memory-core (interno) |
| **Delivery** | Silencioso |

**O que faz:**
Sistema gerenciado pelo plugin `memory-core`. Promove recalls de curto prazo (short-term memories) que atingem critérios rigorosos para a memória de longo prazo (MEMORY.md):
- Score mínimo: 0.800
- Recall count mínimo: 3
- Queries únicas mínimas: 3
- Meia-vida de recência: 14 dias
- Idade máxima: 30 dias
- Limite: 10 entradas por run

**Objetivo:**
Curadoria automática — apenas informações que foram acessadas múltiplas vezes, por queries diferentes, com alta relevância, ganham espaço na memória permanente.

**Resultado esperado:**
- MEMORY.md contém apenas informação de alto valor
- Ruído é filtrado naturalmente
- Memória permanece enxuta e útil

**Status:** ✅ Funcionando

---

### 8.7 — omega:auto-melhoria 🔄

| Campo | Valor |
|---|---|
| **Horário** | 07:00 todo sábado |
| **Modelo** | 4.6 (pago, Tier 3) |
| **Fallback** | 4.7 |
| **Timeout** | 600s |
| **Delivery** | Anuncia no Telegram |

**O que faz:**
Self-review semanal obrigatória:
1. Lista arquivos `memory/` da semana
2. Revisa MEMORY.md e identifica 3 aprendizados principais
3. Verifica se aprendizados foram aplicados em AGENTS.md, TOOLS.md ou IDENTITY.md
4. Atualiza MEMORY.md (remove obsoleto, promove novo)
5. Salva relatório em `memory/auto-melhoria.md`

**Objetivo:**
Garantir que o sistema evolua de forma estruturada. Toda semana, revisar o que aprendeu, consolidar em arquivos core e remover informação obsoleta.

**Resultado esperado:**
- Documentação sempre sincronizada com realidade
- Aprendizados da semana capturados e aplicados
- MEMORY.md enxuto e relevante
- Relatório semal para acompanhamento

**Status:** ✅ Funcionando (última run: 89s, entregue no Telegram)

---

### 8.8 — omega:docs-review 📝

| Campo | Valor |
|---|---|
| **Horário** | 03:51 todo domingo |
| **Modelo** | 4.7-flash (grátis) |
| **Fallback** | 4.5-flash |
| **Timeout** | 300s |
| **Delivery** | Anuncia no Telegram |

**O que faz:**
Revisão estrutural de `docs/`:
1. Verifica arquivos órfãos (sem referência)
2. Busca duplicatas
3. Identifica conteúdo desatualizado
4. Corrige links quebrados
5. Atualiza `_MAPA.md` se desatualizado
6. Não deleta nada sem confirmação

**Objetivo:**
Manter `docs/` organizado e navegável. É a revisão semanal que garante que a documentação não apodrece.

**Resultado esperado:**
- Docs sempre navegáveis
- Links funcionando
- Mapa de arquivos atualizado
- Zero órfãos acumulando

**Status:** ⚠️ Última run com rate limit (2 erros consecutivos)

---

### 8.9 — omega:wiki-self-healing 🩹

| Campo | Valor |
|---|---|
| **Horário** | 03:21 toda segunda |
| **Modelo** | 4.7-flash (grátis) |
| **Fallback** | 4.5-flash |
| **Timeout** | 300s |
| **Delivery** | Anuncia no Telegram |

**O que faz:**
Self-healing loop da wiki:
1. Roda `wiki_lint` e analisa resultados
2. Para cada issue:
   - **Contradição** → não corrige, apenas reporta
   - **Link quebrado** → corrige se óbvio (typo, path errado)
   - **Página vazia** → preenche com dados de fontes confiáveis
   - **Stale page** (>60 dias sem update) → marca com warning
3. Verifica se entities de servidores estão atualizadas
4. Verifica links do MEMORY.md
5. Verifica `_MAPA.md` dos diretórios
6. Salva relatório em `wiki/main/reports/self-healing-YYYY-MM-DD.md`

**Objetivo:**
Wiki saudável sem intervenção humana. Auto-corrigir o seguro, sinalizar o complexo para Denalth.

**Resultado esperado:**
- Wiki sem links quebrados
- Páginas vazias preenchidas
- Contradições sinalizadas
- Stale pages marcadas

**Status:** ⚠️ Última run com API overloaded (4 erros consecutivos)

---

### 8.10 — omega:security-audit 🔒

| Campo | Valor |
|---|---|
| **Horário** | 04:41 nos dias 1 e 15 do mês |
| **Modelo** | 4.7-flash (grátis) |
| **Fallback** | 4.5-flash |
| **Timeout** | 300s |
| **Delivery** | Anuncia no Telegram |

**O que faz:**
Audit de segurança quinzenal:
1. Verifica UFW (firewall ativo)
2. Checa tentativas de SSH suspeitas (últimas 24h)
3. Lista serviços expostos
4. Verifica updates pendentes (`apt list --upgradable`)
5. Relata anomalias

**Objetivo:**
Detecção proativa de problemas de segurança. Bi-semanal para equilibrar vigilância com consumo de API.

**Resultado esperado:**
- Firewall sempre ativo
- Tentativas de intrusão detectadas
- Sistema atualizado
- Anomalias reportadas imediatamente

**Status:** ⚠️ Última run interrompida por gateway restart (erro histórico de modelo no allowlist já corrigido)

---

## 9. Crons Desativadas — Histórico

### omega:autoresearch-memory (v1) — DESATIVADA
- **Horário antigo:** 03:00 diário
- **Modelo:** 4.7-flashx (removido do allowlist)
- **Motivo:** Substituída pela v2 unificada + flashx não funciona mais

### omega:autoresearch-agents (v1) — DESATIVADA
- **Horário antigo:** 04:00 diário
- **Modelo:** 4.7-flashx
- **Motivo:** Mesmo motivo acima

### omega:autoresearch-tools (v1) — DESATIVADA
- **Horário antigo:** 05:00 diário
- **Modelo:** 4.7-flashx
- **Motivo:** Mesmo motivo acima

### omega:autoresearch-v2 — DESATIVADA (temporário)
- **Horário:** 00:11 diário
- **Modelo:** 4.7-flash + fallback 4.7
- **Motivo:** `CronSessionLifecycleClaimError` — conflito de sessão (8 erros consecutivos). Precisa ajuste técnico para reativar.

### watchdog-crons — DESATIVADA
- **Horário antigo:** variável
- **Motivo:** Timeout recorrente, função absorvida por outras crons

### atlas:git-autosave — DESATIVADA
- **Motivo:** Atlas offline, será reativado quando voltar

---

## 10. Cascata de Modelos

Para evitar rate limits e distribuir custo, as crons usam uma cascata de modelos ZAI:

| Tier | Modelo | Custo | Quota | Uso |
|---|---|---|---|---|
| Heartbeat | 4.5-flash | GRÁTIS | própria | Checagens leves |
| Tier 1 | 4.5-flash | GRÁTIS | própria | Crons burras/scripts |
| Tier 2 | 4.7-flash | GRÁTIS | própria | Crons médias |
| Tier 3 | **4.6** | PAGO | independente | Crons pesadas |
| Conversa | 5.2 | PAGO | própria | Chat com Denalth |

### Regras da Cascata

1. **Modelos grátis não consomem quota** do Coding Plan
2. **4.6 tem quota independente do 4.7** — pode rodar em paralelo
3. **❌ flashx REMOVIDO** — não incluído no Coding Plan
4. **Quota renova a cada 5h** (janela)
5. **Pico 03:00-07:00** Fortaleza = 14:00-18:00 Pequim → multiplier 2-3x em 5.1/5-Turbo (4.7 e 4.6 sempre 1x)

### Distribuição Atual por Modelo

**4.5-flash (grátis):** git-autosave, docs-sync, Incremental Wiki
**4.7-flash (grátis):** memory-ingestion, docs-review, wiki-self-healing, security-audit
**4.6 (pago):** Wiki Intelligence, auto-melhoria, (AutoResearch v2 quando reativada)

---

## 11. Status Atual e Próximos Passos

### Saúde do Sistema (23/07/2026)

| Componente | Status | Nota |
|---|---|---|
| git-autosave | ✅ OK | Rodando 2x/dia |
| docs-sync | ✅ OK | Rodando 2x/day |
| Incremental Wiki | ⚠️ 1 erro | Interrompido por restart |
| Wiki Intelligence | ✅ OK | Passo noturno funcionando |
| Memory Ingestion | ❌ Falhando | 5 erros consecutivos (timeout) |
| Dreaming Promotion | ✅ OK | Rodando diariamente |
| Auto-melhoria | ✅ OK | Semanal aos sábados |
| Docs Review | ⚠️ Rate limit | 2 erros consecutivos |
| Wiki Self-healing | ⚠️ Overloaded | 4 erros consecutivos |
| Security Audit | ⚠️ Restart | Interrompido por restart |
| AutoResearch v2 | ❌ Desativado | Lifecycle claim error |

### Problemas Conhecidos

1. **memory-ingestion:** Timeouts consistentes às 03:00 da madrugada — possivelmente sobrecarga da API ZAI no horário de pico asiático
2. **AutoResearch v2:** `CronSessionLifecycleClaimError` indica que a sessão está sendo modificada durante o startup — precisa fix técnico
3. **Rate limits:** Crons Tier 2 (4.7-flash) sofrendo rate limit periódico — considerar migrar mais para Tier 1 ou Tier 3

### Próximos Passos

1. **Investigar timeout do memory-ingestion** — possivelmente mover horário ou trocar para Tier 3 (4.6)
2. **Corrigir AutoResearch v2** — ajustar lifecycle da sessão cron
3. **Considerar 4.6 para mais crons** — quota independente, mais estável
4. **Reativar AutoResearch** quando fix estiver pronto — é o laboratório principal

---

## Conclusão

O sistema AutoResearch + Crons forma um **ecossistema de auto-manutenção** que permite ao Omega operar 24/7 com mínima intervenção humana. O AutoResearch é o cientista que experimenta e otimiza; as crons são os operadores que mantêm a rotina.

A cascata de modelos garante que custos sejam controlados enquanto rate limits são respeitados. E a filosofia fundamental — "medir antes de afirmar" — assegura que apenas melhorias comprovadas sejam preservadas.

É um sistema vivo, em evolução constante, aprendendo com cada erro e cada sucesso.

---

*Ω · Denalth — 2026-07-23*
