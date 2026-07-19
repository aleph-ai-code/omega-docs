# RLS — Problemas e Soluções

> Última atualização: 2026-05-18
> Ω · Denalth

## Problema Identificado

**Tabelas `tb_*` não respondem via API PostgREST**, mesmo com service_role key.

Possíveis causas:
1. **RLS (Row Level Security)** ativado e bloqueando
2. **Service_role key** sem permissão de acesso
3. **Tabelas em schema diferente** (não `public`)
4. **Tabelas não habilitadas** na API do Supabase

## Sintomas
- ✅ `cadastro_liderancas` funciona (200 OK, dados retornam)
- ❌ `tb_municipios` retorna vazio
- ❌ `tb_responsaveis_contato` retorna vazio
- ❌ `tb_acordos_financeiros` retorna vazio
- ❌ Views `vw_*` não verificadas

## Soluções a Tentar

### 1. Verificar RLS no Painel Supabase
1. Acessar: https://supabase.com/dashboard/project/pumsmythpwidsfjhyxhk/auth/policies
2. Para cada tabela `tb_*`:
   - Ver se RLS está ativado
   - Ver se existem políticas permitindo service_role
   - Adicionar política se necessário

### 2. Verificar Schema das Tabelas
1. SQL Editor no painel Supabase
2. Rodar:
```sql
SELECT table_schema, table_name
FROM information_schema.tables
WHERE table_name LIKE 'tb_%' OR table_name LIKE 'vw_%';
```

### 3. Habilitar Tabelas na API
1. Database → API → Tables
2. Verificar se as tabelas `tb_*` estão habilitadas para acesso REST
3. Habilitar se necessário

### 4. Verificar Permissões do Service Role
1. SQL Editor:
```sql
SELECT * FROM pg_roles WHERE rolname = 'service_role';
```

## Próximos Passos (Pra quando o Supabase voltar)
1. **Verificar no painel** se o projeto ainda está ativo
2. **Confirmar service_role key** em Settings → API
3. **Testar acesso SQL Editor** direto às tabelas `tb_*`
4. **Ajustar RLS** se necessário

## Solução Provisória
Enquanto RLS não é resolvido:
- **Usar `cadastro_liderancas`** (tabela acessível)
- **Campo `municipio` denormalizado** funciona pra leitura
- **JOIN com tabelas auxiliares** não funciona via API
