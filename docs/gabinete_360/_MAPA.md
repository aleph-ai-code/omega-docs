# 🗺️ MAPA — Poli5 Supabase

> Diretório: `docs/poli5-supabase/`
> Projeto: Gabinete 360 — Supabase
> Ω · Denalth
> Data de criação: 2026-05-18

## O que é este projeto
**Documentação técnica do Supabase do projeto Poli5 (lideranças políticas).** Schema, tabelas, RLS, views e arquitetura de dados.

## Estrutura

| Arquivo | Propósito |
|---------|-----------|
| `_MAPA.md` | Este mapa |
| `schema.md` | Estrutura do banco de dados (tb_*, vw_*) |
| `rls-problemas.md` | Problemas de RLS identificados e soluções |
| `liderancas_extraidas.json` | Dados extraídos da tabela de lideranças |
| `proximos-passos.md` | Tarefas pendentes pro Atlas (Dev Sênior) |

## Contexto
- **Projeto:** Gabinete 360 — Dashboard de lideranças políticas
- **Candidatos:** Dayany, Reginauro
- **URL Supabase:** `https://pumsmythpwidsfjhyxhk.supabase.co`
- **API Key:** service_role salva no Bitwarden
- **Status:** POC validada, RLS bloqueando acesso a tabelas auxiliares

## Ligações
- → `docs/ideias-futuras-supabase.md` — Expansões futuras
- → `docs/dashboard-liderancas.html` — Dashboard HTML da POC
- → `infra/` — Documentação de infraestrutura geral

## Arquitetura
- **Frontend:** Dashboard HTML (POC)
- **Backend:** Supabase (PostgreSQL + PostgREST)
- **Bot:** @aleph_ai_bot (Telegram) — será reescrito
- **Dev Sênior:** Atlas 🤖 (especialista em Supabase)
