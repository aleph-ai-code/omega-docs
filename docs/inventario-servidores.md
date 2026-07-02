# Inventário de Servidores — Rede CEOPS
> Atualizado: 2026-05-24 | Ω · Denalth

## Mapa Geral

| # | Hostname | IP | CPU | RAM | GPU | Disco | SO | OpenClaw | Papel |
|---|---|---|---|---|---|---|---|---|---|
| 240 | ceopssrv240 | 172.23.39.240 | i7-4770 (4c/8t) | 16GB | 2× Quadro K2200 (4GB) | SSD 120GB + HDD 1TB RAID1 | Ubuntu 26.04 | 2026.5.22 ✅ | Omega (futuro) |
| 241 | ceopssrv241 | 172.23.39.241 | i7-4770 (4c/8t) | 16GB | GTX 1650 (4GB) | SSD 120GB + 2×HDD 1TB RAID1 | Ubuntu 26.04 | 2026.5.20 | LLM Server (Ollama) |
| 247 | ceopssrv247 | 172.23.39.247 | **Ryzen 5 PRO 2400GE** (4c/8t) | 16GB | AMD Vega (integrada) | NVMe 256GB + HDD 1TB | Linux Mint 22.3 | 2026.5.20 | Omega (atual) |
| 250 | ceopssrv250 | 172.23.39.250 | **Ryzen 5 PRO 2400GE** (4c/8t) | **32GB** | AMD Vega (integrada) | SSD 256GB (LVM 100GB) | Ubuntu 24.04 | 2026.5.12 ❌ | Sem papel definido |
| 251 | ceopssrv251 | 172.23.39.251 | **Ryzen 5 PRO 2400GE** (4c/8t) | **8GB** | AMD Vega (integrada) | SSD 256GB (LVM 100GB) | Ubuntu 24.04 | 2026.5.22 ✅ | Voyager (Travel) |
| 39 | info-d1454ns | 172.23.39.39 | i7-4770 (4c/8t) | 16GB | Intel HD 4600 + GT 210 (1GB) | SSD 240GB | Windows 11 Pro | 2026.5.22 ✅ | Atlas (Builder) |

## Detalhamento

---

### ceopssrv240 — Omega (futuro)
- **CPU:** Intel i7-4770 @ 3.40GHz (4 cores, 8 threads)
- **RAM:** 16GB DDR3
- **GPU:** 2× NVIDIA Quadro K2200 (4GB cada) — sem driver verificado
- **Disco:** SSD 120GB (23% usado) + HDD 1TB Toshiba RAID1
- **Firmware:** 51005Q87AU
- **SO:** Ubuntu 26.04 LTS, kernel 7.0.0-15
- **Tailscale:** ✅ 100.86.46.73
- **UFW:** ✅ Ativo (SSH only)
- **Node:** v22.22.2
- **Notas:** Gateway systemd instalado, Telegram desabilitado (aguardando switch)

---

### ceopssrv241 — LLM Server
- **CPU:** Intel i7-4770 @ 3.40GHz (4 cores, 8 threads)
- **RAM:** 16GB DDR3
- **GPU:** NVIDIA GTX 1650 (4GB) — driver 595.71.05, 31°C, 6.23W
- **Disco:** SSD 120GB (14% usado) + 2× HDD 1TB Toshiba RAID1 (data: 27GB/916GB)
- **Firmware:** 41211Q87AU
- **SO:** Ubuntu 26.04 LTS, kernel 7.0.0-15
- **Tailscale:** ❌ Deslogado
- **UFW:** ✅ Ativo (SSH only)
- **Ollama (5 modelos):**
  - nomic-embed-text → 261MB (embeddings)
  - qwen3:4b → 2.4GB (chat)
  - qwen2.5-coder:3b → 1.8GB (código rápido)
  - qwen2.5-coder:7b → 4.5GB (código pesado)
  - gemma4:e4b → 9.2GB (raciocínio+visão)
- **Notas:** OpenClaw desatualizado (5.20). Bot @GEN_Core_bot

---

### ceopssrv247 — Omega (atual)
- **CPU:** AMD Ryzen 5 PRO 2400GE @ 3.20GHz (4 cores, 8 threads)
- **RAM:** 16GB DDR4
- **GPU:** AMD Radeon Vega (integrada, 1GB VRAM)
- **Disco:** WD NVMe 256GB (10% usado) + HDD 1TB WDC
- **Firmware:** Q26 v02.25.00
- **SO:** Linux Mint 22.3, kernel 6.17.0-29
- **Tailscale:** ✅ 100.107.139.44
- **UFW:** ✅ Ativo (SSH only)
- **Node:** v22.22.2
- **Notas:** Será desativado após migração para 240

---

### ceopssrv250 — ⚠️ SEM PAPEL DEFINIDO
- **CPU:** AMD Ryzen 5 PRO 2400GE @ 3.20GHz (4 cores, 8 threads)
- **RAM:** **32GB DDR4** ← mais RAM da rede
- **GPU:** AMD Radeon Vega (integrada)
- **Disco:** SSD LITEON 256GB (10% usado, LVM 100GB — desperdiça 135GB!)
- **Firmware:** Q26 v02.23.00
- **SO:** Ubuntu 24.04.4 LTS, kernel 6.8.0-117
- **Tailscale:** ❌ Não instalado
- **UFW:** ❌ **INATIVO**
- **Node:** v24.15.0
- **Problemas:**
  - OpenClaw desatualizado (2026.5.12)
  - UFW desativado (segurança!)
  - LVM usa só 100GB de 235GB disponíveis
  - Sem smartmontools (não dá pra checar saúde do disco)
  - Firmware antiga (02.23.00)
  - Tailscale não instalado
  - Nenhum papel definido

---

### ceopssrv251 — Voyager (Travel Specialist)
- **CPU:** AMD Ryzen 5 PRO 2400GE @ 3.20GHz (4 cores, 8 threads)
- **RAM:** **8GB DDR4** ← menor RAM da rede
- **GPU:** AMD Radeon Vega (integrada)
- **Disco:** SSD LITEON 256GB (16% usado, LVM 100GB)
- **Firmware:** Q26 v02.24.01
- **SO:** Ubuntu 24.04.4 LTS, kernel 6.8.0-117
- **Tailscale:** ❌ Deslogado
- **UFW:** ✅ Ativo (SSH only)
- **Node:** v24.15.0
- **Notas:** Roda como root. Bot @aleph_ai_bot

---

### info-d1454ns — Atlas (Windows)
- **CPU:** Intel i7-4770 @ 3.40GHz (4 cores, 8 threads)
- **RAM:** 16GB
- **GPU:** Intel HD 4600 + NVIDIA GT 210 (1GB) — GPU fraca
- **Disco:** Kingston SSD 240GB (64% livre)
- **SO:** Windows 11 Pro (Build 26100)
- **Tailscale:** ✅ 100.125.151.96
- **Node:** v24.15.0
- **Notas:** Bot @GEN_Automato_bot. GPU GT 210 é de 2009 — sem utilidade pra ML

---

## Ranking por Capacidade

### CPU (melhor → pior)
1. **Ryzen 5 PRO 2400GE** (247, 250, 251) — arquitetura Zen+ (2018), mais eficiente
2. **i7-4770** (240, 241, 39) — Haswell (2013), ainda competente

### RAM (maior → menor)
1. **250: 32GB** ← destaque
2. 240/241/247/39: 16GB
3. **251: 8GB** ← limitante

### GPU (melhor → pior)
1. **241: GTX 1650 (4GB)** — única GPU útil pra ML/Ollama
2. 240: 2× Quadro K2200 (4GB cada) — OpenGL workstation, sem CUDA
3. 247/250/251: AMD Vega integrada — sem ML, só display
4. 39: GT 210 (1GB) — de 2009, inútil

### Disco (maior → menor)
1. **241: SSD 120GB + 2×1TB RAID1** = 1TB storage
2. **240: SSD 120GB + 1TB RAID1** = 1TB storage
3. 247: NVMe 256GB + 1TB HDD
4. 250/251: SSD 256GB
5. 39: SSD 240GB

---

## Alertas e Recomendações

### 🔴 Crítico
- **250: UFW desativado** — servidor exposto na rede sem firewall
- **250: Sem Tailscale** — sem acesso remoto seguro
- **250: LVM desperdiça 135GB** — 100GB alocado de 235GB disponíveis

### 🟡 Importante
- **241: OpenClaw 5.20** — precisa atualizar pra 5.22
- **250: OpenClaw 5.12** — muito desatualizado
- **250: Firmware 02.23.00** — 247 está em 02.25.00, 251 em 02.24.01 (mesmo hardware)
- **241: Tailscale deslogado** — sem acesso remoto
- **251: Tailscale deslogado** — sem acesso remoto
- **251: 8GB RAM** — limitante pra uso mais pesado

### 🟢 Sugestões
- **250:** Tem o melhor hardware geral (Ryzen + 32GB). Ideal pra:
  - Mover Voyager (251) pra cá (Travel Specialist com mais resources)
  - Ou usar como LLM server secundário (CPU-only inference com Ollama)
  - Ou Data/Pipeline server
- **240:** As 2× Quadro K2200 são GPUs de workstation (sem CUDA/ml). Considerar remover se não usa display
- **251:** Com só 8GB RAM, é limitado. Se mover o Voyager pro 250, o 251 pode ser desativado ou usado pra algo leve
- **247 (pós-migração):** Ryzen + NVMe + 16GB. Pode virar servidor de backup, monitoring, ou laboratório

---

## Atualizações Pendentes

| Servidor | OpenClaw | Node | SO Updates | Firmware |
|---|---|---|---|---|
| 240 | ✅ 5.22 | ✅ v22 | 1 snap | ✅ 51005Q87AU |
| 241 | ❌ 5.20 | ✅ v22 | 0 | ✅ 41211Q87AU |
| 247 | ❌ 5.20 | ✅ v22 | 0 | ✅ Q26 v02.25 |
| 250 | ❌ **5.12** | ⚠️ v24 | 1 snap | ❌ Q26 v02.23 |
| 251 | ✅ 5.22 | ⚠️ v24 | 1 snap | ✅ Q26 v02.24 |
| 39 | ✅ 5.22 | ⚠️ v24 | N/A | N/A |

> **Nota:** Node v24 vs v22 — ambas funcionam com OpenClaw. v24 é mais novo mas não é necessário.

---

## Resumo Executivo

- **7 servidores** na rede (6 Linux + 1 Windows)
- **2 arquiteturas:** Haswell i7-4770 (3 serv) e Ryzen 2400GE (3 serv)
- **Servidor mais forte:** ceopssrv250 (Ryzen + 32GB RAM) — totalmente subutilizado
- **Servidor mais fraco:** ceopssrv251 (8GB RAM) ou info-d1454ns (GT 210)
- **GPU útil pra ML:** só a GTX 1650 da 241
- **3 servidores precisam de OpenClaw update:** 241, 247, 250
- **1 servidor sem firewall:** 250 (crítico)
- **3 servidores sem Tailscale:** 241, 250, 251
