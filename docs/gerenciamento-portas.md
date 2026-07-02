# Gerenciamento de Portas - Inventário e Controle

**Data:** 2026-06-18 02:01
**Objetivo:** Como gerenciar múltiplas portas de múltiplos containers

---

## 🎯 O QUE É GERENCIAR PORTAS

**Objetivo:** Saber:
- Quais portas estão **EM USO**
- Quais portas estão **DISPONÍVEIS**
- Quais **containers** usam quais portas
- Como **documentar** tudo

**Problema:**
```
Container 1: porta 3000
Container 2: porta 8001
Container 3: porta ??? (conflito?)
Container 4: porta ??? (já está em uso?)
```

---

## 📋 FERRAMENTAS PARA GERENCIAR

### 1. Ver portas EM USO (Linux)

```bash
# Todas as portas em uso
sudo netstat -tlnp

# Ou (mais simples)
sudo ss -tlnp

# Ou (Docker específico)
sudo docker ps --format "table {{.Names}}\t{{.Ports}}"
```

**Saída:**
```
Proto Local Address           State  PID/Program name
tcp   0.0.0.0:3000            0.0.0.0:*  LISTEN  1234/docker-proxy
tcp   0.0.0.0:8001            0.0.0.0:*  LISTEN  5678/docker-proxy
tcp   0.0.0.0:8088            0.0.0.0:*  LISTEN  9876/python3
```

### 2. Ver portas DISPONÍVEIS

```bash
# Portas comuns não utilizadas
# 3000-3999: Aplicações web
# 8000-8999: Ferramentas admin
# 5000-5999: Databases

# Verificar se porta específica está em uso
sudo lsof -i :3000  # Se vazio, porta disponível
```

### 3. Docker Port Scanner

```bash
# Ver portas expostas por containers
docker ps --format "{{.Ports}}" | tr ',' '\n' | grep -o '0.0.0.0:[0-9]*' | sort -u

# Saída:
# 0.0.0.0:3000
# 0.0.0.0:8001
```

---

## 📚 SISTEMA DE DOCUMENTAÇÃO

### Arquivo: `ports-inventory.md`

```markdown
# Inventário de Portas - Sentinel (.241)

## Portas em Uso

| Porta | Container | Aplicação | Propósito | Desde |
|---|---|---|---|---|
| 3000 | dokploy.1.dds32yows5iyv3wrkjxrpgpfw | Dokploy | Plataforma deploy | 2026-06-15 |
| 5432 | dokploy-postgres.1.n975qtu3f9uu1iek29nklmw3 | PostgreSQL | Banco Dokploy | 2026-06-15 |
| 6379 | dokploy-redis.1.u45xs1oehrmyosri6xnazf5n2 | Redis | Cache filas | 2026-06-15 |
| 8001 | redisinsight | RedisInsight | UI Redis | 2026-06-17 |
| 8088 | python3 http.server | HTTP Server | Arquivos .md | 2026-06-17 |

## Portas Reservadas

| Faixa | Propósito | Responsável |
|---|---|---|
| 3000-3099 | Aplicações web | Dokploy |
| 8000-8099 | Ferramentas admin | DevOps |
| 5000-5999 | Databases | DBA |

## Portas Disponíveis

- 3001-3099 (exceto 3000)
- 8002-8099 (exceto 8001, 8088)
- 5050, 5051, etc.
```

---

## 🔧 SCRIPT DE GERENCIAMENTO

### Criar: `/usr/local/bin/port-manager.sh`

```bash
#!/bin/bash
# Gerenciador de portas - Sentinel

ACTION=$1
PORT=$2
CONTAINER=$3

case $ACTION in
  list)
    echo "=== Portas em Uso ==="
    docker ps --format "table {{.Names}}\t{{.Ports}}"
    sudo ss -tlnp | grep LISTEN
    ;;
  
  check)
    echo "=== Verificando porta $PORT ==="
    if sudo lsof -i :$PORT >/dev/null 2>&1; then
      echo "❌ Porta $PORT EM USO"
      sudo lsof -i :$PORT
    else
      echo "✅ Porta $PORT DISPONÍVEL"
    fi
    ;;
  
  allocate)
    echo "=== Alocando porta $PORT para $CONTAINER ==="
    if sudo lsof -i :$PORT >/dev/null 2>&1; then
      echo "❌ ERRO: Porta $PORT já está em uso!"
      exit 1
    fi
    echo "✅ Porta $PORT disponível para $CONTAINER"
    # Adicionar ao inventário
    echo "$PORT,$CONTAINER,$(date +%Y-%m-%d)" >> /var/log/port-allocation.log
    ;;
  
  release)
    echo "=== Liberando porta $PORT ==="
    # Remover do inventário
    sed -i "/^$PORT,/d" /var/log/port-allocation.log
    echo "✅ Porta $PORT liberada"
    ;;
  
  *)
    echo "Uso: port-manager.sh {list|check|allocate|release} [porta] [container]"
    exit 1
    ;;
esac
```

**Usar:**
```bash
sudo /usr/local/bin/port-manager.sh list
sudo /usr/local/bin/port-manager.sh check 3001
sudo /usr/local/bin/port-manager.sh allocate 3001 novo-container
sudo /usr/local/bin/port-manager.sh release 3001
```

---

## 📊 INVENTÁRIO ATUAL - .241

### Portas Docker

| Container | Portas Expostas | Porta Host | Status |
|---|---|---|---|
| dokploy.1.dds32yows5iyv3wrkjxrpgpfw | 3000/tcp | 3000 | ✅ Em uso |
| dokploy-postgres.1.n975qtu3f9uu1iek29nklmw3 | 5432/tcp | - | 🔒 Interna |
| dokploy-redis.1.u45xs1oehrmyosri6xnazf5n2 | 6379/tcp | - | 🔒 Interna |
| redisinsight | 8001/tcp | 8001 | ✅ Em uso |

### Portas do Sistema

| Porta | Serviço | Status |
|---|---|---|
| 18789 | OpenClaw | ✅ Em uso |
| 11434 | Ollama | ✅ Em uso |
| 8088 | HTTP Server (Python) | ✅ Em uso |

---

## 🎯 FLUXO DE TRABALHO

### 1. Antes de criar container

```bash
# 1. Verificar porta desejada
sudo /usr/local/bin/port-manager.sh check 3005

# 2. Se disponível, alocar
sudo /usr/local/bin/port-manager.sh allocate 3005 novo-app

# 3. Criar container
docker run -d --name novo-app -p 3005:3000 imagem

# 4. Atualizar inventário
vim /root/.openclaw/workspace/docs/ports-inventory.md
```

### 2. Ao remover container

```bash
# 1. Parar container
docker stop novo-app
docker rm novo-app

# 2. Liberar porta
sudo /usr/local/bin/port-manager.sh release 3005

# 3. Atualizar inventário
vim /root/.openclaw/workspace/docs/ports-inventory.md
```

### 3. Verificação periódica

```bash
# Diário (cron)
sudo /usr/local/bin/port-manager.sh list > /var/log/port-daily.log
```

---

## 📖 EXEMPLO PRÁTICO

### Cenário: Adicionar pgAdmin

```bash
# 1. Verificar porta 5050
sudo /usr/local/bin/port-manager.sh check 5050
# Saída: ✅ Porta 5050 DISPONÍVEL

# 2. Alocar porta
sudo /usr/local/bin/port-manager.sh allocate 5050 pgadmin

# 3. Criar container
docker run -d --name pgadmin -p 5050:80 dpage/pgadmin4

# 4. Atualizar inventário
# Adicionar linha: 5050 | pgadmin | pgAdmin | UI PostgreSQL | 2026-06-18

# 5. Resultado
docker ps | grep pgadmin
# pgadmin ... 0.0.0.0:5050->80/tcp ...
```

---

## 🔍 COMANDOS ÚTEIS

### Ver portas em uso
```bash
sudo ss -tlnp                          # Todas as portas
sudo docker ps --format "table {{.Names}}\t{{.Ports}}"  # Docker ports
sudo lsof -i :3000                     # Porta específica
netstat -tlnp | grep LISTEN            # Portas listening
```

### Ver conflitos
```bash
sudo ss -tlnp | grep :3000              # Porta 3000 em uso?
sudo docker ps | grep 3000              # Qual container usa 3000?
```

### Documentar
```bash
# Atualizar inventário
vim /root/.openclaw/workspace/docs/ports-inventory.md

# Ou script automático
sudo /usr/local/bin/port-manager.sh list >> /var/log/ports.log
```

---

## 🎯 RESPOSTA DIRETA

### Como gerencio múltiplas portas?

**1. Documentar tudo** em `ports-inventory.md`
**2. Usar script** `port-manager.sh`
**3. Verificar antes** de alocar
**4. Atualizar inventário** após mudanças

### Como sei quais portas estão em uso?

```bash
sudo ss -tlnp
sudo docker ps --format "table {{.Names}}\t{{.Ports}}"
```

### Como sei quais portas estão disponíveis?

```bash
# Verificar porta específica
sudo /usr/local/bin/port-manager.sh check 3005

# Ou ver inventário documentado
cat /root/.openclaw/workspace/docs/ports-inventory.md
```

---

## 📦 PRÓXIMO PASSO

Vou criar:
1. ✅ Script `port-manager.sh`
2. ✅ Arquivo `ports-inventory.md`
3. ⏳ Inventário atual do .241

---

Ω · Denalth
