# Guide — Memory System

> Extraído de AGENTS.md para referência
> Ω · Denalth

## Arquitetura

| Arquivo | Papel | Quando usar |
|---------|-------|-------------|
| `memory/YYYY-MM-DD.md` | Raw notes do dia | Toda sessão |
| `MEMORY.md` | Curated wisdom (índice) | Main session only |
| `memory/context/*.md` | Tópicos persistentes | Decisões, lições, pessoas, pendências |

## MEMORY.md — Regras de Segurança

- **SÓ carregar em main session** (chats diretos com Denalth)
- **NUNCA carregar em contextos compartilhados** (Discord, grupos, sessões com outras pessoas)
- Segurança: contém contexto pessoal que não pode vazar pra estranhos

## Disciplina (Non-negotiable)

1. **Toda decisão → `memory/YYYY-MM-DD.md`** no mesmo turno
2. **Mudança de estado → `MEMORY.md`** atualizado na mesma sessão
3. **Antes de compaction/reset → salvar tudo** que não foi persistido
4. **Se resolveu algo → documentar a solução** (não só o problema)
5. **Relações, papéis, arquitetura → MEMORY.md** sempre que definido

### Por quê?
A janela de contexto é finita. Quando o contexto é perdido (reset, compaction, crash), só os arquivos sobrevivem. "Mental notes" morrem com a sessão.

## Auto-Aprendizado

Durante **toda conversa**, identificar e salvar:
1. Preferências do Denalth descobertas
2. Decisões importantes tomadas
3. Restrições mencionadas
4. Metas novas
5. Padrões de infra/comportamento
6. Erros cometidos e soluções encontradas

**Destino:** tudo → `memory/YYYY-MM-DD.md` no dia. Dreaming faz curadoria → MEMORY.md.
**Quando:** imediatamente após identificar. Não acumular.

## Curadoria (Heartbeat)

- Revisar dailies e promover pra MEMORY.md
- Remover info obsoleta
- Manter MEMORY.md enxuto (< 100 linhas idealmente)

## Auto-Flush

O OpenClaw roda um memory flush automático antes de compaction. Mas não dependa disso — salve proativamente.
