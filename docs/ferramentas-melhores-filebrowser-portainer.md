# FERRAMENTAS MELHORES - Arquivos e Portas

**Data:** 2026-06-18 02:55
**Objetivo:** Ferramentas visuais para gestão de arquivos e portas

---

## 1. GESTÃO DE ARQUIVOS - MELHOR QUE HTTP SERVER

### ❌ Problema atual

```bash
python3 -m http.server 8088
```

**Limitações:**
- ❌ Só visualização
- ❌ Sem edição
- ❌ Sem upload
- ❌ Sem gerenciamento
- ❌ Sem preview markdown
- ❌ Sem autenticação

---

### ✅ Solução: **Filebrowser** ⭐

**O que é:**
- Gestor de arquivos web completo
- Interface visual moderna
- Edição de arquivos inline
- Upload/download
- Preview de markdown
- Autenticação por usuário
- Permissões por diretório

**Características:**
- ✅ Open source
- ✅ Docker image disponível
- ✅ Leve (single binary)
- ✅ Autenticação built-in
- ✅ Editor de arquivos embutido
- ✅ Preview markdown
- ✅ Upload/download
- ✅ Criar/renomear/deletar arquivos
- ✅ Múltiplos usuários

**Setup (5 min):**

```bash
# Docker compose
docker run -d \
  --name filebrowser \
  -p 8089:80 \
  -v /root/.openclaw/workspace:/srv \
  -v /root/.config/filebrowser:/config \
  -e FB_BASEURL=/ \
  filebrowser/filebrowser:latest

# Acessar
http://172.23.39.241:8089

# Login padrão (primeiro acesso)
admin/admin
```

**Funcionalidades:**

| Funcionalidade | Status |
|---|---|
| Navegar diretórios | ✅ |
| Criar/editar arquivos | ✅ |
| Upload/download | ✅ |
| Preview markdown | ✅ |
| Editor código | ✅ |
| Syntax highlighting | ✅ |
| Múltiplos usuários | ✅ |
| Permissões | ✅ |
| Buscar arquivos | ✅ |
| Renomear/deletar | ✅ |
| Autenticação | ✅ |

---

### Alternativa: **VS Code Server** (code-server)

**O que é:**
- VS Code completo na web
- Edição profissional
- Extensões
- Terminal integrado
- Git integrado

**Setup:**

```bash
docker run -d \
  --name code-server \
  -p 8090:8080 \
  -v /root/.openclaw/workspace:/home/coder/project \
  -e PASSWORD=seu-senha \
  codercom/code-server:latest

# Acessar
http://172.23.39.241:8090
```

**Comparação:**

| Critério | Filebrowser | code-server |
|---|---|---|
| **Leveza** | ✅ Muito leve | ⚠️ Pesado |
| **Simplicidade** | ✅ Simples | ⚠️ Complexo |
| **Edição markdown** | ✅ Preview | ✅ Full |
| **Upload/download** | ✅ Nativo | ⚠️ Via terminal |
| **Autenticação** | ✅ Built-in | ✅ Senha |
| **Uso de memória** | ✅ ~50MB | ⚠️ ~500MB+ |
| **Setup** | ✅ 5 min | ⚠️ 10 min |

**Recomendação:** **Filebrowser** ✅

---

## 2. GESTÃO DE PORTAS/DOCKER - MELHOR QUE SCRIPT

### ❌ Problema atual

```bash
port-manager.sh list
port-manager.sh check 3005
cat docs/ports-inventory.md
```

**Limitações:**
- ❌ Terminal only
- ❌ Sem visualização gráfica
- ❌ Sem interface web
- ❌ Sem gestão visual de containers
- ❌ Sem verificação de portas em tempo real

---

### ✅ Solução: **Portainer** ⭐

**O que é:**
- UI web completa para Docker
- Gestão visual de containers
- Visualização de portas em uso
- Gestão de volumes, redes, images
- Templates de deploy
- Logs em tempo real

**Características:**
- ✅ Open source
- ✅ Docker image
- ✅ Interface visual moderna
- ✅ Gestão completa Docker
- ✅ Visualização de portas
- ✅ Gestão de volumes
- ✅ Gestão de redes
- ✅ Templates de stacks
- ✅ Logs em tempo real
- ✅ Múltiplos hosts
- ✅ Autenticação
- ✅ Leve

**Setup (5 min):**

```bash
# Volume para dados
docker volume create portainer_data

# Container Portainer
docker run -d \
  --name portainer \
  -p 9000:9000 \
  -p 9443:9443 \
  --restart=unless-stopped \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest

# Acessar
http://172.23.39.241:9000
# OU HTTPS: https://172.23.39.241:9443
```

**Funcionalidades:**

| Funcionalidade | Status |
|---|---|
| Visualizar containers | ✅ |
| Ver portas em uso | ✅ |
| Criar/stop/remove containers | ✅ |
| Gestão de volumes | ✅ |
| Gestão de redes | ✅ |
| Gestão de images | ✅ |
| Logs em tempo real | ✅ |
| Terminal no container | ✅ |
| Templates de stacks | ✅ |
 | Múltiplos hosts | ✅ |
| Autenticação | ✅ |
| Dashboard | ✅ |

---

### Alternativa: **Yacht** (mais simples)

**O que é:**
- UI simplificada para Docker
- Foco em templates
- 1-click deployments
- Interface limpa

**Setup:**

```bash
docker run -d \
  --name yacht \
  -p 8091:80 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v yacht-config:/config \
  selfhostedfans/yacht:latest

# Acessar
http://172.23.39.241:8091
```

**Comparação:**

| Critério | Portainer | Yacht |
|---|---|---|
| **Completude** | ✅ Completo | ⚠️ Simplificado |
| **Templates** | ✅ Stacks | ✅ 1-click |
| **Visualização portas** | ✅ Detalhada | ✅ Básica |
| **Gestão volumes** | ✅ Sim | ❌ Não |
| **Gestão redes** | ✅ Sim | ❌ Não |
| **Múltiplos hosts** | ✅ Sim | ❌ Não |
| **Leveza** | ✅ Leve | ✅ Muito leve |
| **Setup** | ✅ 5 min | ✅ 5 min |

**Recomendação:** **Portainer** ✅

---

## 📋 COMPARAÇÃO FINAL

### Arquivos

| Critério | HTTP Server | Filebrowser | code-server |
|---|---|---|---|
| Visualização | ✅ | ✅ | ✅ |
| Edição | ❌ | ✅ | ✅ |
| Upload | ❌ | ✅ | ⚠️ |
| Preview MD | ❌ | ✅ | ✅ |
| Autenticação | ❌ | ✅ | ✅ |
| Leveza | ✅ | ✅ | ⚠️ |
| Setup | ✅ | ✅ | ⚠️ |

**Recomendação:** **Filebrowser** (porta 8089)

### Portas/Docker

| Critério | Script | Portainer | Yacht |
|---|---|---|---|
| Visualização portas | ✅ | ✅ | ✅ |
| Interface web | ❌ | ✅ | ✅ |
| Gestão containers | ❌ | ✅ | ✅ |
| Gestão volumes | ❌ | ✅ | ❌ |
| Logs real-time | ❌ | ✅ | ✅ |
| Templates | ❌ | ✅ | ✅ |

**Recomendação:** **Portainer** (porta 9000)

---

## 🚀 SETUP COMPLETO - 10 MIN

### Passo 1: Filebrowser (arquivos)

```bash
# Container filebrowser
docker run -d \
  --name filebrowser \
  -p 8089:80 \
  -v /root/.openclaw/workspace:/srv \
  -v /root/.config/filebrowser:/config \
  -e FB_BASEURL=/ \
  --restart=unless-stopped \
  filebrowser/filebrowser:latest

# Acessar
http://172.23.39.241:8089
```

### Passo 2: Portainer (portas/Docker)

```bash
# Volume para dados
docker volume create portainer_data

# Container Portainer
docker run -d \
  --name portainer \
  -p 9000:9000 \
  -p 9443:9443 \
  --restart=unless-stopped \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest

# Acessar
http://172.23.39.241:9000
```

### Passo 3: Liberar portas UFW

```bash
# Filebrowser
sudo ufw allow from 172.23.39.0/24 to any port 8089 proto tcp

# Portainer
sudo ufw allow from 172.23.39.0/24 to any port 9000 proto tcp
sudo ufw allow from 172.23.39.0/24 to any port 9443 proto tcp

# Verificar
sudo ufw status | grep -E "8089|9000|9443"
```

### Passo 4: Atualizar inventário

```bash
vim docs/ports-inventory.md
```

Adicionar:
```markdown
| **8089** | filebrowser | Filebrowser | Gestão arquivos | 2026-06-18 | ✅ |
| **9000** | portainer | Portainer | UI Docker | 2026-06-18 | ✅ |
| **9443** | portainer | Portainer | UI Docker (SSL) | 2026-06-18 | ✅ |
```

---

## 🎯 RESULTADO FINAL

### Acessos

| Ferramenta | URL | Propósito |
|---|---|---|
| **Filebrowser** | http://172.23.39.241:8089 | Gestão arquivos |
| **Portainer** | http://172.23.39.241:9000 | Gestão Docker/portas |
| **Dokploy** | http://172.23.39.241:3000 | Plataforma deploy |
| **RedisInsight** | http://172.23.39.241:8001 | UI Redis |

### Inventário atualizado

```bash
# Ver portas em uso
port-manager.sh list

# Ver via Portainer (visual)
http://172.23.39.241:9000

# Gerenciar arquivos via Filebrowser
http://172.23.39.241:8089
```

---

## ✅ VANTAGENS

### Filebrowser vs HTTP Server

| Antes (HTTP Server) | Depois (Filebrowser) |
|---|---|
| ❌ Só visualização | ✅ Edição completa |
| ❌ Sem upload | ✅ Upload/download |
| ❌ Sem gerenciamento | ✅ CRUD completo |
| ❌ Sem preview MD | ✅ Preview markdown |
| ❌ Sem autenticação | ✅ Múltiplos usuários |

### Portainer vs Script

| Antes (Script) | Depois (Portainer) |
|---|---|
| ❌ Terminal only | ✅ Interface web |
| ❌ Sem visualização | ✅ Dashboard gráfico |
| ❌ Manual | ✅ Gestão visual |
| ❌ Sem logs | ✅ Logs real-time |
| ❌ Sem templates | ✅ Stacks/templates |

---

## 📚 PRÓXIMO PASSO

Vou instalar **AGORA:**

1. ✅ Filebrowser (porta 8089)
2. ✅ Portainer (porta 9000)
3. ✅ Liberar portas UFW
4. ✅ Atualizar inventário

**Tempo estimado:** 10 minutos

---

Ω · Denalth
