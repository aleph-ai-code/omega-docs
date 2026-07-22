# Cron Calendar — Frota Completa (VERIFICADO 2026-07-21 16:24)

**Última verificação real:** 2026-07-21 16:24 GMT-3
**Autor:** Aleph א
**Modelos:** 4.5-flash (GRÁTIS) · 4.7-flash (GRÁTIS) · 4.6 (PAGO quota independente) · 4.7 (PAGO)

---

## Cascata de Modelos ZAI (Nova — 2026-07-21)

| Tier | Modelo | Custo | Quota | Uso |
|------|--------|-------|-------|-----|
| Heartbeat | 4.5-flash | GRÁTIS | própria | Checagens 30min |
| Tier 1 | 4.5-flash | GRÁTIS | própria | Crons burras/scripts |
| Tier 2 | 4.7-flash | GRÁTIS | própria | Crons médias |
| Tier 3 | **4.6** | PAGO | **independente do 4.7** | Crons pesadas |
| Conversa | 5.2 | PAGO | própria | Chat com usuário |

**⚠️ flashx REMOVIDO** — não incluído no Coding Plan, causa "Insufficient balance"

### Modelos ZAI no Allowlist (todos os servidores)
- zai/glm-5.2 — conversa
- zai/glm-5.1
- zai/glm-5-turbo
- zai/glm-4.7
- zai/glm-4.6 — Tier 3 (quota independente)
- zai/glm-4.7-flash — Tier 2 (grátis)
- zai/glm-4.5-flash — Tier 1 (grátis)
- zai/glm-4.5-air — leve incluso no plano

### Coding Plan — Quota
- Renova a cada 5h (janela de 5h)
- Pico: 03:00-07:00 Fortaleza (14:00-18:00 Pequim) — 2-3x multiplier em 5.1/5-Turbo
- 4.7 e 4.6 sempre 1x (sem multiplier)
- Modelos GRATUITOS (4.7-flash, 4.5-flash) não consomem quota

---

## 250 — ALEPH (28 crons)

| Horário | Cron | Modelo | Tier | Status |
|---------|------|--------|------|--------|
| 01:17 | aleph:autoresearch-v2 | 4.7-flash | T2 | ✅ |
| 01:37 | contextpruning-monitor | 4.7-flash | T2 | ✅ |
| 03:00 | Memory Dreaming Promotion | memory-core | — | ✅ |
| 04:17 | session-cleanup-weekly (dom) | 4.7-flash | T2 | ✅ |
| 05:30 | Wiki Intelligence Daily | **4.6** | T3 | ✅ |
| 06:10 | Lembrete Inteligente (seg-sex) | 4.7-flash | T2 | ✅ |
| 06:15 | health-check-diario (seg-sex) | 4.7-flash | T2 | ✅ |
| 06:30 | git-autosave | systemEvent | — | ✅ |
| 07:00 | Relatório Diário Gmail | systemEvent | — | ✅ |
| 08:00 | Rascunhos de Resposta (seg-sex) | 4.7-flash | T2 | ✅ |
| 08:00 | Calendar Sync (seg-sex) | 4.7-flash | T2 | ✅ |
| 08:15 | Wiki Vault Sync | systemEvent | — | ✅ |
| 08:30 | sync-aleph-docs | systemEvent | — | ✅ |
| 09:00 | Extracao de Tarefas (seg-sex) | **4.6** | T3 | ✅ |
| 09:10 | Incremental Wiki (3x/dia) | 4.7-flash | T2 | ✅ |
| 12:15 | Triagem Inbox (2x/dia) | 4.7-flash | T2 | ✅ |
| 14:00 | Monitor GOG OAuth | 4.7-flash | T2 | ✅ |
| 19:00 | Newsletter Aggregator | 4.7-flash | T2 | ✅ |
| 19:00 | Resumo Semanal (dom) | 4.7-flash | T2 | ✅ |
| 21:00 | Relatório Financeiro (sex) | 4.7-flash | T2 | ✅ |
| 22:30 | Backup Diario Drive | 4.7-flash | T2 | ✅ |
| Sábado 10:00-10:20 | Graphify Weekly (5x escalonado) | systemEvent | — | ✅ |
| Sábado 11:00 | auto-melhoria-semanal | **4.6** | T3 | ✅ |
| Sábado 19:00 | Contato Inteligente | 4.7-flash | T2 | ✅ |

## 240 — OMEGA (11 crons)

| Horário | Cron | Modelo | Tier | Status |
|---------|------|--------|------|--------|
| 03:10 | omega:autoresearch-v2 | **4.6** | T3 | ✅ |
| 03:40 | Memory Dreaming Promotion | memory-core | — | ✅ |
| 04:00 | omega:memory-ingestion | 4.7-flash | T2 | ✅ |
| 04:15 | Wiki Intelligence Daily | **4.6** | T3 | ✅ |
| 05:00 (seg) | omega:wiki-self-healing | 4.7-flash | T2 | ✅ |
| 07:00 (sáb) | omega:auto-melhoria | **4.6** | T3 | ✅ |
| 07:00 (dom) | omega:docs-review | 4.7-flash | T2 | ✅ |
| 07:00 (1/15 mês) | omega:security-audit | 4.7-flash | T2 | ✅ |
| 08:50 | omega:git-autosave (2x/dia) | 4.7-flash | T2 | ✅ |
| 08:50 | omega:docs-sync (2x/dia) | 4.7-flash | T2 | ✅ |
| 13:50 | Incremental Wiki (3x/dia) | 4.7-flash | T2 | ✅ |

## 249 — SME (9 crons)

| Horário | Cron | Modelo | Tier | Status |
|---------|------|--------|------|--------|
| 03:10 | Memory Dreaming Promotion | memory-core | — | ✅ |
| 03:15 | server-249:dreaming | memory-core | — | ✅ |
| 03:45 | sme:autoresearch-v2 | 4.7-flash | T2 | ✅ |
| 04:30 | Wiki Intelligence Daily | **4.6** | T3 | ✅ |
| a cada 1h | GitHub Repositories Report | systemEvent | — | ✅ |
| a cada 30m | heartbeat | systemEvent | — | ✅ |
| 09:40 | Incremental Wiki (3x/dia) | 4.7-flash | T2 | ✅ |
| 00:00 (4x/dia) | GLPI Mirror | 4.7-flash | T2 | ✅ |
| 10:00 | GitHub Backup Verification | systemEvent | — | ✅ |

## 251 — VOYAGER (12 crons)

| Horário | Cron | Modelo | Tier | Status |
|---------|------|--------|------|--------|
| 03:15 | voyager:dreaming | memory-core | — | ✅ |
| 03:20 | Memory Dreaming Promotion | memory-core | — | ✅ |
| 03:30 | voyager:autoresearch-v2 | 4.7-flash | T2 | ✅ |
| 05:00 | Wiki Intelligence Daily | **4.6** | T3 | ✅ |
| 05:00 | travel:monitor-precos | 4.7-flash | T2 | ✅ |
| 06:00 (seg) | travel:docs-check | 4.7-flash | T2 | ✅ |
| 08:15 | travel:git-autosave | 4.7-flash | T2 | ✅ |
| 09:00 | travel:monitor-promos | 4.7-flash | T2 | ✅ |
| 09:20 | Incremental Wiki (3x/dia) | 4.7-flash | T2 | ✅ |
| 11:00 | Watchdog | 4.7-flash | T2 | ✅ |
| 15:30 | travel:atualiza-cambio | 4.7-flash | T2 | ✅ |
| Dias 1/15 06:00 | Security Audit | 4.7-flash | T2 | ✅ |

---

## Distribuição por Modelo (pós-cascata 4.6)

### glm-4.5-flash (GRÁTIS)
- Heartbeats (todos os servidores)

### glm-4.7-flash (GRÁTIS — Tier 2)
- Todas as crons médias de todos os servidores
- git-autosave, docs-sync, Incremental Wiki, monitor-precos, etc.

### glm-4.6 (PAGO — quota independente do 4.7 — Tier 3)
- **250:** Wiki Intelligence, auto-melhoria, Extração de Tarefas
- **240:** autoresearch-v2, Wiki Intelligence, auto-melhoria
- **249:** Wiki Intelligence
- **251:** Wiki Intelligence

### systemEvent (sem LLM)
- git-autosave, sync-docs, Graphify, heartbeat, GitHub checks

---

## Dreaming Stagger (memory-core)
| Servidor | Horário | Confirmado |
|----------|---------|------------|
| 250 | 03:00 | ✅ |
| 249 | 03:10 | ✅ |
| 251 | 03:20 (voyager:dreaming 03:15) | ✅ |
| 240 | 03:40 | ✅ |

## Correções Aplicadas 2026-07-21 16:24
1. **TODOS:** flashx removido do allowlist (causa "Insufficient balance")
2. **TODOS:** glm-4.6 adicionado (quota independente do 4.7)
3. **TODOS:** glm-4.5-air adicionado (incluso no plano, 1x sempre)
4. **TODOS:** Wiki Intelligence 4.7 → **4.6** (Tier 3, quota independente)
5. **TODOS:** auto-melhoria 4.7/flashx → **4.6** (Tier 3)
6. **240:** autoresearch-v2 4.7 → **4.6**
7. **250:** Extração de Tarefas 4.7 → **4.6**
8. **TODOS flashx:** revertido para 4.7-flash (grátis)
9. **SHARED-KNOWLEDGE.md** propagado para 240, 249, 251

## Correções Anteriores (2026-07-20)
1. **250:** autoresearch-v2 movido de 03:30 → 01:17 (fora da janela dreaming)
2. **250:** session-cleanup movido de 04:30 → 04:17 (dom)
3. **250:** contextpruning-monitor criado em 01:37

## Notas
- **Quota independente:** 4.6 e 4.7 têm quotas separadas no Coding Plan
- **Pico 03:00-07:00:** multiplier 2-3x em 5.1/5-Turbo (4.7 e 4.6 sempre 1x)
- **Modelos grátis:** 4.7-flash e 4.5-flash não consomem quota do plano
