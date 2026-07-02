# Inventário de Portas - Sentinel (.241)

**Atualizado:** 2026-06-18 02:05
**Servidor:** ceopssrv241 (172.23.39.241)

---

## 📋 PORTAS EM USO

### Portas Docker

| Porta | Container | Aplicação | Propósito | Desde | Status |
|---|---|---|---|---|---|
| **3000** | dokploy.1.dds32yows5iyv3wrkjxrpgpfw | Dokploy v0.29.8 | Plataforma deploy | 2026-06-15 | ✅ Ativo |
| **5432** | dokploy-postgres.1.n975qtu3f9uu1iek29nklmw3 | PostgreSQL 16.14 | Banco Dokploy | 2026-06-15 | 🔒 Interna |
| **6379** | dokploy-redis.1.u45xs1oehrmyosri6xnazf5n2 | Redis 7.4.9 | Cache filas | 2026-06-15 | 🔒 Interna |
| **8001** | redisinsight | RedisInsight | UI Redis | 2026-06-17 | ✅ Ativo |

### Portas do Sistema

| Porta | Serviço | Propósito | Desde | Status |
|---|---|---|---|---|---|
| **18789** | OpenClaw | Gateway OpenClaw | - | ✅ Ativo |
| **11434** | Ollama | API Ollama | LLM local | ✅ Ativo |
| **8088** | Python HTTP Server | Arquivos .md | 2026-06-17 | ✅ Ativo |

---

## 🎯 FAIXAS RESERVADAS

| Faixa | Propósito | Responsável | Observação |
|---|---|---|---|
| **3000-3099** | Aplicações web | Dokploy | 3000 em uso |
| **5000-5999** | Databases | DBA | PostgreSQL em 5432 (interna) |
| **8000-8099** | Ferramentas admin | DevOps | 8001, 8088 em uso |
| **18000-18999** | OpenClaw | Infra | 18789 em uso |

---

## ✅ PORTAS DISPONÍVEIS

### Recomendadas (não conflitam)

| Porta | Uso sugerido | Fácil de lembrar? |
|---|---|---|
| 3001 | Aplicação web 1 | ✅ (3000+1) |
| 3002 | Aplicação web 2 | ✅ (3000+2) |
| 3005 | Aplicação web 3 | ✅ |
| 5050 | pgAdmin | ✅ (50+50) |
| 5051 | Adminer | ✅ |
| 8080 | Ferramenta admin | ✅ |
| 8090 | Monitoramento | ✅ |

### Evitar (potencial conflito)

| Porta | Motivo |
|---|---|
| 3306 | MySQL (padrão) |
| 3389 | MySQL (padrão) |
| 6379 | Redis (já em uso interno) |
| 5432 | PostgreSQL (já em uso interno) |
| 27017 | MongoDB (padrão) |

---

## 🔧 FLUXO DE TRABALHO

### 1. Antes de criar container

```bash
# Verificar porta disponível
/usr/local/bin/port-manager.sh check 3005

# Se disponível, alocar
/usr/local/bin/port-manager.sh allocate 3005 novo-app

# Criar container
docker run -d --name novo-app -p 3005:3000 imagem

# Atualizar este inventário
# Adicionar linha em "PORTAS EM USO"
```

### 2. Ao remover container

```bash
# Parar e remover container
docker stop novo-app
docker rm novo-app

# Liberar porta
/usr/local/bin/port-manager.sh release 3005

# Atualizar este inventário
# Remover linha de "PORTAS EM USO"
```

### 3. Verificação diária

```bash
# Listar portas em uso
/usr/local/bin/port-manager.sh list

# Atualizar inventário se necessário
vim /root/.openclaw/workspace/docs/ports-inventory.md
```

---

## 📊 ESTATÍSTICAS

**Total de portas em uso:** 7
- Docker: 4 (2 externas, 2 internas)
- Sistema: 3

**Portas disponíveis (3000-3099):** 98
**Portas disponíveis (8000-8099):** 98

**Taxa de utilização:** ~3% (7 portas / ~210 faixas comuns)

---

Ω · Denalth
