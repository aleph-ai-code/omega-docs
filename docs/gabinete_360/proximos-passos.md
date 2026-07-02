# Próximos Passos — Tarefas pro Atlas

> Última atualização: 2026-05-18
> Atlas 🤖 · Denalth

## Prioridade Alta

### 1. Resolver RLS das Tabelas `tb_*`
- [ ] Verificar status RLS no painel Supabase
- [ ] Testar acesso via SQL Editor às tabelas
- [ ] Ajustar políticas RLS se necessário
- [ ] Confirmar acesso via API PostgREST

### 2. Migrar `cadastro_liderancas` → `tb_cadastro_liderancas`
- [ ] Verificar se são a mesma tabela ou distintas
- [ ] Se distintas, migrar dados
- [ ] Atualizar queries pra usar tabela correta

### 3. Validar Views `vw_*`
- [ ] Identificar as 2 views mencionadas
- [ ] Entender propósito de cada view
- [ ] Testar acesso via API

## Prioridade Média

### 4. Criar Interface de Preenchimento Seguro
- [ ] Formulário web (React/HTML)
- [ ] Validação de dados antes de salvar
- [ ] Integração com Supabase via client library
- [ ] Autenticação via Supabase Auth

### 5. Evoluir Dashboard
- [ ] Integrar com `tb_municipios` (quando RLS resolvido)
- [ ] Adicionar gráficos interativos
- [ ] Filtragem por município/região
- [ ] Atualização em tempo real

## Prioridade Baixa

### 6. Documentar Arquitetura Completa
- [ ] Mapear todas as tabelas `tb_*`
- [ ] Documentar todas as FKs
- [ ] Criar diagrama ER (Entity Relationship)
- [ ] Atualizar schema.md com discoveries

### 7. Bot Telegram (Futuro)
- [ ] Reescrever @aleph_ai_bot como assistência pessoal
- [ ] Integração com Supabase para consultas
- [ ] Comandos para preencher lideranças via chat
- [ ] Relatórios automáticos via Telegram

## Dependências
- ✅ Dados extraídos e salvos (`liderancas_extraidas.json`)
- ⏳ Supabase online e acessível
- ⏳ RLS resolvido
- ⏳ Service_role key válida

## Contexto
O Omega já fez o trabalho pesado de extração e documentação. O Atlas, como Dev Sênior em Supabase, deve assumir a partir daqui focando em arquitetura, segurança e desenvolvimento frontend.
