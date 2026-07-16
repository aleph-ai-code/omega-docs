# SME - Estratégia .249 - Tudo no Servidor

> Ω · Denalth
- Data: 2026-07-04
- Diretriz: "Tudo no 249"

## Diretriz do Denalth

**"Tudo no .249"**

Significa:
- ✅ Todos os dados ficam no .249
- ✅ Todas as configurações ficam no .249
- ✅ Todo o processamento roda no .249
- ✅ .249 é o agente inteligente de distribuição

---

## Arquitetura Proposta

### .249 (Servidor Central)

**Função:** Agente Inteligente de Distribuição

**Componentes:**

1. **OpenClaw Gateway** ✅ (já rodando)
   - Bind: 0.0.0.0:18789 ✅
   - Perms: root:root 600 ✅
   - Status: funcionando

2. **OSRM (Roteamento)** 🔄 (em instalação)
   - Docker: 29.6.1 ✅
   - Imagem: osrm/osrm-backend ✅
   - Dados: Fortaleza/CE (pendente)
   - API: localhost:5000

3. **GLPI Client** ⏳ (pendente credenciais)
   - API REST
   - Extrair: escolas, técnicos, métricas
   - Salvar local: banco de dados

4. **Banco de Dados Local** 📊 (a criar)
   - Escolas: 600+ (nome, GPS, distrito)
   - Técnicos: 15 (email, telefone, férias, performance)
   - Métricas: calculadas a partir de GLPI
   - Chamados: histórico

5. **API de Distribuição** 🤖 (a implementar)
   - Endpoint: `/distribute/assign`
   - Input: chamado novo
   - Output: técnico atribuído
   - Lógica: disponibilidade + capacidade + distância

---

## Fluxo de Trabalho

### 1. Ingestão de Dados (GLPI → .249)

```
GLPI API → .249 → Banco Local
```

- Extrair escolas (nome, GPS, distrito)
- Extrair técnicos (email, telefone, férias)
- Extrair chamados (últimos 30 dias)
- Calcular métricas

### 2. Roteamento (OSRM)

```
Escola A → Escola B
↓
OSRM API
↓
Tempo real + Distância real
```

- Considerar trânsito
- Não linha reta
- Tempo de viagem real

### 3. Distribuição Inteligente

```
Chamado Novo
↓
API /distribute/assign
↓
Verificar: disponibilidade + capacidade + distância
↓
Atribuir técnico
↓
Notificar (GLPI + Telegram)
```

**Regras:**
- Técnico disponível? (não de férias)
- Técnico capaz? (senior/junior)
- Distância razoável? (tempo real OSRM)
- Categoria compatível? (se especialista)
- **NÃO deixa técnico escolher** (igualitário)

### 4. Monitoramento

```
Performance Técnico
↓
Métricas: atendimentos, tempo, resolução
↓
Dashboard
```

- Kervin + Alisson (enroladores)
- Frank (coordenadorias)
- Geral (justificar recursos)

---

## Estrutura de Diretórios .249

```
/root/.openclaw/
├── workspace/
│   └── sme/                    # Dados SME
│       ├── data/
│       │   ├── schools.json    # 600+ escolas
│       │   ├── technicians.json # 15 técnicos
│       │   ├── metrics.json    # Métricas calculadas
│       │   └── tickets.json    # Chamados históricos
│       ├── osrm/
│       │   ├── data/           # Dados OpenStreetMap
│       │   └── extract/        # Dados processados
│       └── glpi/
│           ├── cache/          # Cache GLPI
│           └── config.json     # Credenciais GLPI
├── agents/
│   └── sme-router/            # Agente de distribuição
└── gateway/
    └── plugins/
        └── sme-distributor/    # Plugin OpenClaw
```

---

## Próximos Passos

### Imediatos

1. ⏳ **Resolver download dados OSRM**
   - Geofabrik não está funcionando
   - Tentar mirror alternativo
   - Ou usuário baixa e SCP

2. ⏳ **Obter credenciais GLPI**
   - Buscar em conversa de ontem
   - Ou usuário fornece agora

3. ⏳ **Instalar OSRM completo**
   - Dados Fortaleza/CE
   - Processar com osrm-extract
   - Iniciar osrm-routed

### Curto Prazo

4. ⏳ **Criar banco de dados local**
   - Esquema: schools, technicians, tickets, metrics
   - SQLite ou PostgreSQL

5. ⏳ **Extrair dados GLPI**
   - Escolas (600+)
   - Técnicos (15)
   - Chamados (30 dias)

6. ⏳ **Criar API distribuição**
   - Endpoint /distribute/assign
   - Lógica de atribuição
   - Integração OSRM

### Médio Prazo

7. ⏳ **Dashboard métricas**
   - Frank (coordenadorias)
   - Geral (tempo espera, resolução)
   - Enroladores (Kervin + Alisson)

8. ⏳ **Automação Zabbix → GLPI**
   - Alertas automáticos
   - Abertura de chamados

---

## Status Atual .249

✅ **Funcionando:**
- OpenClaw Gateway (bind 0.0.0.0:18789)
- Docker (29.6.1)
- OSRM imagem baixada

⏳ **Pendente:**
- Dados OpenStreetMap (Geofabrik falhou)
- Credenciais GLPI
- OSRM processamento
- API distribuição
- Banco de dados local

---

## Memória

**"Tudo no .249"** = Servidor central do SME

- Dados: escolas, técnicos, métricas
- Roteamento: OSRM (tempo real)
- Distribuição: API inteligente
- Monitoramento: dashboard

.249 é o **cérebro** da operação de chamados SME.

---

## Conclusão

**Estratégia clara:**

1. .249 = Servidor central
2. Todos os dados ficam no .249
3. Todo o processamento roda no .249
4. .249 distribui chamados inteligentemente
5. GLPI fornece dados brutos
6. OSRM fornece roteamento
7. .249 decide qual técnico atende

**Próximo passo:**
- Resolver download dados OSRM
- Obter credenciais GLPI
