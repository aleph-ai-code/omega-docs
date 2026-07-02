# Infra Mapa — Rede CEOPS

> Última atualização: 2026-06-17
> Ω · Denalth

## Servidores

| Host | IP LAN | Tailscale | HW | Papel | Disco | OpenClaw | Status |
|---|---|---|---|---|---|---|---|
| **ceopssrv240** | 172.23.39.240 | ✅ | i7-4770, 16GB, 2×K2200, SSD 120GB+RAID | Omega (atual) | 17% | v5.22 ✅ | 🟢 Produção |
| **ceopssrv247** | 172.23.39.247 | ✅ | i7-4770, 16GB, AMD Vega, NVMe 234GB | Omega (antigo) | 10% | v5.20 ⏸️ | 🔴 Offline |
| **ceopssrv241** | 172.23.39.241 | ⏳ | i7-4770, 16GB, GTX 1650, RAID 916GB | Sentinel 🤖 (LLM + Dokploy) | 14% | v5.20 | 🟢 Produção |
| **ceopssrv250** | 172.23.39.250 | — | 30GB RAM, 234GB disco | Aleph (Gabinete 360) | 9% | v5.27 | 🟢 ATIVO (updated) |
| **ceopssrv251** | 172.23.39.251 | — | Ryzen 5 2400GE, 6.7GB, Vega 1GB | Voyager (Travel) | 16% | v5.20 | 🟢 Produção |
| **info-d1454ns** | 172.23.39.39 | ✅ | Windows, i5 | Atlas (Builder) | — | v5.22 ✅ | 🟢 Produção |

## Agentes OpenClaw

| Agente | Host | Canal | Modelo principal | Embeddings | Status |
|---|---|---|---|---|---|
| **Omega** ♾️ | 240 | Telegram @ceops_bot | zai/glm-4.7 | ollama/nomic (241) | ✅ Produção |
| **Sentinel** 🤖 | 241 | TG @GEN_Core_bot | ollama/qwen3:4b | nomic local | ✅ 100% local |
| **Aleph** א | 250 | Telegram @aleph_ai_bot | zai/glm-4.7 | Gemini | ✅ Produção |
| **Atlas** 🏗️ | .39 | Telegram @GEN_Automato_bot | zai/glm-4.7 | — | ✅ Produção |
| **Voyager** ✈️ | 251 | WhatsApp + TG @Travel_Especialist_Bot | zai/glm-4.7 | Gemini | ✅ Produção |

## APIs Configuradas

| API | Uso | Host | Status |
|---|---|---|---|
| ZAI GLM-5.1 | Chat principal | Cloud | ✅ |
| Google Gemini | Visão, search | Cloud | ⚠️ Quota limitada |
| Groq | Transcrição áudio | Cloud | ✅ |
| SearXNG | Web search local | 240:8888 | ✅ (Docker em 240) |
| Duffel | Passagens aéreas | Cloud (sandbox) | ✅ |
| Ollama | LLM + Embeddings local | 241:11434 | ✅ (RAID1 /data) |

## Ollama (241) — Modelos

| Modelo | Tam | VRAM | Velocidade | Uso |
|---|---|---|---|---|
| qwen2.5-coder:3b | 1.9GB | 57% | 49.5 tok/s | ⚡ Código rápido |
| qwen3:4b | 2.5GB | 70% | 32.7 tok/s | 💬 Chat geral **← principal 241** |
| phi4-mini:latest | 2.5GB | 51% | 41.0 tok/s | 💬 Chat geral **← NOVO** |
| gemma4:e4b | 9.6GB | 79% (+offload) | 13.6 tok/s | 🧠 Raciocínio+visão |
| nomic-embed-text | 261MB | — | — | 📊 Embeddings locais |

> Endpoint: http://172.23.39.241:11434/v1 (OpenAI-compatible)

## Embeddings

| Host | Provider | Modelo | Dims | Status |
|---|---|---|---|---|
| 247 (Omega) | ollama (241 remoto) | nomic-embed-text | 768 | ✅ 386 chunks |
| 241 (LLM) | ollama (local) | nomic-embed-text | 768 | ✅ |
| 251 (Voyager) | Gemini | embedding-2 | 768 | ✅ |

## Backups

| O quê | Onde | Frequência |
|---|---|---|
| GitHub openclaw-omega | 247 ~/.openclaw/ | 2x/dia (8h/22h) |
| GitHub openclaw-atlas | .39 ~/.openclaw/ | 2x/dia (8:30/22:30) |
| GitHub openclaw-atlas-desktop | .39 Desktop/OpenClaw/ | 2x/dia |
| Bitwarden | Cloud | Vault |

## Segurança — 240 (Omega atual)

| Item | Status |
|---|---|
| UFW | ✅ Ativo (SSH only) |
| OpenClaw | v5.22 (latest) |
| SearXNG | ✅ Docker (240:8888) |

## Serviços Configurados (.241)

- **Dokploy** → Plataforma de deploy, porta 3000, v0.29.8 ✅
  - 3 containers: dokploy, postgres, redis
  - Acesso: http://172.23.39.241:3000
  - Status: pronto para registro inicial

- **Portainer** → Gestão Docker, porta 9000 ✅ (novo, 2006-06-18)
  - 2 containers: portainer, portainer-agent
  - Acesso: http://172.23.39.241:9000
  - Status: instalado e funcionando

- **Filebrowser** → Gestão de arquivos, porta 8089 ✅ (novo, 2006-06-18)
  - Acesso: http://172.23.39.241:8089
  - Status: instalado e funcionando

- **RedisInsight** → UI Redis, porta 8001 ✅
  - Acesso: http://172.23.39.241:8001
  - Status: funcionando

- **CloudBeaver** → UI Banco de Dados, porta 8978 ✅ (NOVO, 2006-06-18)
  - Container: dbeaver/cloudbeaver:latest
  - Acesso: http://172.23.39.241:8978
  - Volume: /home/ceops/data/cloudbeaver
  - Status: instalado e respondendo (200)

## Pendências

- [x] **Aleph (.250):** Gateway ATIVO (2026-06-15)
- [x] **Updates .250:** 33 pacotes instalados (2026-06-15)
- [x] **Identidade 241:** DEFINIDA como Sentinel ✅
- [x] **llava removido:** VRAM liberada ✅
- [x] **Dokploy instalado:** Pronto para uso ✅
- [x] **Portainer instalado:** Gestão Docker completa ✅
- [x] **Filebrowser instalado:** Gestão de arquivos web ✅
- [x] **CloudBeaver instalado:** UI Banco de Dados completa ✅ (2006-06-18)
- [x] **Limpeza ferramentas antigas:** Scripts removidos ✅ (2006-06-18)
- [ ] Chave SSH nos servidores (241, 251) — acesso via senha só
- [ ] Configurar Ollama como fallback nos outros agentes
- [ ] Atlas cron: remover senha plaintext do payload
