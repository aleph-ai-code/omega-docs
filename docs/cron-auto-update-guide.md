# Cron Auto-Update Guide

**Data:** 2026-06-17
**Servidor:** ceopssrv241 (Sentinel)

## 🤖 Atualização Automática - Crons

### Opções Disponíveis

#### 1. Sistema Operacional (apt upgrade)

**Recomendado:** ⚠️ **CUIDADO** - atualização automática pode quebrar serviços

```bash
# Cron: Semanal (domingo 03h)
# Verifica atualizações, mas NÃO instala automaticamente
03 03 * * 0 apt update && apt list --upgradable > /var/log/apt-updates.log
# Notifica via Telegram se houver atualizações críticas
```

#### 2. Docker Containers (Recomendado ✅)

**Dokploy:**
```bash
# Cron: Semanal (domingo 04h)
# Verifica nova versão, notifica, NÃO atualiza automaticamente
```

**Imagens base (Postgres/Redis):**
```bash
# Cron: Mensal (dia 01 02h)
# Verifica atualizações, notifica
```

### Política Recomendada

**NOTIFY ONLY** ✅ (Não atualizar automaticamente)

**Por quê:**
- Atualização pode quebrar serviços
- Precisa de backup antes
- Precisa de janela de manutenção
- Teste manual antes

### Cron Job Exemplo

```yaml
name: "sentinel:auto-update-check"
description: "Verificar atualizações semanais (.241)"
schedule:
  kind: cron
  expr: "0 4 * * 0"  # Domingo 04h
  tz: "America/Fortaleza"
payload:
  kind: agentTurn
  model: ollama/qwen3:4b
  message: |
    Verificar atualizações disponíveis:
    1. Sistema (apt)
    2. Dokploy (GitHub API)
    3. Docker images

    Se houver atualizações críticas, notificar via Telegram.
    NÃO instalar atualizações automaticamente.
```

### Script de Verificação

```bash
#!/bin/bash
# /usr/local/bin/check-updates

echo "=== Sistema ==="
apt update >> /var/log/apt-update.log 2>&1
UPDATES=$(apt list --upgradable 2>/dev/null | grep -v "WARNING" | wc -l)

echo "=== Dokploy ==="
LATEST=$(curl -s https://api.github.com/repos/dokploy/dokploy/releases/latest | jq -r '.tag_name')
CURRENT=$(docker ps --format '{{.Image}}' | grep dokploy | cut -d: -f2)

echo "=== Docker Images ==="
docker pull postgres:16 >> /var/log/docker-pull.log 2>&1
docker pull redis:7 >> /var/log/docker-pull.log 2>&1

if [ "$LATEST" != "$CURRENT" ]; then
  echo "Nova versão Dokploy: $LATEST (atual: $CURRENT)"
  # Notificar Telegram
fi
```

---

## 💻 DBeaver - Instalação

**Resposta:** DBeaver instala **DIRETO na máquina** (nativo), NÃO em container.

### Por quê?

- DBeaver é uma aplicação desktop Java
- Precisa de UI gráfica (X11/Wayland)
- Usuário abre na própria máquina

### Instalação

```bash
# Opção 1: Apt (Recomendado)
wget -O - https://dbeaver.io/debs/dbeaver.gpg.key | sudo apt-key add -
echo "deb https://dbeaver.io/debs/ /" | sudo tee /etc/apt/sources.list.d/dbeaver.list
sudo apt update
sudo apt install dbeaver-ce

# Opção 2: Snap
sudo snap install dbeaver-ce

# Opção 3: Download direto
wget https://dbeaver.io/files/dbeaver-ce-latest-stable.x86_64.deb
sudo dpkg -i dbeaver-ce-latest-stable.x86_64.deb
```

### Uso

```bash
# Abrir DBeaver
dbeaver &

# Ou via menu de aplicações
# → Development → DBeaver
```

---

## 🌐 Domínios Gratuitos - Opções

### 1. DuckDNS (RECOMENDADO ✅)

**Vantagens:**
- 100% gratuito
- Subdomínios ilimitados
- DNS dinâmico (IP muda)
- SSL via Let's Encrypt (manual)
- Sem renovação

**Setup:**
```bash
# 1. Criar conta em duckdns.org
# 2. Criar subdomínio: ceops.duckdns.org
# 3. Atualizar IP automaticamente

# Cron (5 minutos)
*/5 * * * * curl "https://www.duckdns.org/update/domains/ceops/tokens/<TOKEN>/ip/"
```

**Resultado:** `ceops.duckdns.org` → 172.23.39.241

**Dokploy:** `dokploy.ceops.duckdns.org` (não suporta wildcard)

### 2. FreeDNS (freedns.afraid.org)

**Vantagens:**
- 100% gratuito
- Subdomínios de usuários compartilhados
- Vários domínios públicos disponíveis

**Setup:**
```bash
# 1. Criar conta
# 2. Escolher subdomínio público (ex: ceops.mooo.com)
# 3. Configurar IP
```

**Resultado:** `ceops.mooo.com` → 172.23.39.241

### 3. No-IP (no-ip.com)

**Vantagens:**
- Grátis
- DNS dinâmico

**Desvantagens:**
- Renovação mensal obrigatória (30 dias)
- Se esquecer, perde o domínio

### 4. Cloudflare (se já tem domínio)

**Se você já tem ceops.net:**

```bash
# Cloudflare é GRÁTIS para domínios que você já possui
# Setup:
# 1. Adicionar ceops.net no Cloudflare
# 2. Criar subdomínio: dokploy.ceops.net
# 3. Proxy ON → SSL automático
```

### 5. Path-based (/dokploy) - NÃO RECOMENDADO ❌

**Por quê:**
- Problemas com WebSocket
- Problemas com assets estáticos
- Dokploy não suporta bem
- Complexo de configurar

**Alternativa:**
```bash
# Usar Caddy com path-based
dokploy.ceops.net/dokploy {
    reverse_proxy 172.23.39.241:3000
}
```

⚠️ **Ainda assim, não é ideal**

---

## 🎯 Recomendação Oficial

### Domínio Gratuito: **DuckDNS** ✅

**Setup completo:**
```bash
# 1. Criar conta em duckdns.org
# 2. Criar subdomínio: denalth-ceops.duckdns.org
# 3. Configurar atualização automática

# Script de atualização
cat > /usr/local/bin/duckdns-update.sh << 'EOF'
#!/bin/bash
DOMAIN="denalth-ceops"
TOKEN="<seu-token>"
curl "https://www.duckdns.org/update?domains=$DOMAIN&token=$TOKEN&ip="
EOF

chmod +x /usr/local/bin/duckdns-update.sh

# Cron (5 minutos)
*/5 * * * * /usr/local/bin/duckdns-update.sh >> /var/log/duckdns.log 2>&1
```

**Dokploy:**
- Criar ANOTHER subdomínio: `denalth-dokploy.duckdns.org`
- Ou usar: `denalth-ceops.duckdns.org/dokploy` ❌ (não ideal)

**Limitação DuckDNS:**
- Sem wildcard DNS
- 1 subdomínio = 1 entrada no DuckDNS
- **Solução:** Criar `denalth-dokploy.duckdns.org` separado

---

## 📦 Instalando RedisInsight AGORA

Vou instalar o RedisInsight agora mesmo! 👇

---

Ω · Denalth
