# Sistema Imunológico — Guia de Proteção

> "Agents are 30% of the work. The other 70% is the immune system." — Eric Siu
> v1, 18/05/2026 | Ω · Denalth
> Link: `docs/auto-melhoria-guia.md` → este arquivo

## 1. Watchdog de Crons

**O que previne:** Crons falhando silenciosamente por dias sem ninguém perceber.

**Como funciona:**
- Cron diário (8h) lista todos os crons ativos
- Checa último run de cada um
- Se falhou: retry automático (até 3x)
- Se falhou 3x: alertar o humano via Telegram

**Config Omega:** `watchdog-crons` (id: 789843c6)

## 2. Feedback Loops

**O que previne:** Repetir sugestões que o humano já rejeitou.

**Como funciona:**
- Diretório: `memory/feedback/`
  - `content.json` — feedback sobre conteúdo, drafts, sugestões
  - `tasks.json` — feedback sobre entregas de tasks
  - `recommendations.json` — feedback sobre sugestões de tools/processos

**Formato:**
```json
{
  "entries": [
    {
      "date": "2026-05-18",
      "context": "Sugeri X",
      "decision": "approve",
      "reason": "Motivo",
      "tags": ["tag1", "tag2"]
    }
  ]
}
```

**Regras:**
- Max 30 entradas por arquivo (FIFO)
- Agente DEVE consultar feedback antes de sugerir
- Ciclo: Feedback (JSON) → Lessons (prosa) → Decisions (permanente)

## 3. Monitoramento de Custos

**O que previne:** Gasto runaway com API.

| Uso | Modelo | Custo |
|-----|--------|-------|
| Interação direta | GLM-5.1 | $$$ |
| Crons e automação | GLM-4.5-flash | grátis |
| Heartbeats | GLM-4.5-flash | grátis |

**Regra:** TODOS os crons em modelo gratuito. Só chat direto usa modelo pago.

**Comando pra checar custo:** `session_status` ou `/status`

## 4. Sub-agents: Nunca Fire-and-Forget

**O que previne:** Sub-agents travados no limbo, trabalho perdido.

**Fluxo com sessions_yield:**
```
1. Agent spawna sub-agent com sessions_spawn
2. Agent chama sessions_yield → turno encerra limpo
3. Sub-agent termina → próximo turno começa com o resultado
```

**Regras:**
1. Ao spawnar: informar o que o sub-agent vai fazer
2. Usar `sessions_yield` após spawnar
3. Follow-up em 15-30 min
4. Sucesso: resumir em linguagem humana
5. Falha: retry → se 2x falhar → avisar humano
6. **Nunca** deixar cair no limbo silencioso

## 5. Backup antes de mudanças

**O que previne:** Perda de dados por mudança estrutural que deu errado.

**Procedimento:**
1. Criar backup em `backups/YYYY-MM-DD/`
2. Se der erro, criar `ROLLBACK.md` com instruções de reversão
3. `trash` > `rm` sempre

**Template ROLLBACK.md:** `backups/ROLLBACK.md`

## 6. Secrets

**O que previne:** Credenciais vazadas em backup ou compartilhamento.

**Regras:**
- Nunca hardcodar senhas/tokens em arquivos .md ou .json do workspace
- Usar Bitwarden CLI (`bw`) pra tudo
- Rodar `openclaw secrets audit` periodicamente
- Rodar `openclaw doctor` após atualizações

**Comandos úteis:**
```bash
openclaw secrets audit          # Auditar workspace
openclaw secrets audit --report # Relatório detalhado
openclaw doctor                 # Health check geral
openclaw doctor --fix           # Corrigir problemas automáticos
```

## Checklist de Implementação

- [x] Watchdog de crons ativo
- [x] Feedback loops configurados (3 domínios)
- [x] Split de modelos aplicado
- [x] Regra de sub-agents documentada no AGENTS.md
- [x] Backup automático antes de mudanças
- [x] `openclaw secrets audit` executado — zero leaks críticos
- [x] `openclaw doctor` rodado e corrigido
- [x] ROLLBACK.md criado

## Ligações
- → `docs/auto-melhoria-guia.md` — Guia principal de auto-melhoria
- → `AGENTS.md` — Regras operacionais (contém seção Sistema Imunológico)
- → `backups/ROLLBACK.md` — Procedimentos de reversão

---

_Ω · Denalth_

## Guias Relacionados

| Guia | Arquivo | Propósito |
|------|---------|-----------|
| Auto-Melhoria | `docs/auto-melhoria-guia.md` | Como operar e evoluir (operacional) |
| Bootstrap | `docs/guia-bootstrap-aleph.md` | Criar novo agente |
