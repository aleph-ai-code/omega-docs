# 🗺️ MAPA — Docs

> Diretório: `workspace/docs/`
> Última atualização: 2026-05-16
> Ω · Denalth

## O que vai aqui
**Documentos gerais, referências, tutoriais, anotações de estudo.** Tudo que é conhecimento e não se encaixa em infra, scripts ou memória.

## O que NÃO vai aqui
- Diários (→ `memory/`)
- Documentação de infra (→ `infra/`)
- Scripts (→ `scripts/`)
- Configs do agente (→ raiz)

## Estrutura

| Arquivo | Propósito |
|---------|-----------|
| `_MAPA.md` | Este mapa |
| `<assunto>.md` | Documento sobre um assunto específico |
| `<assunto>/` | Subpasta para temas complexos (com `_MAPA.md` próprio) |

## Convenções
- Nome descritivo e direto (ex: `ansible-basico.md`, `git-workflow.md`)
- Se o assunto crescer, virar subpasta com `_MAPA.md`
- Incluir fonte/referência quando aplicável
- Taggear com tipo: `[tutorial]`, `[referência]`, `[how-to]`, `[estudo]`

## Comportamento
- Aprendeu algo novo e útil? → Documenta aqui
- Precisa consultar depois? → Fica aqui
- É sobre infra? → `infra/` (não aqui)

## Ligações
- → `../_MAPA.md` — Mapa raiz do workspace
- → `../memory/_MAPA.md` — Diários podem referenciar docs
- → `../infra/_MAPA.md` — Se o doc é sobre infra, pertence lá
- → `../scripts/_MAPA.md` — Scripts podem ter docs associados aqui

## Arquivos específicos

| Arquivo | Propósito |
|---------|-----------|
| `auto-melhoria-guia.md` | Guia de auto-melhoria (v3) |
| `guide-group-chats.md` | Guia de comportamento em grupos [extraído AGENTS.md] |
| `guide-heartbeats.md` | Guia de heartbeats e checks periódicos [extraído AGENTS.md] |
| `guide-memory-system.md` | Guia do sistema de memória [extraído AGENTS.md] |
| `sistema-imunologico.md` | Sistema Imunológico completo |
| `gabinete_360/` | Projeto Gabinete 360 (Supabase lideranças) |
| `dashboard-liderancas.html` | Dashboard HTML da POC |
| `ideias-futuras-supabase.md` | Expansões futuras do projeto |
| `changelog.md` | Registro de mudanças significativas |
| `cron-calendar.md` | Calendário de jobs agendados |
| `grupos-telegram.md` | Grupos do Telegram configurados |
