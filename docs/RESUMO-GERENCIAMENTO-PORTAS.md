# GERENCIAMENTO DE PORTAS - SOLUÇÃO COMPLETA

**Data:** 2026-06-18 02:10
**Servidor:** ceopssrv241 (Sentinel)

---

## 🎯 RESPOSTA DIRETA

### Como gerencio múltiplas portas de múltiplos containers?

**Solução:** 3 camadas

**1. Script `port-manager.sh`** ✅
- Ver portas em uso
- Checar se porta disponível
- Alocar porta para container
- Liberar porta

**2. Inventário `ports-inventory.md`** ✅
- Documentação de todas as portas
- Faixas reservadas
- Portas disponíveis
- Histórico de alocações

**3. Processo** ✅
- Verificar antes de alocar
- Documentar após criar
- Atualizar ao remover

---

## 📋 FERRAMENTAS CRIADAS

### 1. Script: `/usr/local/bin/port-manager.sh`

**Instalado:** ✅ Sim (.241)

**Comandos:**
```bash
# Listar portas em uso
port-manager.sh list

# Verificar se porta disponível
port-manager.sh check 3005

# Alocar porta para container
port-manager.sh allocate 3005 novo-app

# Liberar porta
port-manager.sh release 3005
```

**Testes realizados:**
```bash
# Porta 3005 disponível ✅
port-manager.sh check 3005
# Output: ✅ Porta 3005 DISPONÍVEL

# Porta 3000 em uso ✅
port-manager.sh check 3000
# Output: ❌ Porta EM USO
# Mostra: docker-proxy (porta 3000)
```

### 2. Inventário: `docs/ports-inventory.md`

**Criado:** ✅

**Conteúdo:**
- Portas em uso (3000, 5432, 6379, 8001, 18789, 11434, 8088)
- Faixas reservadas (3000-3099, 5000-5999, 8000-8099)
- Portas disponíveis (3001-3099, exceto 3000)
- Fluxo de trabalho
- Estatísticas

---

## 📊 INVENTÁRIO ATUAL - .241

### Portas Docker em Uso

| Porta | Container | Aplicação | Status |
|---|---|---|---|
| **3000** | dokploy.1 | Dokploy v0.29.8 | ✅ Up 2 days |
| **5432** | dokploy-postgres.1 | PostgreSQL 16.14 | 🔒 Interna |
| **6379** | dokploy-redis.1 | Redis 7.4.9 | 🔒 Interna |
| **8001** | redisinsight | RedisInsight | ✅ Up 8 hours |

### Portas do Sistema

| Porta | Serviço | Propósito |
|---|---|---|
| **18789** | OpenClaw | Gateway |
| **11434** | Ollama | LLM local |
| **8088** | Python HTTP | Arquivos .md |

---

## 🔧 COMO USAR

### Antes de criar container

```bash
# 1. Verificar porta disponível
port-manager.sh check 3005

# 2. Se disponível, alocar
port-manager.sh allocate 3005 novo-app

# 3. Criar container
docker run -d --name novo-app -p 3005:3000 imagem

# 4. Atualizar inventário
vim docs/ports-inventory.md
```

### Ao remover container

```bash
# 1. Parar e remover
docker stop novo-app
docker rm novo-app

# 2. Liberar porta
port-manager.sh release 3005

# 3. Atualizar inventário
vim docs/ports-inventory.md
```

### Verificação diária

```bash
# Listar todas as portas
port-manager.sh list

# Ver inventário
cat docs/ports-inventory.md
```

---

## ✅ PORTAS DISPONÍVEIS

### Recomendadas (fáceis de lembrar)

| Porta | Uso | Disponível? |
|---|---|---|
| **3001** | App 1 | ✅ Sim |
| **3002** | App 2 | ✅ Sim |
| **3005** | App 3 | ✅ Sim |
| **5050** | pgAdmin | ✅ Sim |
| **5051** | Adminer | ✅ Sim |
| **8090** | Monitoramento | ✅ Sim |

### Faixas livres

- **3001-3099**: 98 portas disponíveis (exceto 3000)
- **8002-8099**: 97 portas disponíveis (exceto 8001, 8088)

---

## 📚 DOCUMENTAÇÃO

**Acessar:** http://172.23.39.241:8088/docs/

**Arquivos:**
- `gerenciamento-portas.md` - Guia completo
- `ports-inventory.md` - Inventário atual
- `STATUS-FINAL-PORTAS.md` - Resumo

---

## 🎯 RESUMO FINAL

### Como gerencio portas?

1. **Script `port-manager.sh`** - Ferramenta de gerenciamento ✅
2. **Inventário `ports-inventory.md`** - Documentação ✅
3. **Verificar antes** - `port-manager.sh check` ✅
4. **Documentar depois** - Atualizar inventário ✅

### Como sei quais portas estão em uso?

```bash
port-manager.sh list
```

### Como sei quais portas estão disponíveis?

```bash
port-manager.sh check 3005
cat docs/ports-inventory.md
```

### Tudo criado e funcionando?

**SIM!** ✅
- Script instalado em /usr/local/bin/
- Inventário criado em docs/
- Testes funcionando (3005 disponível, 3000 em uso)

---

**Pronto para gerenciar suas portas!** 🎉

---

Ω · Denalth
