# Dokploy - Guia de Domínio e Acesso

**Data:** 2026-06-17
**Servidor:** ceopssrv241 (Sentinel)

## 🌐 Domínio - Configuração e Recomendações

### Opções

#### 1. Subdomínio Cloudflare (RECOMENDADO ✅)

**Vantagens:**
- SSL gratuito automático
- Proxy/DDoS protection
- DNS fácil de gerenciar
- Zero downtime deployment

**Setup:**
```bash
# 1. Criar subdomínio no Cloudflare
# dokploy.seudominio.com → 172.23.39.241

# 2. No Dokploy (via UI)
# Settings → Domains → Add domain
# Domain: dokploy.seudominio.com

# 3. Cloudflare vai gerar SSL automaticamente
```

**Exemplo:**
- `dokploy.ceops.net` → 172.23.39.241
- SSL automático via Cloudflare

#### 2. Traefik (Reverse Proxy)

**Vantagens:**
- SSL automático (Let's Encrypt)
- Auto-discovery de containers
- Balanceamento de carga
- Middleware avançado

**Setup:**
```bash
# Instalar Traefik no .241
# Adicionar labels no Dokploy
# SSL automático via Let's Encrypt
```

**Complexidade:** Alta

#### 3. Caddy (Reverse Proxy Simples)

**Vantagens:**
- SSL automático (Let's Encrypt)
- Config HTTP/2, HTTP/3
- Simples de configurar

**Setup:**
```bash
# Caddyfile
dokploy.seudominio.com {
    reverse_proxy 172.23.39.241:3000
}

# SSL automático!
```

**Complexidade:** Baixa-Média

#### 4. IP Direto (NÃO RECOMENDADO ❌)

```bash
http://172.23.39.241:3000
```

**Problemas:**
- Sem SSL (HTTP inseguro)
- Porta 3000 exposta
- Sem proteção DDoS
- Não profissional

### Recomendação Oficial

**Cloudflare + Subdomínio** ✅

**Por quê:**
1. **Simples** - DNS aponta para IP, Cloudflare cuida do resto
2. **SSL grátis** - Certificado automático
3. **Protegido** - DDoS protection, WOP gratuito
4. **Profissional** - HTTPS, DNS estável
5. **Zero custo** - Plano gratuito é suficiente

### Setup Passo a Passo

```bash
# 1. Cloudflare DNS
# Adicionar registro A:
#   Type: A
#   Name: dokploy
#   IPv4: 172.23.39.241
#   Proxy: ON (nuvem laranja)
#   TTL: Auto

# 2. Aguardar propagação (5-10 min)

# 3. Acessar Dokploy
http://dokploy.seudominio.com

# 4. Configurar domínio no Dokploy
# Settings → Domains → Add domain
# Domain: dokploy.seudominio.com
# Cloudflare vai gerar SSL
```

### Alternativa: Domínio Próprio + Let's Encrypt

Se não usar Cloudflare:

```bash
# Instalar Caddy
sudo apt install caddy

# Caddyfile
dokploy.seudominio.com {
    reverse_proxy localhost:3000
}

# SSL automático via Let's Encrypt!
sudo systemctl restart caddy
```

---

## 🗄️ Acesso ao Banco de Dados - Ferramentas

### PostgreSQL (16.14)

#### 1. DBeaver (RECOMENDADO ✅)

**Vantagens:**
- Opensource gratuito
- Suporta todos os bancos (Postgres, MySQL, etc.)
- UI rica e moderna
- ER diagrams automático
- SSH tunnel integrado

**Setup:**
```bash
# Instalar
sudo apt install dbeaver-ce  # Community Edition
# OU
snap install dbeaver-ce

# Conexão
# Host: 172.23.39.241
# Port: 5432
# Database: dokploy
# User: dokploy
# Password: [verificar]
# SSH Tunnel: ceops@172.23.39.241 → localhost:5432
```

**Download:** https://dbeaver.io/download/

#### 2. pgAdmin 4 (Web-based)

**Vantagens:**
- Oficial PostgreSQL
- Web UI (acessa via browser)
- Recursos completos

**Setup via Docker:**
```bash
docker run -p 5050:80 \
  -e PGADMIN_DEFAULT_EMAIL=admin@ceops.net \
  -e PGADMIN_DEFAULT_PASSWORD=admin \
  dpage/pgadmin4
```

**Acesso:** http://172.23.39.241:5050

#### 3. psql CLI (Nativo)

**Vantagens:**
- Leve, rápido
- Já instalado no container
- Ideal para quick checks

**Setup:**
```bash
# Via SSH
ssh ceops@172.23.39.241
sudo docker exec -it dokploy-postgres.1.n975qtu3f9uu1iek29nklmw3 psql -U dokploy -d dokploy

# Queries úteis
\dt                    # Listar tabelas
\du                    # Listar usuários
SELECT * FROM "user";  # Ver usuários Dokploy
\q                     # Sair
```

#### 4. TablePlus (Mac/Windows)

**Vantagens:**
- UI moderna
- Multi-DB
- SSH tunnel

**Preço:** Pago (~$59)

**Download:** https://tableplus.com/

### Recomendação PostgreSQL

**DBeaver** ✅

**Por quê:**
- Opensource gratuito
- UI rica e moderna
- SSH tunnel integrado
- Multi-banco (útil para outros projetos)

---

### Redis (7.4.9)

#### 1. RedisInsight (RECOMENDADO ✅)

**Vantagens:**
- Oficial Redis
- Web UI moderna
- Browser de keys
- Graficos em tempo real
- Gratis

**Setup via Docker:**
```bash
docker run -d --name redisinsight -p 8001:8001 redislabs/redisinsight
```

**Acesso:** http://172.23.39.241:8001

**Conexão:**
```
Host: 172.23.39.241
Port: 6379
(ou via SSH tunnel se Redis não estiver exposto)
```

#### 2. redis-cli (Naturo)

**Vantagens:**
- Leve, rápido
- Já instalado

**Setup:**
```bash
# Via SSH
ssh ceops@172.23.39.241
sudo docker exec -it dokploy-redis.1.u45xs1oehrmyosri6xnazf5n2 redis-cli

# Comandos úteis
KEYS *                 # Listar keys
GET bull:deployments:*  # Ver valor
INFO server            # Info Redis
MONITOR                # Ver comandos em tempo real
exit                   # Sair
```

#### 3. TablePlus

Suporta Redis também!

### Recomendação Redis

**RedisInsight** ✅

**Por quê:**
- Oficial Redis
- Web UI moderna
- Browser visual de keys
- Totalmente gratuito

---

## 🔐 Acesso SSH - Ferramentas

### 1. OpenClaw Nodes (RECOMENDADO ✅)

**Vantagens:**
- Já configurado
- Comandos via message
- Integrado com o sistema
- Seguro (tokens)

**Uso:**
```bash
# Via Telegram
@ceops_bot
nodes --action describe --node=.241

# Via CLI
nodes --action camera_snap --node=.241
```

**Setup:** Já configurado ✅

### 2. SSH CLI (Nativo)

**Vantagens:**
- Universal
- Leve
- Scriptável

**Setup:**
```bash
# SSH direto
ssh ceops@172.23.39.241

# Com comandos
ssh ceops@172.23.39.241 "docker ps | grep dokploy"

# Via SSH config
cat ~/.ssh/config
Host ceops241
    HostName 172.23.39.241
    User ceops
    # IdentityFile ~/.ssh/id_ceops
```

### 3. Visual Studio Code (Remote SSH)

**Vantagens:**
- Editor completo
- Terminal integrado
- Extensions (Docker, PostgreSQL)
- SFTP nativo

**Setup:**
```bash
# Instalar extensão "Remote - SSH"
# Ctrl+Shift+P → "Remote-SSH: Connect to Host"
# Adicionar: ssh ceops@172.23.39.241

# Salvar em ~/.ssh/config
Host ceops241
    HostName 172.23.39.241
    User ceops
```

### 4. Termius (Desktop + Mobile)

**Vantagens:**
- Cross-platform (Desktop, iOS, Android)
- UI bonita
- SFTP integrado
- Snippets

**Setup:**
```bash
# Download: https://termius.com/
# Adicionar host:
# Host: 172.23.39.241
# User: ceops
# Password: [Bitwarden]
```

### 5. Mosh (Mobile SSH)

**Vantagens:**
- Reconnect automático
- Lag tolerance
- Ideal para mobile

**Setup:**
```bash
# Instalar mosh
sudo apt install mosh

# Conectar
mosh ceops@172.23.39.241
```

### Recomendação SSH

**Combo:** OpenClaw Nodes + VS Code Remote SSH

**Por quê:**
- **OpenClaw:** Comandos rápidos via Telegram, já configurado
- **VS Code:** Edição completa, terminal integrado, extensões
- **SSH CLI:** Fallback universal

---

## 🎯 Resumo - Setup Recomendado

### Domínio
- **Cloudflare** + subdomínio ✅
- `dokploy.seudominio.com` → SSL automático

### Banco de Dados
- **PostgreSQL:** DBeaver ✅
- **Redis:** RedisInsight ✅

### SSH
- **OpenClaw Nodes:** Comandos via Telegram ✅
- **VS Code Remote SSH:** Edição completa ✅
- **SSH CLI:** Fallback universal

---

## 📦 Setup Completo - Exemplo Prático

```bash
# 1. Configurar domínio Cloudflare
# dokploy.seudominio.com → 172.23.39.241

# 2. Instalar DBeaver
sudo apt install dbeaver-ce
# Conectar: ceops@172.23.39.241 → localhost:5432 → dokploy/dokploy

# 3. Instalar RedisInsight
docker run -d --name redisinsight -p 8001:8001 redislabs/redisinsight
# Acessar: http://172.23.39.241:8001

# 4. Configurar VS Code Remote SSH
# Ctrl+Shift+P → "Remote-SSH: Connect to Host"
# ssh ceops@172.23.39.241

# 5. Testar tudo
# - Domínio: curl https://dokploy.seudominio.com
# - Postgres: dbeaver (conectar SSH tunnel)
# - Redis: http://172.23.39.241:8001
# - SSH: code --remote ssh-remote+ceops241
```

---

Ω · Denalth
