# Cron Calendar — Todos os Agentes

> Última atualização: 2026-07-18
> Ω · Denalth

## Regras de escalonamento
- **Mínimo 20min** entre crons de Omega e Aleph (API key compartilhada)
- Todos usam glm-4.7-flash exceto onde indicado
- **Google Calendar:** Omega usa "Omega Cron", Aleph usa "Aleph Cron"
- Verificar calendários do outro antes de criar crons

---

## Omega (ceopssrv240 — Linux, porta 18789)

| Cron | ID | Horário (BRT) | Modelo | Frequência | Status |
|------|----|--------------|--------|------------|--------|
| Memory Dreaming Promotion | 21a59bbc | **03:00** | default | diário | ✅ OK |
| omega:autoresearch-v2 | f0458b99 | **03:00** | glm-4.7 | diário | ✅ NOVO v2 |
| omega:git-autosave | 2f49e8ef | **08:50, 22:50** | glm-4.7-flash | 2x/dia | ✅ OK |
| omega:docs-sync | 9a2fbf1d | **08:50, 22:50** | glm-4.7-flash | 2x/dia | ✅ OK |
| omega:auto-melhoria | 59182433 | **Sáb 07:00** | glm-4.7-flash | semanal | ✅ OK |
| omega:docs-review | 42711a59 | **Dom 07:00** | glm-4.7-flash | semanal | ✅ OK |
| omega:security-audit | 0bf46e12 | **Dia 1/15 07:00** | glm-4.7-flash | quinzenal | ✅ OK |
| ~~omega:autoresearch-memory~~ | ~~53190ff9~~ | ~~03:00~~ | — | — | ❌ DESATIVADO v1 |
| ~~omega:autoresearch-agents~~ | ~~1ed6c04f~~ | ~~04:00~~ | — | — | ❌ DESATIVADO v1 |
| ~~omega:autoresearch-tools~~ | ~~71ad1666~~ | ~~05:00~~ | — | — | ❌ DESATIVADO v1 |

### Google Calendar — Omega Cron
- **ID:** fbd4cbd83fc92f1ee6addb54333199f33342f9f23578ed27d53de7b8a85f5aa0@group.calendar.google.com
- **Acesso:** `gog calendar events "Omega Cron" --account aleph.ai.code@gmail.com`

---

## SME (ceopssrv249 — Linux, porta 18789)

| Cron | Horário (BRT) | Modelo | Frequência | Status |
|------|--------------|--------|------------|--------|
| sme:autoresearch-v2 | **03:00** | glm-4.7-flash | diário | ✅ NOVO v2 |

> v1 removida (crontab root limpo, scripts apagados)

---

## Sentinel (ceopssrv241 — Linux, porta 18789)

| Cron | Horário (BRT) | Modelo | Frequência | Status |
|------|--------------|--------|------------|--------|
| sentinel:autoresearch-v2 | **03:00** | glm-4.7-flash | diário | ✅ NOVO v2 |

> ⚠️ Gateway esteve baixo 15-18/07 (config Ollama quebrado). Corrigido com `doctor --fix`
> v1 removida (crontab root limpo)

---

## Aleph (ceopssrv250 — Linux, porta 18789)

| Cron | Horário (BRT) | Modelo | Frequência | Status |
|------|--------------|--------|------------|--------|
| **Backup Diário** | **02:00** | — | diário | ✅ OK |
| Memory Dreaming | 03:00 | glm-4.7-flash | diário | ✅ OK |
| aleph:autoresearch-v2 | **03:00** | glm-4.7-flash | diário | ✅ NOVO v2 |
| Auto-melhoria | Sáb 06:00 | glm-4.7-flash | semanal | ✅ OK |
| Docs Review | Dom 06:00 | glm-4.7-flash | semanal | ✅ OK |
| Security Audit | Dia 1/15 06:35 | glm-4.7-flash | quinzenal | ✅ OK |
| Git Autosave Aleph | 08:00, 22:00 | glm-4.7-flash | 2x/dia | ✅ OK |
| Watchdog Crons | 08:10 | glm-4.7-flash | diário | ✅ OK |
| Git Autosave Voyager | 08:30, 22:30 | glm-4.7-flash | 2x/dia | ✅ OK |
| Monitorar Promoções | Seg/Qui 09:00 | glm-4.7-flash | 2x/semana | ✅ OK |

> v1 removida (crontab root limpo, scripts apagados)

### Google Calendar — Aleph Cron
- **Acesso:** `gog calendar events "Aleph Cron" --account aleph.ai.code@gmail.com`

---

## Voyager (ceopssrv251 — Linux, porta 18789)

| Cron | Horário (BRT) | Modelo | Frequência | Status |
|------|--------------|--------|------------|--------|
| Memory Dreaming Promotion | 03:00 | default | diário | ✅ OK |
| voyager:autoresearch-v2 | **03:00** | glm-4.7-flash | diário | ✅ NOVO v2 |
| travel:healthcheck | 08:00 | glm-4.7-flash | diário | ✅ OK |
| Watchdog | 08:00 | glm-4.7-flash | diário | ✅ OK |
| travel:git-autosave | 08:00, 22:00 | glm-4.7-flash | 2x/dia | ✅ OK |
| Monitorar Promoções | Seg/Qui 09:00 | default | 2x/semana | ✅ OK |
| Security Audit | Dia 1/15 06:00 | glm-4.7-flash | quinzenal | ✅ OK |

> v1 removida (crontab root limpo, scripts apagados)

---

## Atlas (info-d1454ns — Windows, porta 18790)

| Cron | Horário (BRT) | Modelo | Frequência | Status |
|------|--------------|--------|------------|--------|
| atlas:git-autosave | 08:30, 22:30 | glm-4.5-flash | 2x/dia | ✅ OK |

> ⚠️ Atlas crons rodam via schtasks no Windows, gerenciados pelo Omega remotamente

---

## AutoResearch v2 — Baselines (2026-07-18)

| Servidor | Memory Score | Wiki Score | Órfãos | Duplicatas | Disco (MB) |
|----------|-------------|------------|--------|------------|------------|
| .240 Omega | 0 | 88 | 63 | 15 | 6943 |
| .249 SME | 0 | 88 | 34 | 8 | 3143 |
| .241 Sentinel | 57 | 88 | 2 | 2 | 1561 |
| .250 Aleph | 0 | 87 | 107 | 14 | 3167 |
| .251 Voyager | 0 | 88 | 10 | 1 | 5130 |

> Rotação v2: Seg=memory-dedup, Ter=wiki-health, Qua=stale-cleanup, Qui=disk-cache, Sex=config-audit, Sáb=cross-refs, Dom=skip
> Primeira run: 03:00 de 19/07 (Sábado = cross-refs)

---

## Mapa de Portas

| Servidor | IP LAN | OpenClaw | Ollama | SearXNG | Tailscale | Agente |
|----------|--------|----------|--------|---------|-----------|--------|
| ceopssrv240 | .240 | :18789 ✅ | — | :8888 (Docker) | ✅ | Ω Omega |
| ceopssrv247 | .247 | offline | — | — | ✅ | — |
| ceopssrv241 | .241 | :18789 ✅ | :11434 (LAN) | — | — | Sentinel |
| ceopssrv249 | .249 | :18789 ✅ | — | — | — | SME |
| ceopssrv250 | .250 | :18789 ✅ | — | — | — | א Aleph |
| ceopssrv251 | .251 | :18789 ✅ | — | — | — | ✈️ Voyager |
| info-d1454ns | .39 | :18790 ✅ | — | — | ✅ | 🗺️ Atlas |

---

## Mudanças 2026-07-18

- ✅ AutoResearch v2 deployed em **5 servidores** (.240, .241, .249, .250, .251)
- ✅ AutoResearch v1 removida de todos (crontab root + scripts + crons OpenClaw)
- ✅ Gateway .241 restaurado (doctor --fix, config Ollama corrigido)
- ✅ Cron security-audit corrigido (ollama → glm-4.7-flash)
- ✅ Cron auto-melhoria corrigido (ollama → glm-4.7-flash)
- ✅ metrics-collect.sh deployed e baseline coletada em todos
