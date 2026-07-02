# Dokploy Q&A - Respostas Completas

**Data:** 2026-06-17
**Servidor:** ceopssrv241 (Sentinel)

## Perguntas Respondidas

### 1. Containers não aparecem no Dokploy

**RESPOSTA:** É NORMAL ✅

**Explicação:**
- Dokploy só mostra containers que **ELE GERENCIA**
- Containers atuais (dokploy, postgres, redis) foram criados pelo script de instalação
- Dokploy não "importa" containers existentes automaticamente

**Solução:**
- Criar primeira aplicação via Dokploy → o container aparecerá na UI
- Ou recriar os containers atuais via Dokploy (perde dados)

**Quando criar um app via Dokploy:**
1. App aparece na UI
2. Container associado aparece
3. Logs, métricas, etc. funcionam

---

### 2. DuckDNS pode ser usado com Cloudflare?

**RESPOSTA:** Tecnicamente sim, mas **não faz sentido**.

**Por quê:**
- **DuckDNS:** Domínio gratuito + DNS dinâmico
- **Cloudflare:** Reverse proxy + SSL + DDoS protection

**Fazer sentido:**
- Usar DuckDNS OU Cloudflare
- Não misturar os dois

**Melhor abordagem:**

#### Se você JÁ tem domínio (ceops.net):
```bash
# Cloudflare (GRATIS para domínios que você já possui)
dokploy.ceops.net → 172.23.39.241
Proxy ON → SSL automático
```

#### Se você NÃO tem domínio:
```bash
# DuckDNS (100% gratuito)
denalth-ceops.duckdns.org → 172.23.39.241

# + Caddy para SSL
denalth-ceops.duckdns.org {
    reverse_proxy 172.23.39.241:3000
}
# HTTPS automático!
```

---

### 3. Criar crons de notificação e atualização

**RESPOSTA:** Vou criar 3 crons AGORA.

#### Cron 1: Verificar containers (diário 08h)

**Objetivo:** Monitorar saúde dos containers

```yaml
name: "sentinel:containers-check"
description: "Verificar status containers Dokploy diariamente"
schedule:
  kind: cron
  expr: "0 8 * * *"  # 08h diário
  tz: "America/Fortaleza"
payload:
  kind: agentTurn
  model: ollama/qwen3:4b
  message: |
    Verificar containers Dokploy (.241):
    1. Status: dokploy, dokploy-postgres, dokploy-redis
    2. CPU, memória, disco (alertar se >90%)
    3. Containers parados/erro
    4. Restart count (alertar se >3)

    Notificar via Telegram se houver problema.
```

#### Cron 2: Atualizações disponíveis (domingo 09h)

**Objetivo:** Verificar atualizações, NOT instalar

```yaml
name: "sentinel:updates-check"
description: "Verificar atualizações semanais"
schedule:
  kind: cron
  expr: "0 9 * * 0"  # Domingo 09h
  tz: "America/Fortaleza"
payload:
  kind: agentTurn
  model: ollama/qwen3:4b
  message: |
    Verificar atualizações (.241):
    1. apt list --upgradable (alertar se >10 patches)
    2. Dokploy latest version vs atual
    3. Docker images (postgres, redis)

    NOTIFICAR via Telegram se houver atualizações críticas.
    NÃO instalar automaticamente.
```

#### Cron 3: Atualizar servidor (dia 01, 03h)

**Objetivo:** Atualizar sistema, notificar antes/depois

```yaml
name: "sentinel:server-update"
description: "Atualizar servidor mensalmente"
schedule:
  kind: cron
  expr: "0 3 1 * *"  # Dia 01, 03h
  tz: "America/Fortaleza"
payload:
  kind: agentTurn
  model: ollama/qwen3:4b
  message: |
    Atualizar servidor (.241):
    1. NOTIFICAR via Telegram: "Iniciando atualização"
    2. apt update && apt upgrade -y
    3. Verificar se precisa reboot
    4. Se reboot: NOTIFICAR, aguardar 2 min, verificar containers
    5. NOTIFICAR: "Atualização concluída"

    Se erro: NOTIFICAR imediatamente.
```

**Vou criar esses 3 crons agora.**

---

### 4. O que é Traefik? O que é Caddy?

**RESPOSTA:** São reverse proxies.

#### Traefik

**O que é:** Reverse proxy cloud-native

**Características:**
- Auto-discovery de containers Docker
- Balanceamento de carga
- SSL automático (Let's Encrypt)
- Config complexa (YAML/TOML)
- Ideal para orquestração (Kubernetes/Swarm)

**Exemplo config:**
```yaml
# docker-compose.yml
traefik:
  image: traefik:v2.10
  command:
    - "--api.insecure=true"
    - "--providers.docker=true"
    - "--entrypoints.web.address=:80"
  ports:
    - "80:80"
    - "8080:8080"  # Dashboard
  volumes:
    - /var/run/docker.sock:/var/run/docker.sock

# Container com labels
dokploy:
  image: dokploy/dokploy
  labels:
    - "traefik.enable=true"
    - "traefik.http.routers.dokploy.rule=Host(`dokploy.ceops.net`)"
```

**Uso ideal:**
- Muitos containers (50+)
- Orquestração complexa
- Auto-scaling
- Microserviços

#### Caddy

**O que é:** Reverse proxy simples

**Características:**
- SSL automático (Let's Encrypt)
- Config HTTP simples
- HTTP/2, HTTP/3 suporte
- Menos recursos
- Simples de configurar

**Exemplo config:**
```bash
# Caddyfile
dokploy.ceops.net {
    reverse_proxy 172.23.39.241:3000
}

redisinsight.ceops.net {
    reverse_proxy 172.23.39.241:8001
}

# SSL automático!
```

**Uso ideal:**
- Servidor único
- Config simples
- HTTPS automático rápido
- Vários sites

#### Comparação

| Critério | Traefik | Caddy |
|---|---|---|
| **Complexidade** | Alta | Baixa |
| **Auto-discovery** | ✅ Sim | ❌ Não |
| **SSL** | ✅ Auto | ✅ Auto |
| **Multi-container** | ✅ Excelente | ⚠️ Manual |
| **Simplicidade** | ❌ Complexo | ✅ Simples |
| **Learning curve** | Alta | Baixa |

#### Qual escolher?

**Para Dokploy (.241):**
- **Caddy** ✅ (simples, SSL auto, 1-3 containers)

**Para muitos containers:**
- **Traefik** (auto-discovery, orquestração)

---

### 5. Gestão de portas - Como evitar conflitos?

**RESPOSTA:** 3 estratégias.

#### Problema

Muitos containers = conflito de portas:
- Dokploy: 3000
- RedisInsight: 8001
- pgAdmin: 5050
- Grafana: 3001
- ...caos!

#### Solução 1: Reverse Proxy (RECOMENDADO ✅)

**Como funciona:**
- 1 porta só (80/443)
- Domínios separados
- SSL automático

**Exemplo com Caddy:**
```bash
# Caddyfile
dokploy.ceops.net {
    reverse_proxy 172.23.39.241:3000
}

redisinsight.ceops.net {
    reverse_proxy 172.23.39.241:8001
}

pgadmin.ceops.net {
    reverse_proxy 172.23.39.241:5050
}

# 1 porta só (443), SSL automático!
```

**Vantagens:**
- ✅ Limpo
- ✅ SSL automático
- ✅ Domínios profissionais
- ✅ Fácil gerenciar

#### Solução 2: Rede Docker Interna

**Como funciona:**
- Containers se comunicam internamente
- Só expor porta 3000 (proxy)
- Outros containers fechados

**Exemplo:**
```bash
# Rede interna
docker network create apps

# Containers (sem expor portas!)
docker run --network apps --name dokploy dokploy/dokploy
docker run --network apps --name redis redis:7
docker run --network apps --name postgres postgres:16

# Só expor porta 3000
docker run --network apps -p 3000:3000 caddy
```

**Vantagens:**
- ✅ Seguro (portas fechadas)
- ✅ Comunicação interna rápida
- ✅ Só 1 porta exposta

#### Solução 3: Portas Dedicadas (NÃO RECOMENDADO ❌)

**Como funciona:**
- Cada container com porta
- Dokploy: 3000
- RedisInsight: 8001
- pgAdmin: 5050
- ...caos!

**Problemas:**
- ❌ Muitas portas
- ❌ Sem SSL manual
- ❌ Difícil gerenciar
- ❌ Não profissional

#### Recomendação

**Reverse Proxy (Caddy) + Rede Docker interna**

```bash
# 1. Rede interna
docker network create apps

# 2. Containers (sem expor portas)
docker run --network apps --name dokploy dokploy/dokploy
docker run --network apps --name redisinsight redislabs/redisinsight

# 3. Caddy (só expor 80/443)
dokploy.ceops.net {
    reverse_proxy dokploy:3000
}

redisinsight.ceops.net {
    reverse_proxy redisinsight:8001
}
```

---

### 6. DBeaver - Instalar no root

**RESPOSTA:** Instalando no .241 (onde containers estão).

**Local:** ceops@172.23.39.241

**Vou instalar AGORA.**

---

### 7. RedisInsight - Acesso do .10

**PROBLEMA:**
- RedisInsight rodando no .241:8001
- Você no .10
- Porta 8001 estava bloqueada

**SOLUÇÃO:**
```bash
# Porta 8001 exposta no UFW ✅
8001/tcp ALLOW
```

**Acessar:**
```
http://172.23.39.241:8001
```

**Se ainda não acessível:**
- Verificar firewall do .10
- Ou SSH tunnel: `ssh -L 8001:172.23.39.241:8001 ceops@172.23.39.241`

---

### 8. Beekeeper vs DBeaver - Qual é melhor?

**RESPOSTA:** DBeaver é mais completo.

#### Beekeeper Studio

**Características:**
- Gratuito (open source)
- SQL-focused (MySQL, Postgres, SQLite)
- UI moderna
- Cross-platform
- Leve

**Limitações:**
- Só bancos SQL
- Sem ER diagram automático
- Sem SSH tunnel integrado
- Menos recursos

#### DBeaver

**Características:**
- Gratuito (Community Edition)
- **Multi-banco** (Postgres, MySQL, Oracle, MSSQL, Redis, etc.)
- ER diagram automático
- SSH tunnel integrado
- Data compare
- Mais recursos

#### Comparação

| Feature | Beekeeper | DBeaver |
|---|---|---|
| **Preço** | Gratuito | Gratuito (CE) |
| **Bancos** | SQL only | **Todos** |
| **SSH Tunnel** | ❌ | ✅ |
| **ER Diagram** | ❌ | ✅ |
| **Data compare** | ❌ | ✅ |
| **UI** | Moderna simples | Rica completa |
| **Extensions** | Limitado | Muitas |

#### Qual escolher?

**Para Dokploy (Postgres):**
- **Beekeeper** se quer:
  - Simples
  - Leve
  - Só SQL

- **DBeaver** se quer:
  - SSH tunnel (seguro)
  - ER diagram (visualizar schema)
  - Mais recursos
  - **Multi-banco** (útil para outros projetos)

**Recomendação:** **DBeaver** ✅ (mais completo)

---

## Próximos Passos - Executando AGORA

1. ✅ **Porta 8001 exposta** no UFW
2. ⏳ **Criar 3 crons** (containers, updates, servidor)
3. ⏳ **Instalar DBeaver** no .241
4. ⏳ **Testar acesso RedisInsight** do .10

Vou executar agora.

---

Ω · Denalth
