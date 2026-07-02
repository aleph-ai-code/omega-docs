# Guide — Heartbeats

> Extraído de AGENTS.md para referência
> Ω · Denalth

## Filosofia

Heartbeats não são só "tô vivo". São oportunidades de ser proativo.

## Heartbeat vs Cron

| Heartbeat | Cron |
|-----------|------|
| Batch de checks juntos | Timing exato |
| Precisa de contexto conversacional | Tarefa isolada |
| Timing pode variar (~30min ok) | Isolado da session principal |
| Reduz API calls combinando | Modelo/thinking diferente |
| — | Lembretes one-shot |
| — | Output direto pra canal |

**Tip:** Batch checks periódicos no HEARTBEAT.md ao invés de criar múltiplos cron jobs.

## Checagens (rotacionar 2-4x/dia)

- Emails urgentes?
- Calendário (próximas 24-48h)?
- Mencions/notifications?
- Clima (se o Denalth for sair)?

Track em `memory/heartbeat-state.json`.

## Quando alcançar

- Email importante
- Evento do calendário (<2h)
- Algo interessante encontrado
- >8h sem falar nada

## Quando ficar quieto (HEARTBEAT_OK)

- Madrugada (23h-08h) salvo urgente
- Humano claramente ocupado
- Nada de novo desde último check
- Check há <30 minutos

## Trabalho proativo (sem pedir)

- Ler e organizar memory files
- Checar projetos (git status)
- Atualizar documentação
- Commit e push mudanças próprias
- Revisar e atualizar MEMORY.md

## Memory Maintenance (durante heartbeats)

Periodicamente (a cada poucos dias):
1. Ler `memory/YYYY-MM-DD.md` recentes
2. Identificar eventos/lições/insights worth keeping
3. Atualizar MEMORY.md com aprendizados destilados
4. Remover info obsoleta do MEMORY.md

Meta: ser útil sem ser chato. Check algumas vezes ao dia, faça trabalho de fundo, respeite horário quieto.
