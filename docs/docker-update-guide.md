# Docker Update Guide — Dokploy (.241)

**Data:** 2026-06-17
**Servidor:** ceopssrv241 (Sentinel)

## Status Atual (2026-06-17)

| Componente | Versão | Status | Última Atualização |
|---|---|---|---|
| **Dokploy** | v0.29.8 | ✅ Última versão | 2026-06-08 |
| **PostgreSQL** | 16 | ✅ LTS (até 2028) | 2026-06-11 |
| **Redis** | 7 | ⚠️ Estável | 2026-06-11 |

### Detalhes

**Dokploy v0.29.8**
- Última release: v0.29.8 (2026-06-08)
- ✅ **Está atualizado**
- Próxima versão: ainda não lançada

**PostgreSQL 16**
- Versão LTS suportada até 2028
- PostgreSQL 17 existe, mas 16 é mais estável
- ✅ **Manter versão atual**

**Redis 7**
- Versão estável e amplamente usada
- Redis 8 existe, mas não é necessário atualizar
- ⚠️ **Atualizar só se houver necessidade**

## Como Verificar Atualizações

### 1. Via GitHub API (Dokploy)

```bash
curl -s https://api.github.com/repos/dokploy/dokploy/releases/latest | grep '"tag_name"'
```

### 2. Via Docker Hub (imagens base)

```bash
# PostgreSQL
curl -s https://registry.hub.docker.com/v2/repositories/library/postgres/tags/ | jq -r '.results[:3] | .[] | .name'

# Redis
curl -s https://registry.hub.docker.com/v2/repositories/library/redis/tags/ | jq -r '.results[:3] | .[] | .name'
```

### 3. Via SSH (.241)

```bash
# Ver imagens locais
ssh ceops@172.23.39.241 "sudo docker images --format 'table {{.Repository}}:{{.Tag}}\t{{.Size}}\t{{.CreatedAt}}'"

# Ver containers rodando
ssh ceops@172.23.39.241 "sudo docker ps --format 'table {{.Names}}\t{{.Image}}\t{{.Status}}'"

# Puxar atualizações (dry run)
ssh ceops@172.23.39.241 "sudo docker pull dokploy/dokploy:latest"
```

### 4. Via OpenClaw (.241)

```bash
# Ver imagens
nodes --action describe --node=.241 | grep docker

# Ou diretamente
exec --host=node --node=.241 --command="docker images"
```

## Política de Atualização

### Recomendada (LTS + Estabilidade)

- **Dokploy:** Atualizar sempre que houver nova versão
- **PostgreSQL:** Manter versão LTS (16 → 17 só em 2028)
- **Redis:** Atualizar major versões a cada 1-2 anos

### Agressiva (Bleeding Edge)

- **Dokploy:** Atualizar sempre
- **PostgreSQL:** Atualizar para 17 agora
- **Redis:** Atualizar para 8 agora

⚠️ **Não recomendado** para produção

## Como Atualizar

### Dokploy (quando houver nova versão)

```bash
# 1. Fazer backup do banco
ssh ceops@172.23.39.241 "sudo docker exec dokploy-postgres.1.xxx pg_dump -U dokploy dokploy > backup.sql"

# 2. Parar containers
ssh ceops@172.23.39.241 "cd /var/lib/dokploy && docker-compose down"

# 3. Puxar nova versão
ssh ceops@172.23.39.241 "sudo docker pull dokploy/dokploy:latest"

# 4. Atualizar docker-compose.yml
ssh ceops@172.23.39.241 "sudo vim /var/lib/dokploy/docker-compose.yml"

# 5. Subir containers
ssh ceops@172.23.39.241 "cd /var/lib/dokploy && docker-compose up -d"

# 6. Verificar status
ssh ceops@172.23.39.241 "sudo docker ps | grep dokploy"
```

### PostgreSQL/Redis (se necessário)

```bash
# 1. Backup obrigatório
ssh ceops@172.23.39.241 "sudo docker exec dokploy-postgres.1.xxx pg_dump -U dokploy dokploy > backup.sql"

# 2. Alterar imagem em docker-compose.yml
ssh ceops@172.23.39.241 "sudo vim /var/lib/dokploy/docker-compose.yml"

# 3. Recreate containers
ssh ceops@172.23.39.241 "cd /var/lib/dokploy && docker-compose up -d --force-recreate"
```

## Automatização (Futuro)

### Cron Job (Semanal)

```bash
# Adicionar em docs/cron-calendar.md
# Verificar atualizações do Dokploy (semanalmente, domingo 10h)
```

### Script de Checagem

```bash
# /usr/local/bin/check-dokploy-updates
#!/bin/bash
LATEST=$(curl -s https://api.github.com/repos/dokploy/dokploy/releases/latest | jq -r '.tag_name')
CURRENT=$(docker ps --format '{{.Image}}' | grep dokploy | cut -d: -f2)

if [ "$LATEST" != "$CURRENT" ]; then
  echo "Nova versão disponível: $LATEST (atual: $CURRENT)"
  # Enviar notificação via Telegram
fi
```

## Resumo

✅ **Tudo atualizado**
- Dokploy v0.29.8 (última versão)
- PostgreSQL 16 (LTS estável)
- Redis 7 (versão estável)

📅 **Próxima verificação:** 2026-06-24 (1 semana)

---

Ω · Denalth
