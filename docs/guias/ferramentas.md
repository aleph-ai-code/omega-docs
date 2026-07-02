# Ferramentas Disponíveis — Rede CEOPS
> Atualizado: 2026-05-24 | Ω · Denalth

## Todas as máquinas Linux

### 🔍 Busca e processamento
- **jq** — Processa JSON no terminal
  - `curl api/x | jq '.data[].name'`
  - `jq '.field' file.json`
- **ripgrep (rg)** — Busca texto em arquivos (mais rápido que grep)
  - `rg "palavra" memory/`
  - `rg -l "erro" docs/` (só lista arquivos)
- **fd-find (fdfind)** — Busca arquivos (melhor que find)
  - `fdfind "\.md$" docs/`
  - `fdfind "config" /etc`
- **grep/egrep** — Busca padrões (nativo)

### 📊 Monitoramento
- **htop** — Monitor interativo CPU/RAM/processos
- **ncdu** — Analisa uso de disco interativo (`ncdu /`)
- **smartctl** — Saúde do disco (`smartctl -a /dev/sda`)
- **bwm-ng** — Monitor de tráfego de rede em tempo real

### 🛠️ Sistema
- **tmux** — Multiplexador de terminal (sessões persistentes)
- **curl/wget** — Downloads e APIs
- **git** — Controle de versão

### 🔧 OpenClaw
- **openclaw gateway status** — Status do gateway
- **openclaw gateway restart** — Reiniciar gateway
- **openclaw gateway install** — Instalar service systemd
- **openclaw memory search "query"** — Buscar memória
- **openclaw cron list** — Listar crons
- **openclaw status** — Status geral
- **openclaw channels login --channel telegram** — Relink canal

## Instalação (todas de uma vez)
```bash
sudo apt install -y ripgrep fd-find ncdu smartmontools bwm-ng jq htop tmux curl wget git
```

## Status por servidor (2026-05-24)

| Ferramenta | 247 | 240 | 241 | 250 | 251 | 39 |
|---|---|---|---|---|---|---|
| jq | ✅ | ✅ | ✅ | ✅ | ✅ | ❓ |
| rg | ✅ | ✅ | ✅ | ✅ | ✅ | ❓ |
| fdfind | ✅ | ✅ | ✅ | ✅ | ✅ | ❓ |
| htop | ✅ | ✅ | ✅ | ✅ | ✅ | N/A |
| ncdu | ✅ | ✅ | ✅ | ✅ | ✅ | N/A |
| smartctl | ❌ | ❌ | ❌ | ✅ | ❌ | N/A |
| bwm-ng | ❌ | ❌ | ❌ | ❌ | ❌ | N/A |
| tmux | ✅ | ✅ | ✅ | ✅ | ✅ | N/A |
| curl/wget/git | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

> **Nota:** 247 e 240 não têm smartctl — instalar quando possível. Windows (39) usa comandos diferentes.
