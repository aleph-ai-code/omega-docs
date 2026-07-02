# Dokploy - Setup Completo

**Data:** 2026-06-17
**Servidor:** ceopssrv241 (Sentinel)
**IP:** 172.23.39.241

## Status Atual

✅ **Dokploy instalado e funcionando 100%**

### Containers

| Container | Status | Portas |
|---|---|---|
| dokploy.1.dds32yows5iyv3wrkjxrpgpfw | Up 2 days (healthy) | 3000 |
| dokploy-postgres.1.n975qtu3f9uu1iek29nklmw3 | Up 2 days | 5432 |
| dokploy-redis.1.u45xs1oehrmyosri6xnazf5n2 | Up 2 days | 6379 |

### PostgreSQL 16.14

- **Usuário:** dokploy (Superuser)
- **Database:** dokploy
- **Tabelas:** 62 (account, user, application, deployment, etc.)
- **Status:** 0 usuários → Pronto para setup inicial

### Redis 7.4.9

- **Porta:** 6379
- **Keys:** 2 (bull:deployments:stalled-check, bull:deployments:meta)
- **Uso:** Filas de deploy BullMQ

### Acesso

- **URL:** http://172.23.39.241:3000
- **Versão:** v0.29.8
- **Status:** Pronto para registro inicial

## Comandos Úteis

```bash
# Ver containers
docker ps | grep dokploy

# Acessar Postgres
docker exec -it dokploy-postgres.1.xxx psql -U dokploy -d dokploy

# Acessar Redis
docker exec -it dokploy-redis.1.xxx redis-cli

# Logs Dokploy
docker logs dokploy.1.xxx -f

# Logs Postgres
docker logs dokploy-postgres.1.xxx -f

# Logs Redis
docker logs dokploy-redis.1.xxx -f
```

## Bitwarden

Ver `/tmp/dokploy_bitwarden_instructions.md` para instruções completas de como adicionar no Bitwarden.

## Próximos Passos

1. ⏳ Acessar http://172.23.39.241:3000
2. ⏳ Criar conta administradora
3. ⏳ Atualizar Bitwarden com credenciais
4. ⏳ Configurar domínio/SSL (opcional)

---

Ω · Denalth
