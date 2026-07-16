# SME - Opções de Roteamento e Enroladores

> Ω · Denalth
> Secretaria Municipal de Educação - Fortaleza
> Data: 2026-07-04

## Enroladores Identificados

**CONFIRMADO:**
- **Kervin Rocha** - Junior, enrolador
- **Alisson Ferreira** - Junior, enrolador

**Necessário:**
- Monitorar performance
- Identificar métricas de produtividade
- Corrigir comportamento
- Ajustar ou realocar se não melhorar

---

## Roteamento - Opções Técnicas

### OSRM (Open Source Routing Machine) ⭐ RECOMENDADO

**O que é:**
- Motor de roteamento open source
- Usa dados do OpenStreetMap
- Calcula rotas realistas (não linha reta)
- Considera vias, velocidades, restrições

**APIs Disponíveis:**
1. **Route** - Rota entre 2 pontos com turn-by-turn
2. **Table** - Matriz de distâncias/tempo (pontos A, B, C, D...)
3. **Match** - Match de GPS traces para road network
4. **Nearest** - Ponto mais próximo

**Vantagens:**
- ✅ Open source (GRATUITO)
- ✅ Milissegundos de resposta
- ✅ Pode rodar on-premise (.249)
- ✅ Dados OpenStreetMap (comunidade mantém)
- ✅ Considera trânsito (se configurado)
- ✅ Tempo real de viagem
- ✅ Distância real (quilômetros)

**Desvantagens:**
- ⚠️ Precisa configurar servidor OSRM
- ⚠️ Precisa baixar dados do OpenStreetMap (Fortaleza/CE)
- ⚠️ Requer manutenção

**Como Funciona:**

```
GET /route/v1/driving/{longitude},{latitude};{longitude},{latitude}

Exemplo:
GET /route/v1/driving/-38.5243,-3.7327;-38.5334,-3.7228

Resposta:
{
  "code": "Ok",
  "routes": [{
    "distance": 5430.1,      // 5.4 km
    "duration": 540.5,        // 540 segundos = 9 minutos
    "geometry": "..."
  }]
}
```

**Para .249:**
1. Instalar OSRM no .249
2. Baixar dados OpenStreetMap (Fortaleza/CE)
3. Configurar API local
4. .249 consulta OSRM para cada rota
5. Calcula tempo real de viagem
6. Considera trânsito (se dados disponíveis)

---

### Viajante (Waze) - LIMITADO

**Problema:**
- ❌ Waze NÃO tem API pública oficial
- ❌ Termos de serviço proíbem automação
- ❌ Risco de bloqueio
- ❌ Não é estável para sistema crítico

**Alternativa:**
- Usar **Google Maps API** (pago)
- Usar **MapBox** (pago)
- Usar **OSRM** (GRATUITO) ⭐

---

### Mapas Gratuitos

**Opções:**

1. **OSRM** (GRATUITO) ⭐
   - Open source
   - Comunidade ativa
   - Dados OpenStreetMap
   - Pode rodar on-premise

2. **GraphHopper** (GRATUITO)
   - Similar ao OSRM
   - Open source
   - Dados OpenStreetMap

3. **OpenRouteService** (GRATUITO com limites)
   - API pública
   - 2000 requisições/dia grátis
   - Acima: pago

4. **MapBox** (PAGO)
   - $50/mês (até 50k requisições)
   - Dados OpenStreetMap + dados premium

---

## SRM (Sistema de Roteamento Municipal)

**O que parece ser:**
- Sistema interno da prefeitura
- Geolocalização de escolas
- Dados de mobilidade urbana

**Necessário:**
- Confirmar se existe API SRM
- Confirmar se está disponível
- Confirmar dados que fornece

**Como verificar:**
- Perguntar ao Denalth sobre SRM
- Ver documentação interna
- Verificar se há API disponível

---

## Recomendação Técnica

### Solução Híbrida ⭐

**1. Primário: OSRM (on-premise no .249)**
- Instalar OSRM no .249
- Baixar dados OpenStreetMap (Fortaleza/CE)
- Configurar para cálculo de rotas
- Usar para tempo real de viagem
- Considerar distância real (não linha reta)

**2. Secundário: SRM (se disponível)**
- Usar para geolocalização de escolas
- Complementar dados do OSRM
- Validar rotas

**3. Terciário: Planilha (fallback)**
- Manter planilha manual
- Caso OSRM falhe
- Caso SRM não esteja disponível

---

## Implementação no .249

### Passos

1. **Instalar OSRM no .249**
   ```bash
   # Baixar OSRM
   # Baixar dados OpenStreetMap (Fortaleza/CE)
   # Configurar servidor
   # Testar API
   ```

2. **Mapear escolas com coordenadas**
   - Nome da escola
   - Endereço
   - Latitude/Longitude
   - Distrito de educação

3. **Criar API de roteamento**
   - Input: escola origem, escola destino
   - Output: tempo real, distância real
   - Considerar trânsito

4. **Integrar no .249**
   - Quando técnico precisar ir para escola A → B
   - Consultar OSRM
   - Obter tempo real de viagem
   - Adicionar tempo de serviço
   - Calcular total

5. **Otimizar rotas**
   - Múltiplas escolas em um dia
   - Ordem baseada em tempo real
   - Minimizar tempo total
   - Maximizar atendimentos

---

## Exemplo Prático

**Cenário:**
- Técnico precisa visitar 3 escolas
- Escola A: Rua das Flores, 123
- Escola B: Av. Borges de Melo, 456
- Escola C: Rua São José, 789

**Linha Reta (ponto a ponto):**
- A → B: 5 km
- B → C: 6 km
- C → A: 7 km
- Total: 18 km

**Tempo Real (OSRM):**
- A → B: 5 km, 40 min (trânsito)
- B → C: 8 km, 15 min (sem trânsito)
- C → A: 10 km, 50 min (trânsito)
- Total: 23 km, 105 min

**Conclusão:**
- Distância real: 23 km vs 18 km (28% maior)
- Tempo real: 105 min vs estimado 60 min (75% maior)
- OSRM considerou rotas viárias, trânsito, semáforos

---

## Próximos Passos

1. ⏳ **Confirmar SRM** - Denalth pode confirmar?
2. ⏳ **Instalar OSRM no .249** - posso fazer isso
3. ⏳ **Mapear escolas** - coordenadas GPS
4. ⏳ **Criar API roteamento** - integrar no .249
5. ⏳ **Testar roteamento** - validar com técnicos

---

## Status

✅ **Enroladores identificados:**
- Kervin Rocha
- Alisson Ferreira

✅ **Opção técnica recomendada:**
- OSRM (GRATUITO, open source)

⏳ **Pendente:**
- Confirmar SRM
- Instalar OSRM
- Mapear escolas
- Implementar roteamento
