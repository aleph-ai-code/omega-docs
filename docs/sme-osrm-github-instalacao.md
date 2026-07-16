# SME - OSRM GitHub + Guia de Instalação

> Ω · Denalth
- Data: 2026-07-04

## OSRM - GitHub

**Repositório Principal:**
- **URL:** https://github.com/Project-OSRM/osrm-backend
- **Linguagem:** C++
- **Descrição:** High performance routing engine for OpenStreetMap data

**Website:**
- **URL:** https://project-osrm.org/
- **Demo:** Demo server disponível

**Serviços Disponíveis:**
- HTTP API
- C++ library interface
- Node.js wrapper

**Modos Suportados:**
- Car 🚗
- Bicycle 🚴
- Walk 🚶
- Customizável via profiles

---

## Métodos de Instalação

### 1. Docker (Mais Fácil) ⭐ RECOMENDADO

**Vantagens:**
- ✅ Mais simples
- ✅ Isolado do sistema
- ✅ Fácil de gerenciar
- ✅ Reproduzível

**Comando:**
```bash
docker pull osrm/osrm-backend
```

### 2. Pre-built Binaries

**Vantagens:**
- ✅ Não precisa compilar
- ✅ Instalação rápida
- ⚠️ Dependências ainda necessárias

### 3. Building from Source

**Vantagens:**
- ✅ Controle total
- ✅ Otimização específica
- ⚠️ Mais complexo
- ⚠️ Requer dependências

**Passos:**
```bash
# Baixar
wget https://github.com/Project-OSRM/osrm-backend/archive/vX.Y.Z.tar.gz
tar -xzf vX.Y.Z.tar.gz
cd osrm-backend-X.Y.Z

# Compilar
mkdir -p build
cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
cmake --build .

# Instalar (opcional)
sudo cmake --build . --target install
```

### 4. Node.js Bindings

**Vantagens:**
- ✅ Integrável com Node.js
- ✅ Bom para aplicações JavaScript
- ⚠️ Requer Node.js instalado

---

## Instalação no .249

### Opção Recomendada: Docker

**Por que Docker?**
- Simples de instalar
- Fácil de atualizar
- Isolado do sistema
- Pode rodar no .249 sem conflitos

**Passos:**

1. **Verificar Docker no .249:**
   ```bash
   ssh ceops@172.23.39.249
   docker --version
   ```

2. **Baixar Imagem OSRM:**
   ```bash
   docker pull osrm/osrm-backend:latest
   ```

3. **Baixar Dados OpenStreetMap:**
   ```bash
   # Fortaleza/CE
   wget http://download.geofabrik.de/south-america/brazil/fortaleza-latest.osm.pbf
   ```

4. **Processar Dados:**
   ```bash
   docker run -t -v "${PWD}:/data" osrm/osrm-backend osrm-extract -p /data/profiles/car.lua /data/fortaleza-latest.osm.pbf
   ```

5. **Iniciar Servidor:**
   ```bash
   docker run -t -i -p 5000:5000 -v "${PWD}:/data" osrm/osrm-backend osrm-routed --algorithm mld /data/fortaleza-latest.osrm
   ```

6. **Testar API:**
   ```bash
   curl "http://localhost:5000/route/v1/driving/-38.5243,-3.7327;-38.5334,-3.7228?overview=false"
   ```

### Opção 2: Building from Source (sem Docker)

**Requisitos:**
- GCC/Clang
- CMake 3.15+
- Boost 1.74+
- Lua 5.3+
- Expat 2.2+
- TBB 2020+

**Passos:**

1. **Instalar Dependências (.249):**
   ```bash
   sudo apt update
   sudo apt install -y build-essential cmake \
       libboost-all-dev liblua5.3-dev \
       libexpat1-dev libtbb-dev \
       zlib1g-dev
   ```

2. **Clonar Repositório:**
   ```bash
   cd /opt
   git clone https://github.com/Project-OSRM/osrm-backend.git
   cd osrm-backend
   ```

3. **Compilar:**
   ```bash
   mkdir -p build
   cd build
   cmake .. -DCMAKE_BUILD_TYPE=Release
   cmake --build . -j$(nproc)
   ```

4. **Baixar Dados Fortaleza:**
   ```bash
   wget http://download.geofabrik.de/south-america/brazil/fortaleza-latest.osm.pbf
   ```

5. **Processar Dados:**
   ```bash
   ./build/osrm-extract -p profiles/car.lua fortaleza-latest.osm.pbf
   ```

6. **Iniciar Servidor:**
   ```bash
   ./build/osrm-routed --algorithm mld fortaleza-latest.osrm
   ```

---

## API OSRM

### Endpoint: Route

**Calcula rota entre dois pontos:**

```
GET /route/v1/driving/{longitude},{latitude};{longitude},{latitude}

Exemplo:
GET /route/v1/driving/-38.5243,-3.7327;-38.5334,-3.7228
```

**Resposta:**
```json
{
  "code": "Ok",
  "routes": [{
    "distance": 5430.1,      // metros
    "duration": 540.5,       // segundos
    "geometry": "..."
  }]
}
```

### Endpoint: Table

**Matriz de distâncias/tempo (múltiplos pontos):**

```
GET /table/v1/driving/{lng1},{lat1};{lng2},{lat2};{lng3},{lat3}
```

**Resposta:**
```json
{
  "durations": [[0, 540, 720], [540, 0, 630], [720, 630, 0]],
  "distances": [[0, 5430, 7120], [5430, 0, 6540], [7120, 6540, 0]]
}
```

---

## Integração com .249

### Arquitetura

```
.249 (OpenClaw)
  │
  ├── API Roteamento (/route/calculate)
  │     │
  │     └── OSRM (localhost:5000 ou Docker)
  │
  └── GLPI API
        │
        ├── Dados Escolas (GPS)
        ├── Dados Técnicos
        └── Métricas
```

### Implementação

1. **OSRM fornece:**
   - Tempo real de viagem
   - Distância real (quilômetros)
   - Considera trânsito (se configurado)

2. **.249 implementa:**
   - Endpoint `/route/calculate`
   - Input: escola origem + escola destino
   - Output: tempo + distância + rota otimizada

3. **GLPI fornece:**
   - Coordenadas GPS das escolas
   - Dados dos técnicos
   - Métricas de atendimento

---

## Dados OpenStreetMap

### Fontes

**Geofabrik (Recomendado):**
- URL: http://download.geofabrik.de/
- Fortaleza: http://download.geofabrik.de/south-america/brazil/fortaleza-latest.osm.pbf
- Atualização: Diária

### Baixar Fortaleza/CE

```bash
# Fortaleza apenas
wget http://download.geofabrik.de/south-america/brazil/fortaleza-latest.osm.pbf

# Ceará completo (opcional)
wget http://download.geofabrik.de/south-america/brazil/ce-latest.osm.pbf

# Brasil (opcional - muito grande)
wget http://download.geofabrik.de/south-america/brazil-latest.osm.pbf
```

### Tamanho Arquivos

- Fortaleza: ~30 MB
- Ceará: ~150 MB
- Brasil: ~5 GB

---

## Próximos Passos

1. ⏳ **Aguardar credenciais GLPI**
2. ⏳ **Verificar Docker no .249**
3. ⏳ **Instalar OSRM (Docker ou source)**
4. ⏳ **Baixar dados Fortaleza**
5. ⏳ **Testar API OSRM**
6. ⏳ **Criar endpoint roteamento no .249**
7. ⏳ **Integrar com GLPI**

---

## Status

✅ **OSRM encontrado no GitHub**
- Repositório: Project-OSRM/osrm-backend
- Site: project-osrm.org

⏳ **Aguardando:**
- Credenciais GLPI
- Verificar Docker no .249

🚀 **Pronto para:**
- Instalar OSRM
- Baixar dados Fortaleza
- Criar API roteamento
