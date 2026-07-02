# Omega Dashboard — Instalação

> Dashboard única para todos os serviços CEOPS
> Ω · Denalth

## Acesso

**Após instalação:** http://172.23.39.240 (porta 80)

## Instalação Rápida (.240)

```bash
# Criar rede
docker network create omega-net

# Ir para diretório
cd /root/.openclaw/workspace/docs/dashboard

# Subir container
docker-compose up -d

# Acessar
firefox http://172.23.39.240
```

## Serviços Monitorados

### .240 (Omega)
- OpenClaw (18789) ✅
- SearXNG (8888) ✅
- FileBrowser (8089) — Em breve
- HedgeDoc (3001) — Em breve

### .241 (Sentinel)
- OpenClaw (18789) ✅
- FileBrowser (8089) ✅
- Dokploy (3000) ✅
- Portainer (9000) ✅
- CloudBeaver (8978) ✅
- RedisInsight (8001) ✅
- HedgeDoc (3001) — Em breve
- code-server (8080) — Em breve

### .250 (Aleph)
- OpenClaw (18789) ✅
- FileBrowser (8089) — Em breve
- HedgeDoc (3001) — Em breve

### .251 (Voyager)
- OpenClaw (18789) ✅
- FileBrowser (8089) — Em breve
- HedgeDoc (3001) — Em breve

### .249 (Storage)
- Online (voltou recentemente)
- Em identificação

### .39 (Atlas)
- Online (voltou recentemente)
- Em identificação (Windows)

## Customização

**Editar:** `index.html`

**Adicionar serviço:**
```html
<a href="http://IP:PORTA" target="_blank" class="service-card">
    <div class="service-icon">ICONE</div>
    <div class="service-name">Nome</div>
    <div class="service-description">Descrição</div>
    <div class="service-url">http://IP:PORTA</div>
    <span class="service-port">Porta PORTA</span>
    <span class="status-badge status-online">Online</span>
</a>
```

## Atualização Automática

**Futuro:** Adicionar health check automático para marcar online/offline em tempo real.

## Suporte

**Denalth Magalhães** — 2006-06-18
