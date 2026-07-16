# SME - .249 Status + Hardware + Preparação OSRM

> Ω · Denalth
- Data: 2026-07-04
- Servidor: ceopssrv249 (172.23.39.249)

## Status .249

### Hardware

**CPU:**
- Modelo: AMD Ryzen 5 PRO 2400GE w/ Radeon Vega Graphics
- CPUs: 8 (4 cores + 8 threads)
- Threads per core: 2
- Frequência: Escalando (89% atual)

**Memória:**
- Total: 6.7 GB
- Used: 1.1 GB
- Available: 5.6 GB
- Buff/cache: 1.5 GB

**Disco:**
- Total: 232 GB
- Used: 41 GB (19%)
- Available: 181 GB
- **Excelente para OSRM!**

### Software

**Instalado:**
- ✅ Git: 2.43.0
- ✅ CMake: 3.28.3
- ✅ OpenClaw Gateway: Rodando
- ✅ SSH: Configurado

**NÃO Instalado:**
- ❌ Docker

### Conclusão

**.249 é IDEAL para OSRM!**
- ✅ Hardware suficiente
- ✅ Memória adequada
- ✅ Muito espaço em disco (181 GB livres)
- ✅ CMake instalado
- ⚠️ Docker não instalado

---

## Opções de Instalação OSRM

### Opção 1: Instalar Docker (Recomendado) ⭐

**Por que:**
- ✅ Mais simples
- ✅ Fácil de atualizar
- ✅ Isolado do sistema
- ✅ Pode instalar facilmente

**Como instalar:**
```bash
# Atualizar
sudo apt update

# Instalar dependências
sudo apt install -y ca-certificates curl gnupg lsb-release

# Adicionar repositório Docker
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Instalar Docker
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io

# Verificar
docker --version
```

**Depois:**
```bash
# Baixar OSRM
docker pull osrm/osrm-backend

# Baixar dados Fortaleza
wget http://download.geofabrik.de/south-america/brazil/fortaleza-latest.osm.pbf

# Processar
docker run -t -v "${PWD}:/data" osrm/osrm-backend osrm-extract -p /data/profiles/car.lua /data/fortaleza-latest.osm.pbf

# Iniciar
docker run -t -i -p 5000:5000 -v "${PWD}:/data" osrm/osrm-backend osrm-routed --algorithm mld /data/fortaleza-latest.osrm
```

### Opção 2: Building from Source (Sem Docker)

**Vantagens:**
- ✅ Não precisa Docker
- ✅ CMake já instalado
- ✅ Compilação otimizada

**Dependências necessárias:**
```bash
sudo apt install -y build-essential \
    libboost-all-dev \
    liblua5.3-dev \
    libexpat1-dev \
    libtbb-dev \
    zlib1g-dev
```

**Instalação:**
```bash
cd /opt
sudo git clone https://github.com/Project-OSRM/osrm-backend.git
cd osrm-backend

mkdir -p build
cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
cmake --build . -j$(nproc)
```

---

## Recomendação

**Opção 1 (Docker)** é RECOMENDADA porque:
- Mais simples
- Menos propenso a erros
- Fácil de gerenciar
- Pode instalar sem complicações

**Opção 2 (Source)** é alternativa se:
- Prefere compilar
- Quer otimização máxima
- Não quer Docker

---

## Próximos Passos

1. ⏳ **Aguardar:**
   - Credenciais GLPI
   - Permissão para instalar Docker

2. ⏳ **Instalar:**
   - Docker (ou dependências para source)
   - OSRM
   - Dados Fortaleza

3. ⏳ **Configurar:**
   - API OSRM
   - Endpoint roteamento
   - Integração GLPI

---

## Status

✅ **.249 verificado**
- Hardware: Adequado
- Software: Git + CMake OK
- Disco: 181 GB livres

⏳ **Aguardando:**
- Credenciais GLPI
- Permissão para instalar Docker

🚀 **Pronto para:**
- Instalar OSRM
- Configurar roteamento
- Integrar com GLPI

---

## Espaço Necessário

**OSRM + Dados:**
- Binários OSRM: ~500 MB
- Dados Fortaleza: ~30 MB
- Dados processados: ~100 MB
- **Total: ~630 MB**

**.249 tem 181 GB livres** ✅

**Espaço MAIS que suficiente!**
