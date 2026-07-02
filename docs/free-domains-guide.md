# Domínios Gratuitos - Guia Completo

**Data:** 2026-06-17
**Objetivo:** Setup Dokploy com domínio gratuito

## 🎯 Resumo das Perguntas Respondidas

### 1. ✅ Cron para atualização do sistema e containers?

**SIM!** Mas recomendo **NOTIFY ONLY** (não atualizar automaticamente).

**Por quê:**
- Atualização pode quebrar serviços
- Precisa de backup antes
- Precisa de janela de manutenção

**Recomendação:**
- Verificar atualizações semanalmente
- Notificar via Telegram se houver críticas
- NUNCA atualizar automaticamente sem aprovação

### 2. 💻 DBeaver instala em container ou direto na máquina?

**DIRETO na máquina** (nativo), não em container.

**Instalação:**
```bash
wget -O - https://dbeaver.io/debs/dbeaver.gpg.key | sudo apt-key add -
echo "deb https://dbeaver.io/debs/ /" | sudo tee /etc/apt/sources.list.d/dbeaver.list
sudo apt update
sudo apt install dbeaver-ce
```

### 3. ✅ RedisInsight instalado!

**Status:** Rodando na porta 8001

```bash
# Acessar
http://172.23.39.241:8001

# Conectar ao Redis Dokploy
Host: 172.23.39.241
Port: 6379
Name: Dokploy Redis
```

### 4. 🌐 Domínios gratuitos - Opções reais

**SIM!** Várias opções 100% gratuitas:

---

## 🆓 Domínios Gratuitos - Opções Detalhadas

### 1. DuckDNS (RECOMENDADO ✅)

**Vantagens:**
- 100% gratuito
- DNS dinâmico (IP muda automaticamente)
- Atualização via API simples
- Sem renovação mensal
- Vários subdomínios

**Desvantagens:**
- SSL manual (Let's Encrypt)
- Sem wildcard DNS

**Setup Passo a Passo:**

```bash
# 1. Criar conta em https://www.duckdns.org
# Email + senha (sem confirmação de email)

# 2. Criar subdomínio
# Domain: denalth-ceops
# Resultado: denalth-ceops.duckdns.org

# 3. Obter token
# Settings → Token (copiar)

# 4. Script de atualização automática
cat > /usr/local/bin/duckdns-update.sh << 'EOF'
#!/bin/bash
DOMAIN="denalth-ceops"
TOKEN="seu-token-aqui"
curl -s "https://www.duckdns.org/update?domains=$DOMAIN&token=$TOKEN&ip=" >> /var/log/duckdns.log 2>&1
EOF

chmod +x /usr/local/bin/duckdns-update.sh

# 5. Testar manual
/usr/local/bin/duckdns-update.sh
# Deve retornar: OK ou <ip>

# 6. Adicionar cron (5 minutos)
crontab -e
# Adicionar linha:
*/5 * * * * /usr/local/bin/duckdns-update.sh >> /var/log/duckdns.log 2>&1
```

**Para Dokploy:**

```bash
# Opção A: Criar outro subdomínio DuckDNS
# denalth-dokploy.duckdns.org

# Opção B: Usar Caddy com path (não ideal)
denalth-ceops.duckdns.org/dokploy
```

**SSL com Let's Encrypt:**

```bash
# Instalar Caddy
sudo apt install caddy

# Caddyfile
denalth-ceops.duckdns.org {
    reverse_proxy 172.23.39.241:3000
}

# SSL automático!
sudo systemctl restart caddy
```

---

### 2. FreeDNS (freedns.afraid.org)

**Vantagens:**
- 100% gratuito
- Vários domínios públicos
- Subdomínios compartilhados

**Desvantagens:**
- Interface confusa
- Alguns domínios exigem confirmação
- Menos profissional

**Setup:**

```bash
# 1. Criar conta em https://freedns.afraid.org/

# 2. Escolher subdomínio público
# Exemplos:
# - ceops.mooo.com
# - denalth.githubusercontent.com (se tiver GitHub)
# - ceops.duckdns.org (mesmo conceito)

# 3. Configurar IP (manual ou script)
```

---

### 3. No-IP (no-ip.com)

**Vantagens:**
- Grátis
- DNS dinâmico

**Desvantagens:**
- **Renovação mensal OBRIGATÓRIA** (30 dias)
- Se esquecer, perde o domínio
- Interface cheia de anúncios

**Setup:**

```bash
# 1. Criar conta
# 2. Criar hostname: ceops.ddns.net
# 3. Instalar cliente No-IP (ou curl)
```

**⚠️ Não recomendo** - fácil esquecer renovação

---

### 4. Cloudflare (se já tem domínio ceops.net)

**Vantagens:**
- SSL automático
- Proxy/DDoS protection
- DNS fácil
- **Se já tem domínio, é GRÁTIS**

**Setup:**

```bash
# 1. Adicionar ceops.net no Cloudflare
# 2. Criar registro A:
#    Type: A
#    Name: dokploy
#    IPv4: 172.23.39.241
#    Proxy: ON (nuvem laranja)

# 3. Pronto! SSL automático
```

**Resultado:** `dokploy.ceops.net` → HTTPS automático

---

### 5. Path-based (/dokploy) - NÃO RECOMENDADO ❌

**Por quê NÃO usar:**

- Problemas com WebSocket
- Assets estáticos 404
- Dokploy não suporta bem
- Complexo de configurar
- Não profissional

**Se não tiver opção:**

```bash
# Usar Caddy com path
ceops.net {
    handle /dokploy* {
        reverse_proxy 172.23.39.241:3000
    }
}
```

**⚠️ Ainda assim, não é ideal**

---

## 🎯 Minha Recomendação

### Se você JÁ tem domínio (ceops.net):

**Cloudflare** ✅

- Adicionar ceops.net no Cloudflare
- Criar `dokploy.ceops.net`
- SSL automático
- Zero custo

### Se você NÃO tem domínio:

**DuckDNS** ✅

1. Criar conta DuckDNS (grátis)
2. Criar `denalth-ceops.duckdns.org`
3. Instalar Caddy para SSL
4. Dokploy acessível via HTTPS

**Setup completo DuckDNS + Caddy:**

```bash
# 1. DuckDNS account
# denalth-ceops.duckdns.org → 172.23.39.241

# 2. Instalar Caddy
sudo apt install caddy

# 3. Caddyfile
echo "denalth-ceops.duckdns.org {
    reverse_proxy 172.23.39.241:3000
}" | sudo tee /etc/caddy/Caddyfile

# 4. Restart Caddy
sudo systemctl restart caddy

# 5. Pronto! HTTPS automático
```

---

## 📦 RedisInsight - JÁ INSTALADO! ✅

**Status:** Rodando agora

```bash
# Acessar
http://172.23.39.241:8001

# Conectar ao Redis Dokploy
Host: 172.23.39.241
Port: 6379
Name: Dokploy Redis
```

**Container:**
- `redisinsight` → porta 8001
- Restart: always

---

## 🚀 Próximos Passos

1. **Escolher domínio:**
   - DuckDNS (se não tem domínio)
   - Cloudflare (se já tem ceops.net)

2. **Configurar domínio:**
   - DuckDNS: script de atualização + Caddy
   - Cloudflare: A record → Proxy ON

3. **Testar:**
   - `curl https://seu-dominio.com`
   - Acessar Dokploy via HTTPS

---

Ω · Denalth
