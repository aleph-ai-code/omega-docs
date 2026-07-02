# Cron Calendar — Todos os Agentes

> Última atualização: 2026-07-01
> Ω · Denalth

## Regras de escalonamento
- **Mínimo 20min** entre crons de Omega e Aleph (API key compartilhada)
- Todos usam modelo gratuito (glm-4.5-flash) exceto emergências
- **Google Calendar:** Omega usa "Omega Cron", Aleph usa "Aleph Cron"
- Verificar calendários do outro antes de criar crons

---

## Omega (ceopssrv240 — Linux, porta 18789)

| Cron | ID | Horário (BRT) | Modelo | Frequência | Status |
|------|----|--------------|--------|------------|--------|
| Memory Dreaming Promotion | dec6d229 | **04:00** | default | diário | ✅ OK |
| omega:auto-melhoria | 59182433 | **Sáb 07:00** | glm-4.5-flash | semanal | ✅ OK |
| omega:docs-review | 42711a59 | **Dom 07:00** | glm-4.5-flash | semanal | ✅ OK |
| omega:security-audit | 0bf46e12 | **Dia 1/15 07:00** | glm-4.5-flash | quinzenal | ⚠️ Rate limit |
| omega:git-autosave | 2f49e8ef | **08:50, 22:50** | glm-4.5-flash | 2x/dia | ⚠️ Timeout |
| watchdog-crons | 789843c6 | **09:00** | glm-4.5-flash | diário | ❌ 3+ falhas |
| atlas:git-autosave | aa6a53fe | 08:30, 22:30 | glm-4.5-flash | 2x/dia | ✅ OK |

### Google Calendar — Omega Cron
- **ID:** fbd4cbd83fc92f1ee6addb54333199f33342f9f23578ed27d53de7b8a85f5aa0@group.calendar.google.com
- **Acesso:** `gog calendar events "Omega Cron" --account aleph.ai.code@gmail.com`

### Status Atual (2026-06-15)
- ✅ **Todos os crons OK** — migrados para Ollama local (ollama/qwen2.5-coder:3b + fallback ollama/qwen3:4b)
- ✅ **Timeouts resolvidos** — payload 300s
- ✅ **Watchdog** removido (função absorvida por crons individuais)

## Atlas (info-d1454ns — Windows, porta 18790)

| Cron | Horário (BRT) | Modelo | Frequência | Status |
|------|--------------|--------|------------|--------|
| atlas:git-autosave | 08:30, 22:30 | glm-4.5-flash | 2x/dia | ✅ OK |

> ⚠️ Atlas crons rodam via schtasks no Windows, gerenciados pelo Omega remotamente

## Aleph (ceopssrv250 — Linux, porta 18789)

| Cron | Horário (BRT) | Modelo | Frequência | Status |
|------|--------------|--------|------------|--------|
| **Backup Diário** | **02:00** | — | diário | ✅ NOVO |
| Memory Dreaming | 03:00 | glm-4.5-flash | diário | ✅ OK |
| Auto-melhoria | Sáb 06:00 | glm-4.5-flash | semanal | ✅ OK |
| Docs Review | Dom 06:00 | glm-4.5-flash | semanal | ✅ OK |
| Security Audit | Dia 1/15 06:35 | glm-4.5-flash | quinzenal | ✅ OK |
| Git Autosave Aleph | 08:00, 22:00 | glm-4.5-flash | 2x/dia | ✅ OK |
| Watchdog Crons | 08:10 | glm-4.5-flash | diário | ✅ OK |
| Git Autosave Voyager | 08:30, 22:30 | glm-4.5-flash | 2x/dia | ✅ OK |
| Monitorar Promoções | Seg/Qui 09:00 | glm-4.5-flash | 2x/semana | ✅ OK |

### Google Calendar — Aleph Cron
- **Acesso:** `gog calendar events "Aleph Cron" --account aleph.ai.code@gmail.com`

## Voyager (ceopssrv251 — Linux, porta 18789)

| Cron | Horário (BRT) | Modelo | Frequência | Status |
|------|--------------|--------|------------|--------|
| Memory Dreaming Promotion | 03:00 | default | diário | ✅ OK |
| Memory Dreaming | 03:00 | glm-4.5-flash | diário | ✅ OK |
| travel:healthcheck | 08:00 | glm-4.5-flash | diário | ✅ OK |
| Watchdog | 08:00 | glm-4.5-flash | diário | ✅ OK |
| travel:git-autosave | 08:00, 22:00 | glm-4.5-flash | 2x/dia | ✅ OK |
| Monitorar Promoções | Seg/Qui 09:00 | default | 2x/semana | ✅ OK |
| Security Audit | Dia 1/15 06:00 | glm-4.5-flash | quinzenal | ✅ OK |

## 241 (LLM Server — Linux, porta 18789)

| Cron | Horário (BRT) | Modelo | Frequência | Status |
|------|--------------|--------|------------|--------|
| Memory Dreaming Promotion | 03:00 | default | diário | ✅ OK |
| healthcheck | 08:00 | glm-4.5-flash | diário | ✅ OK |
| git-autosave | 08:00, 22:00 | glm-4.5-flash | 2x/dia | ✅ OK |

> ⚠️ 241 agente sem identidade definida — usando core files genéricos

---

## Mapa de Portas

| Servidor | IP LAN | OpenClaw | Ollama | SearXNG | Tailscale | Agente |
|----------|--------|----------|--------|---------|-----------|--------|
| ceopssrv240 | .240 | :18789 ✅ | — | :8888 (Docker) | ✅ | Ω Omega |
| ceopssrv247 | .247 | :18789 (offline) | — | — | ✅ | — |
| ceopssrv241 | .241 | :18789 ✅ | :11434 (LAN) | — | ⏳ | Sentinel (?) |
| ceopssrv250 | .250 | :18789 ✅ | — | — | — | א Aleph |
| ceopssrv251 | .251 | :18789 ✅ | — | — | — | ✈️ Voyager |
| info-d1454ns | .39 | :18790 ✅ | — | — | ✅ | 🗺️ Atlas (Windows) |

---

## Separação de Horários (20min de distância)

| Hora | Aleph | Omega | Status |
|------|-------|-------|--------|
| 02:00 | **Backup Diário** ⭐ | — | ✅ NOVO |
| 03:00 | Memory Dreaming | — | ✅ OK |
| 04:00 | — | Memory Dreaming | ✅ OK |
| 06:00 | Auto-melhoria (Sáb) / Docs (Dom) | — | ✅ OK |
| 06:35 | Security Audit | — | ✅ OK |
| 07:00 | — | Auto/Docs/Security | ✅ OK |
| 08:00 | Git Autosave | — | ✅ OK |
| 08:10 | Watchdog | — | ✅ OK |
| 08:30 | Git Autosave Voyager | — | ✅ OK |
| 08:50 | — | Git Autosave Omega | ✅ OK |
| 09:00 | Monitorar Promoções | Watchdog | ⚠️ Promoções só seg/qui |
| 22:00 | Git Autosave | — | ✅ OK |
| 22:30 | Git Autosave Voyager | — | ✅ OK |
| 22:50 | — | Git Autosave Omega | ✅ OK |
