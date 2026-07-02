# COMO USAR - Gerenciamento de Portas

**Data:** 2026-06-18 02:15
**Objetivo:** Exemplos práticos de uso

---

## 🎯 O QUE VOCÊ PRECISA SABER

### Pergunta 1: Quais portas estão EM USO?

```bash
# Ver todas as portas
port-manager.sh list
```

**Saída:**
```
=== Portas Docker em Uso ===
redisinsight    0.0.0.0:8001->8001/tcp     Up 8 hours
dokploy.1       0.0.0.0:3000->3000/tcp     Up 2 days

=== Portas do Sistema ===
LISTEN 0.0.0.0:3000  (docker-proxy)
LISTEN 0.0.0.0:8001  (docker-proxy)
LISTEN 0.0.0.0:18789 (node)
LISTEN 0.0.0.0:11434 (ollama)
```

**Ou ver inventário documentado:**
```bash
cat docs/ports-inventory.md
```

### Pergunta 2: Quais portas estão DISPONÍVEIS?

```bash
# Verificar porta específica
port-manager.sh check 3005
# Output: ✅ Porta 3005 DISPONÍVEL

port-manager.sh check 3000
# Output: ❌ Porta EM USO (mostra qual processo)
```

**Ou consultar inventário:**
```bash
cat docs/ports-inventory.md | grep "DISPONÍVEIS"
```

### Pergunta 3: Como vou saber qual porta USEI?

```bash
# Alocar porta para container
port-manager.sh allocate 3005 meu-novo-app

# Isso registra em /var/log/port-allocation.log:
# 3005,meu-novo-app,2026-06-18
```

### Pergunta 4: Como documentar?

```bash
# Atualizar inventário manualmente
vim docs/ports-inventory.md

# Adicionar linha em "PORTAS EM USO":
# | 3005 | meu-novo-app | Aplicação | Meu app | 2026-06-18 | ✅ |
```

---

## 📖 EXEMPLO COMPLETO - Adicionar pgAdmin

### Passo 1: Verificar porta disponível

```bash
port-manager.sh check 5050
# Output: ✅ Porta 5050 DISPONÍVEL
```

### Passo 2: Alocar porta

```bash
port-manager.sh allocate 5050 pgadmin
# Output: ✅ Porta 5050 disponível para pgadmin
#         ✅ Porta alocada com sucesso
```

### Passo 3: Criar container

```bash
docker run -d \
  --name pgadmin \
  -p 5050:80 \
  -e PGADMIN_DEFAULT_EMAIL=admin@ceops.local \
  -e PGADMIN_DEFAULT_PASSWORD=admin123 \
  dpage/pgadmin4
```

### Passo 4: Atualizar inventário

```bash
vim docs/ports-inventory.md
```

Adicionar em "PORTAS EM USO":
```markdown
| **5050** | pgadmin | pgAdmin | UI PostgreSQL | 2026-06-18 | ✅ Ativo |
```

### Passo 5: Verificar funcionamento

```bash
port-manager.sh list | grep 5050
# Output: pgadmin ... 0.0.0.0:5050->80/tcp ...
```

---

## 🚨 CENÁRIO DE CONFLITO

### Tentar usar porta já em uso

```bash
# Tentar alocar porta 3000
port-manager.sh allocate 3000 novo-app
# Output: ❌ ERRO: Porta 3000 já está em uso!
#         Mostra: docker-proxy usando porta 3000
```

### Solução: Escolher outra porta

```bash
port-manager.sh check 3001
# Output: ✅ Porta 3001 DISPONÍVEL

port-manager.sh allocate 3001 novo-app
# Output: ✅ Porta 3001 disponível para novo-app
```

---

## 📋 INVENTÁRIO ATUAL - .241

### Portas em uso (HOJE)

| Porta | Container | Aplicação | Propósito |
|---|---|---|---|
| **3000** | dokploy.1 | Dokploy | Plataforma deploy |
| **5432** | dokploy-postgres.1 | PostgreSQL | Banco (interna) |
| **6379** | dokploy-redis.1 | Redis | Cache (interna) |
| **8001** | redisinsight | RedisInsight | UI Redis |
| **18789** | node | OpenClaw | Gateway |
| **11434** | ollama | Ollama | LLM local |
| **8088** | python3 | HTTP Server | Arquivos .md |

### Portas disponíveis (agora)

- **3001-3099** (98 portas)
- **8002-8099** (97 portas)
- **5050** (pgAdmin, se quiser)
- **5051** (Adminer, se quiser)

---

## 🎯 FLUXO DE TRABALHO DIÁRIO

### Manhã (verificar status)

```bash
# Listar portas em uso
port-manager.sh list

# Ver inventário
cat docs/ports-inventory.md
```

### Criar novo container

```bash
# 1. Verificar porta
port-manager.sh check 3002

# 2. Se disponível, alocar
port-manager.sh allocate 3002 meu-app

# 3. Criar container
docker run -d --name meu-app -p 3002:3000 imagem

# 4. Atualizar inventário
vim docs/ports-inventory.md
```

### Remover container

```bash
# 1. Parar container
docker stop meu-app
docker rm meu-app

# 2. Liberar porta
port-manager.sh release 3002

# 3. Atualizar inventário
vim docs/ports-inventory.md
```

---

## 🔍 COMANDOS RÁPIDOS

```bash
# Ver todas as portas em uso
port-manager.sh list

# Ver se porta específica está disponível
port-manager.sh check 3005

# Alocar porta para container
port-manager.sh allocate 3005 meu-container

# Liberar porta
port-manager.sh release 3005

# Ver inventário documentado
cat docs/ports-inventory.md
```

---

## 📊 RESPOSTA DIRETA

### "Como gerencio múltiplas portas?"

**1. Script `port-manager.sh`** - Ferramenta de gerenciamento ✅
**2. Inventário `ports-inventory.md`** - Documentação ✅
**3. Verificar antes de alocar** - `port-manager.sh check` ✅
**4. Documentar após criar** - Atualizar inventário ✅

### "Como vou saber qual tá disponível?"

```bash
port-manager.sh check 3005
cat docs/ports-inventory.md
```

### "Como vou saber qual usei?"

```bash
port-manager.sh list
cat /var/log/port-allocation.log
cat docs/ports-inventory.md
```

---

## ✅ TUDO JÁ CRIADO

**Script:** `/usr/local/bin/port-manager.sh` ✅ Instalado
**Inventário:** `docs/ports-inventory.md` ✅ Criado
**Log:** `/var/log/port-allocation.log` ✅ Ativo

**Pronto para usar AGORA!** 🎉

---

Ω · Denalth
