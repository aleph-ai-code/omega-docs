# Infra Disco — Espaço Não Particionado

> Ω · Denalth
> Última atualização: 2026-07-01

## Resumo Executivo

| Servidor | IP | Não Particionado | VG Livre | **Total Disponível** | Prioridade |
|---|---|---|---|---|---|
| .240 (Omega) | 172.23.39.240 | 0 GB | 0 GB | **0 GB** | — |
| .241 (Sentinel) | 172.23.39.241 | 0 GB | 0 GB | **0 GB** | — |
| .249 | 172.23.39.249 | 0 GB | 0 GB | **0 GB** | — |
| .250 (Aleph) | 172.23.39.250 | **931,5 GB** | **135,42 GB** | **1.066,92 GB** ⭐ | Alta |
| .251 (Voyager) | 172.23.39.251 | 0 GB | 0 GB | **0 GB** | — |

**Total de Espaço Disponível na Infra:** ~1.07 TB

---

## Detalhes por Servidor

### .240 (ceopssrv240) - Omega

**Status:** ✅ Saudável

**Discos:**
- `sda`: 111,8 GB — TUDO particionado (LVM)
- `sdb`: 931,5 GB — linux_raid_member (RAID1)
- `sdc`: 931,5 GB — linux_raid_member (RAID1)

**LVM:**
- VG: ubuntu-vg (108,73 GB)
- LV: ubuntu-lv (108,73 GB)
- VG Livre: 0 GB

**Não Particionado:** 0 GB
**Total Disponível:** 0 GB

**Observação:** RAID1 configurado (sdb + sdc), mas aparentemente não em uso ou montado em outro lugar.

---

### .241 (ceopssrv241) - Sentinel

**Status:** ⚠️ Root crítico (73% usado)

**Discos:**
- `sda`: 931,5 GB — linux_raid_member (RAID1)
- `sdb`: 931,5 GB — linux_raid_member (RAID1)
- `sdc`: 111,8 GB — TUDO particionado (LVM)

**LVM:**
- VG: ubuntu-vg (108,73 GB)
- LV: ubuntu-lv (108,73 GB)
- VG Livre: 0 GB

**Não Particionado:** 0 GB
**Total Disponível:** 0 GB

**Observação:** RAID1 configurado (sda + sdb), mas aparentemente não em uso ou montado em outro lugar.

**Status do Root:** 28 GB livres de 108 GB (73% usado) — necessita limpeza ou expansão urgente.

---

### .249 (ceopssrv249) - Gateway

**Status:** ✅ Saudável

**Discos:**
- `sda`: 238,5 GB — TUDO particionado (235,4 GB LVM)

**LVM:**
- VG: ubuntu-vg (235,42 GB)
- LV: ubuntu-lv (235,42 GB)
- VG Livre: 0 GB

**Não Particionado:** 0 GB
**Total Disponível:** 0 GB

---

### .250 (ceopssrv250) - Aleph ⭐

**Status:** ⚠️ Config OpenClaw pendente

**Discos:**
- `sda`: 931,5 GB — **NÃO PARTICIONADO** ⭐⭐⭐
- `nvme0n1`: 238,5 GB — TUDO particionado (100 GB LVM + 135,4 GB livres)

**LVM:**
- VG: ubuntu-vg (235,42 GB)
- LV: ubuntu-lv (100 GB)
- VG Livre: **135,42 GB**

**Não Particionado:** **931,5 GB**
**Total Disponível:** **1.066,92 GB** ⭐⭐⭐

**Oportunidades:**
1. **Formatar /dev/sda (931,5 GB)** — autorizado em 30/06
2. **Expandir root** — pode expandir de 100 GB → 235 GB (+135 GB)
3. **Criar novo VG/LV** — adicionar /dev/sda ao LVM para ~1.06 TB total

**Status do Disco /dev/sda:**
- Autorizada formatação em 30/06 (dados pessoais foram removidos)
- Aguardando execução da formatação

---

### .251 (ceopssrv251) - Voyager

**Status:** ✅ Saudável

**Discos:**
- `sda`: 238,5 GB — TUDO particionado (235,4 GB LVM)

**LVM:**
- VG: ubuntu-vg (235,42 GB)
- LV: ubuntu-lv (235,42 GB)
- VG Livre: 0 GB

**Não Particionado:** 0 GB
**Total Disponível:** 0 GB

**Observação:** Root expandido recentemente (100 GB → 235 GB) — 75 GB livres.

---

## Análise de Oportunidades

### Prioridade Alta

#### 1. ceopssrv250 — 1.066,92 GB Disponível ⭐⭐⭐

**Opções:**

**A) Expandir Root (Simples - Imediato)**
- Expandir root de 100 GB → 235 GB
- Ganho: +135 GB
- Tempo: 5 minutos
- Risco: Baixo

**B) Formatado /dev/sda + LVM (Complexo - Recomendado)**
1. Format /dev/sda (931,5 GB)
2. Criar PV: `pvcreate /dev/sda`
3. Adicionar ao VG: `vgextend ubuntu-vg /dev/sda`
4. Expandir root para 500 GB ou mais
- Ganho: +~400 GB (ou mais, dependendo do tamanho)
- Tempo: 30-60 minutos
- Risco: Médio

**C) Criar Novo Volume (Alternativo)**
1. Format /dev/sda (931,5 GB)
2. Criar novo VG/LV para dados
- Ganho: +~900 GB para dados
- Tempo: 30-60 minutos
- Risco: Médio

**Recomendação:** Começar com **A) Expandir Root** (imediato) e depois avaliar **B) ou C)**.

---

### Prioridade Crítica

#### 2. ceopssrv241 — Root em 73% (28 GB livres)

**Status:** Crítico — necessita atenção imediata

**Opções:**

**A) Limpeza (Imediato)**
- Remover logs antigos
- Limpar cache de pacotes
- Remover modelos Ollama não usados
- Ganho: +10-20 GB
- Tempo: 30 minutos
- Risco: Baixo

**B) Investigar RAID1 (Médio Prazo)**
- Investigar se RAID1 (sda + sdb) está funcional
- Se sim, pode adicionar ao LVM
- Ganho: Potencialmente +~900 GB
- Tempo: 1-2 horas
- Risco: Alto (envolve RAID)

**Recomendação:** Começar com **A) Limpeza** imediata e depois investigar **B) RAID1**.

---

## Comandos Úteis

### Ver Espaço Não Particionado

```bash
# Listar discos não particionados:
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT,FSTYPE

# Ver espaço livre no VG:
sudo vgs

# Ver detalhes do LV:
sudo lvs

# Ver detalhes do PV:
sudo pvs
```

### Expandir Root (Imediato)

```bash
# Expandir LV para o tamanho máximo do VG:
sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv

# Resize do filesystem:
sudo resize2fs /dev/ubuntu-vg/ubuntu-lv

# Verificar novo tamanho:
df -h /
```

### Formatar Novo Disco (Exemplo .250)

```bash
# 1. Verificar disco:
lsblk /dev/sda

# 2. Criar PV:
sudo pvcreate /dev/sda

# 3. Adicionar ao VG:
sudo vgextend ubuntu-vg /dev/sda

# 4. Expandir LV:
sudo lvextend -L +500G /dev/ubuntu-vg/ubuntu-lv

# 5. Resize filesystem:
sudo resize2fs /dev/ubuntu-vg/ubuntu-lv
```

---

## Resumo de Ações Recomendadas

### Imediato (Hoje)

1. **.250** — Expandir root de 100 GB → 235 GB (+135 GB)
   - Tempo: 5 minutos
   - Risco: Baixo

2. **.241** — Limpeza de disco (logs, cache, modelos)
   - Tempo: 30 minutos
   - Risco: Baixo

### Curto Prazo (Esta semana)

1. **.250** — Formatar /dev/sda (931,5 GB) e adicionar ao LVM
   - Tempo: 1 hora
   - Risco: Médio
   - Ganho: +~400 GB ou mais

2. **.241** — Investigar RAID1 (sda + sdb)
   - Tempo: 2 horas
   - Risco: Alto
   - Ganho: Potencialmente +~900 GB

### Longo Prazo

1. **Avaliar migração** — Servidores com pouco espaço (.240, .241, .249, .251)
2. **Planejar backup** — Antes de operações de disco
3. **Documentar procedimentos** — Para futuras expansões

---

## Referências

- Data do levantamento: 2026-07-01
- Responsável: Omega (Ω)
- Documentação relacionada:
  - `memory/2026-06-30-disk-expansion-opportunities.md`
  - `memory/2026-06-30-to-2026-07-01-consolidated.md`
  - `docs/infra-mapa.md`

---

_Ω · Denalth - 2026-07-01_
