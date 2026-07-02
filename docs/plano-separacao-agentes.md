# Plano de Separação Omega/Aleph

> Criado: 2026-05-26
> Ω · Denalth

## Objetivo
Separar vida profissional (Omega .240) da pessoal (Aleph .250).

## Mapa Final

| Agente | Host | IP | Bot TG | Escopo |
|--------|------|-----|--------|--------|
| **Omega** ♾️ | .240 | 172.23.39.240 | @ceops_bot | Profissional, infra, sistemas |
| **Aleph** | .250 | 172.23.39.250 | @aleph_ai_bot | Pessoal, Gabinete 360, finanças |
| **Voyager** ✈️ | .251 | 172.23.39.251 | ? | Travel Specialist |

## O que migra pro Aleph (.250)

### Projetos
- **Gabinete 360** → `docs/gabinete_360/`, `memory/projects/gabinete-360.md`
  - Dashboard lideranças políticas (trabalho extra-oficial)
  - Schema, PRD, credenciais Supabase

### Memórias pessoais (migrar conteúdo relevante)
- **Travel/Viagens** → `memory/travel/` (tudo — perfil Denalth, milhas, cartões, rotas)
  - Transferir pra Voyager (.251) que é o Travel Specialist
- **Contas pessoais** → cartões (Nanquim, Inter Black), milhas, programas fidelidade
- **Parceiros não-infra** → Bruno Okamoto (curso OpenClaw)
- **Rotas pessoais** → FOR→BSB, FOR→CGR, esposa

### Crons
- Promoções de milhas → migrar pra Voyager (.251)

## O que fica no Omega (.240)

### Projetos
- **Infra OpenClaw** → toda a operação de rede CEOPS
- Monitoramento, segurança, backups
- APIs compartilhadas (config, não contexto pessoal)

### Memórias
- Servidores, IPs, credenciais SSH (infra)
- Decisões técnicas, lições aprendidas de infra
- Crons de sistema (dreaming, autosave, watchdog, security-audit)
- Contatos de infra/equipe técnica

## APIs (compartilhadas, ambas usam)
- ZAI GLM — cada host com sua key
- Google Gemini — compartilhada
- Groq — compartilhada
- SearXNG — compartilhada

## Passos de Execução

### Fase 1 — Preparar conteúdo pro Aleph (.250)
- [ ] Empacotar Gabinete 360 (docs + memória + schema)
- [ ] Migrar travel pro Voyager (.251)
- [ ] Limpar referências pessoais do Omega

### Fase 2 — Configurar Aleph (.250)
- [ ] Verificar se OpenClaw está instalado na .250
- [ ] Configurar API keys próprias
- [ ] Copiar contexto pessoal
- [ ] Configurar bot @aleph_ai_bot apontando pra .250 (não .251)

### Fase 3 — Limpar Omega (.240)
- [ ] Remover arquivos Gabinete 360
- [ ] Remover pasta travel (migrou pra Voyager)
- [ ] Atualizar MEMORY.md — só profissional
- [ ] Atualizar people.md — só contatos profissionais
- [ ] Atualizar business-context.md — remover Gabinete 360
- [ ] Atualizar pending.md — realocar pendências

### Fase 4 — Atualizar Voyager (.251)
- [ ] Receber pasta travel/ completa
- [ ] Configurar como Travel Specialist dedicado
- [ ] Manter Duffel API

## Status
- **Fase 1:** Pendente
- **Fase 2:** Pendente (precisa confirmar estado da .250)
- **Fase 3:** Pendente
- **Fase 4:** Pendente
