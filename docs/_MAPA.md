# 🗺️ MAPA — Docs

> Diretório: `workspace/docs/`
> Última atualização: 2026-07-19 (FASE 4 — Limpeza Completa)
> Ω · Denalth

## O que vai aqui
**Documentos gerais, referências, tutoriais, anotações de estudo.** Tudo que é conhecimento e não se encaixa em infra, scripts ou memória.

## O que NÃO vai aqui
- Diários (→ `memory/`)
- Documentação SME (→ `sme/docs/`)
- Scripts (→ `scripts/`)
- Configs do agente (→ raiz)

## Estrutura Limpa (Pós-FASE 4)

```
docs/
├── _MAPA.md                          # Este mapa
├── MOC.md                            # Map of Content (índice)
├── auto-melhoria-guia.md             # Guia de auto-melhoria (v3)
├── sistema-imunologico.md            # Sistema Imunológico completo
├── bruno-okamoto/                    # Projeto Bruno Okamoto (PRDs)
├── composes/                         # Docker compose files
├── dashboard/                        # Dashboards HTML
├── gabinete_360/                     # Projeto Gabinete 360 (Supabase lideranças)
├── guias/                            # Guias técnicos e how-tos
│   ├── cron-auto-update-guide.md
│   ├── docker-update-guide.md
│   ├── FERRAMENTAS-INSTALADAS-SUCESSO.md
│   ├── ferramentas.md
│   ├── guia-bootstrap-aleph.md
│   ├── guia-instalacao-dokploy.md
│   ├── guide-group-chats.md          # Comportamento em grupos
│   ├── guide-heartbeats.md           # Heartbeats e checks
│   ├── guide-memory-system.md        # Sistema de memória
│   └── guide-windows-ssh-setup.md
├── infra/                            # Infraestrutura
│   ├── changelog.md                  # Histórico mudanças
│   ├── cron-calendar.md              # Calendário crons
│   ├── infra-mapa.md                 # Mapa infra
│   └── 241/                          # Servidor 241 específicos
├── operacoes/                        # Relatórios operacionais
│   ├── gerenciamento-portas.md
│   ├── grupos-telegram.md
│   ├── dashboard-gabinete360.html
│   ├── dashboard-liderancas.html
│   ├── RESPOSTA-COMPLETA.md
│   ├── RESUMO-GERENCIAMENTO-PORTAS.md
│   └── STATUS-FINAL-PORTAS.md
├── servidores/                       # Configurações de servidores
│   └── configuracao-241-final-2026-06-03.md
├── pessoal/                          # (Vazio, reservado)
└── sentinel/                         # (Vazio, reservado)
```

## Convenções
- Nome descritivo e direto (ex: `ansible-basico.md`, `git-workflow.md`)
- Se o assunto crescer, virar subpasta com `_MAPA.md` próprio
- Incluir fonte/referência quando aplicável
- Taggear com tipo: `[tutorial]`, `[referência]`, `[how-to]`, `[estudo]`

## Comportamento
- Aprendeu algo novo e útil? → Documenta aqui
- Precisa consultar depois? → Fica aqui
- É sobre infra? → `infra/` (não aqui)
- É SME? → `sme/docs/` (não aqui)

## Ligações
- → `../_MAPA.md` — Mapa raiz do workspace
- → `../memory/_MAPA.md` — Diários podem referenciar docs
- → `../infra/_MAPA.md` — Infraestrutura técnica
- → `../sme/docs/` — Documentação SME (consolidada)

## Arquivos Principais

| Arquivo | Propósito |
|---------|-----------|
| `auto-melhoria-guia.md` | Guia de auto-melhoria (v3) |
| `sistema-imunologico.md` | Sistema Imunológico completo |
| `MOC.md` | Map of Content (índice geral) |

## Subdiretórios Principais

| Subdir | Propósito |
|--------|-----------|
| `guias/` | Guias técnicos, how-tos, tutoriais |
| `infra/` | Governança, changelog, cron calendar |
| `operacoes/` | Relatórios operacionais, dashboards |
| `servidores/` | Configurações de servidores |
| `bruno-okamoto/` | Projeto Bruno Okamoto (PRDs) |
| `gabinete_360/` | Projeto Gabinete 360 (Supabase) |
| `composes/` | Docker compose files |
| `dashboard/` | Dashboards HTML |
