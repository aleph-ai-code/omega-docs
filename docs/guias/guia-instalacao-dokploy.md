# Guia de Instalação via Dokploy — .241

> Ω · Denalth
> 2006-06-18

## Pré-requisitos

✅ Diretórios criados:
- `/root/data/code-server`
- `/root/data/hedgedoc`
- `/root/data/filebrowser`
- `/root/data/dashboard`
- `/root/workspace`

✅ Dashboard copiado:
- `/root/.openclaw/workspace/docs/dashboard/index.html`

---

## Opção A: Via Dokploy (Recomendado)

### 1. Acessar Dokploy

```
http://172.23.39.241:3000
```

### 2. Para cada serviço:

#### Dashboard

1. **Create Project** → **Compose Project**
2. **Project name:** `omega-dashboard`
3. **Repository:** (não needed - local compose)
4. **Compose file:** Colar conteúdo de `docker-compose-241.yml`
5. **Deploy**

#### HedgeDoc

1. **Create Project** → **Compose Project**
2. **Project name:** `hedgedoc`
3. **Compose file:** Colar conteúdo de `composes/hedgedoc.yml`
4. **Deploy**
5. **Configurar:** Primeiro acesso vai pedir setup

#### code-server

1. **Create Project** → **Compose Project**
2. **Project name:** `code-server`
3. **Compose file:** Colar conteúdo de `composes/code-server.yml`
4. **Deploy**
5. **Senha:** `omega2024`

---

## Opção B: Manual (Teste antes de Dokploy)

### Testar containers individualmente:

```bash
# Dashboard
docker run -d --name omega-dashboard \
  --restart unless-stopped \
  -p 80:80 \
  -v /root/.openclaw/workspace/docs/dashboard/index.html:/usr/share/nginx/html/index.html:ro \
  nginx:alpine

# HedgeDoc (com PostgreSQL)
docker run -d --name hedgedoc-db \
  -e POSTGRES_USER=hedgedoc \
  -e POSTGRES_PASSWORD=hedgedoc-password \
  -e POSTGRES_DB=hedgedoc \
  -v /root/data/hedgedoc/db:/var/lib/postgresql/data \
  postgres:15-alpine

docker run -d --name hedgedoc \
  --link hedgedoc-db:database \
  -p 3001:3001 \
  -e CMD_DBURL=postgres://hedgedoc:hedgedoc-password@database:5432/hedgedoc \
  -e CMD_DOMAIN=172.23.39.241:3001 \
  -v /root/data/hedgedoc/uploads:/hedgedoc/public/uploads \
  quay.io/hedgedoc/hedgedoc:latest

# code-server
docker run -d --name code-server \
  --restart unless-stopped \
  -p 8080:8080 \
  -v /root/data/code-server:/home/coder/.local/share/code-server \
  -v /root/workspace:/workspace \
  -e PASSWORD=omega2024 \
  codercom/code-server:latest
```

---

## Compose Files Criados

### 1. Dashboard
**Localização:** `/root/.openclaw/workspace/docs/dashboard/docker-compose-241.yml`

**Conteúdo:**
```yaml
version: '3.8'

services:
  dashboard:
    image: nginx:alpine
    container_name: omega-dashboard
    restart: unless-stopped
    ports:
      - "80:80"
    volumes:
      - /root/.openclaw/workspace/docs/dashboard/index.html:/usr/share/nginx/html/index.html:ro
    networks:
      - dokploy-network

networks:
  dokploy-network:
    external: true
```

### 2. HedgeDoc
**Localização:** `/root/.openclaw/workspace/docs/composes/hedgedoc.yml`

**Conteúdo:**
```yaml
version: '3.8'

services:
  database:
    image: postgres:15-alpine
    container_name: hedgedoc-db
    restart: unless-stopped
    environment:
      - POSTGRES_USER=hedgedoc
      - POSTGRES_PASSWORD=hedgedoc-password
      - POSTGRES_DB=hedgedoc
    volumes:
      - /root/data/hedgedoc/db:/var/lib/postgresql/data
    networks:
      - dokploy-network

  hedgedoc:
    image: quay.io/hedgedoc/hedgedoc:latest
    container_name: hedgedoc
    restart: unless-stopped
    depends_on:
      - database
    environment:
      - CMD_DBURL=postgres://hedgedoc:hedgedoc-password@database:5432/hedgedoc
      - CMD_DOMAIN=172.23.39.241:3001
      - CMD_PROTOCOL_USESSL=false
      - CMD_ALLOW_ORIGIN=*
    ports:
      - "3001:3001"
    volumes:
      - /root/data/hedgedoc/uploads:/hedgedoc/public/uploads
    networks:
      - dokploy-network

networks:
  dokploy-network:
    external: true
```

### 3. code-server
**Localização:** `/root/.openclaw/workspace/docs/composes/code-server.yml`

**Conteúdo:**
```yaml
version: '3.8'

services:
  code-server:
    image: codercom/code-server:latest
    container_name: code-server:
      - /root/data/code-server:/home/coder/.local/share/code-server
      - /root/workspace:/workspace
    environment:
      - PASSWORD=omega2024
    networks:
      - dokploy-network

networks:
  dokploy-network:
    external: true
```

---

## Acesso Pós-Instalação

- **Dashboard:** http://172.23.39.241
- **HedgeDoc:** http://172.23.39.241:3001
- **code-server:** http://172.23.39.241:8080
- **Dokploy:** http://172.23.39.241:3000

---

## Troubleshooting

### Porta 80 já em uso?

```bash
# Verificar o que usa porta 80
sudo netstat -tlnp | grep :80

# Se algo usar, mudar porta do Dashboard para 8080
# No compose: "8080:80" ao invés de "80:80"
```

### HedgeDoc não conecta no DB?

```bash
# Verificar logs
docker logs hedgedoc

# Verificar se DB está rodando
docker ps | grep hedgedoc-db
```

---

## Próximo Passo: Padrão Global

Após testar no .241, replicar FileBrowser + HedgeDoc para:
- .240 (Omega)
- .250 (Aleph)
- .251 (Voyager)

**Usar mesmos composes** (adaptar IP se necessário)
