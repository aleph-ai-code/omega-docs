# SME - Contexto Divisão e Roteamento

> Ω · Denalth
> Secretaria Municipal de Educação - Fortaleza
> Célula de Suporte e Operações
> Data: 2026-07-04

## Sobre a Divisão de Técnicos

### Tentativas Anteriores
- Já fez diversas tentativas de combinar equipe/pessoas
- Já dividiu serviços de várias formas
- **Problema:** Ainda não funcionou bem

### Flávio - Função Especial Criada

**Por que existe:**
- Demora extrema nos atendimentos
- Equipes já fazem o serviço dele
- Criou função para **agilizar chamados simples**
- Ele **NÃO** pega chamados complexos

**Substituto:**
- **NÃO** precisa necessariamente de uma pessoa substituta
- As equipes já cobrem o serviço dele
- É uma função de **agilidade**, não substituição

## Omega - Meu Papel

**Objetivo:**
- Ajudar na **gerência da equipe** de suporte e operações
- Célula de Suporte e Operações da SME
- Gerenciamento de técnicos
- Distribuição inteligente de chamados

**Não existe:**
- ❌ Especialista por categoria
- ❌ Divisão rígida por tipo de problema

**Existe:**
- ✅ Pessoa com mais tempo de campo
- ✅ Pessoa com menos tempo de campo
- ✅ Pessoa mais resolutiva
- ✅ Pessoa que gosta de dar enrolada (precisa ajustar)

---

## Roteamento com .249 - COMPLEXIDADES

### Dados Necessários (Waze/Google Maps)

**Viajante (Waze):**
- ✅ Distância real (quilômetros)
- ✅ Tempo de carro (considerando trânsito)
- ✅ Rotas otimizadas
- ❌ NÃO é distância ponto a ponto (linha reta)

**SRM:**
- ✅ Dados de geolocalização
- ✅ Informações de tráfego

### Problema de Distância

**Exemplo:**
- Escola A: 5km ponto a ponto (mas muito trânsito, 40 min)
- Escola B: 8km ponto a ponto (mas sem trânsito, 15 min)

**Conclusão:**
- Escola A parece mais próxima (linha reta)
- Mas escola B é mais rápida (tempo real de viagem)
- **Carro não viaja como avião (ponto a ponto)**

### Fatores a Considerar

1. **Distância real em quilômetros**
   - Não linha reta
   - Rotas viárias

2. **Tempo de carro**
   - Considerando trânsito
   - Horário de pico vs fora de pico

3. **Tempo parado**
   - Retornos
   - Espera em congestions

4. **Contas de sinal**
   - Semáforos
   - Pausas no trânsito

5. **Distância de trânsito vs linha reta**
   - Escola pode parecer próxima (ponto a ponto)
   - Mas ser longe (tempo de viagem)

### Necessidade

**.249 precisa:**
- Acesso ao Viajante (Waze)
- Acesso ao SRM (geolocalização)
- Calcular tempo real de viagem
- NÃO calcular distância ponto a ponto
- Considerar trânsito

---

## Monitoramento Atual

### Zabbix
- Monitoramento é **manual**
- Abrem chamados no GLPI

### GLPI
- Chamados abertos **manualmente**
- Não é automático

---

## Problemas a Resolver

### 1. Divisão de Técnicos
- Quem com quem?
- Quem cobre o quê?
- Como ajustar "enroladores"?

### 2. Roteamento Inteligente
- Considerar tempo real (não distância)
- Considerar trânsito
- Considerar retornos

### 3. Agilidade
- Reduzir de 2 meses para poucos dias
- Evitar técnicos escolhendo chamados

### 4. Monitoramento
- Automatizar alertas Zabbix → GLPI
- Não depender de abertura manual

---

## Perguntas para Decisão

### Respondidas
- ✅ Flávio precisa de substituto? **NÃO** - equipes já cobrem
- ✅ Omega ajuda na gerência? **SIM** - gestão de equipe
- ✅ Existe especialista por categoria? **NÃO** - é tempo de campo + resolutividade
- ✅ Quem são os "enroladores"? **Kervin Rocha + Alisson Ferreira**
- ✅ Como fazer roteamento? **OSRM (Open Source Routing Machine)**
- ✅ Qual estratégia de divisão? **Híbrida (opção C)**

### Pendentes
- [ ] Confirmar se SRM existe API
- [ ] Instalar OSRM no .249
- [ ] Mapear escolas (coordenadas GPS)
- [ ] Criar API roteamento
- [ ] Ajustar comportamento enroladores

---

## Enroladores CONFIRMADOS

**Identificados:**
- **Kervin Rocha** - Junior, perde tempo em redes sociais
- **Alisson Ferreira** - Junior, perde tempo em redes sociais

**Necessário:**
- Monitorar performance individual
- Métricas de produtividade
- Corrigir comportamento
- Ajustar ou realocar se não melhorar

---

## Estratégia de Divisão ESCOLHIDA

**Opção C - Híbrida** ⭐
- Especialistas onde faz sentido (Flávio VOIP, Davi manutenção)
- Geográfica para volume alto (Internet/Wifi)
- Flexível para backup
- Equilíbrio entre especialização e cobertura

---

## Roteamento - OSRM

**O que é:**
- Open Source Routing Machine
- Motor de roteamento open source
- Usa dados OpenStreetMap
- Calcula tempo real de viagem
- Distância real (quilômetros)
- Considera trânsito

**Vantagens:**
- ✅ GRATUITO
- ✅ Open source
- ✅ Pode rodar on-premise no .249
- ✅ Tempo real (não linha reta)
- ✅ Distância real (não ponto a ponto)

**Viajante = Waze:**
- ❌ Waze NÃO tem API pública oficial
- ❌ Termos de serviço proíbem automação
- ⚠️ OSRM é a alternativa GRATUITA

---

## Observações

**Denalth:**
- "deixa eu ver se você me perguntou mais alguma coisa"
- "deixa eu ver"
- Pode ter mais contexto vindo
- "enroladores são Kervin e Alisson"

**Monitoramento hoje:**
- Zabbix manual
- GLPI manual
- Precisa automatizar

---

## Próximos Passos

1. ⏳ **Aguardar mais contexto** do Denalth
2. ⏳ **Definir estratégia de divisão** (A/B/C)
3. ⏳ **Como acessar Viajante/Waze** via .249
4. ⏳ **Como acessar SRM** para geolocalização
5. ⏳ **Identificar "enroladores"**
6. ⏳ **Automatizar Zabbix → GLPI**
7. ⏳ **Implementar roteamento inteligente**

---

## Status

⏳ Aguardando:
- Mais contexto do Denalth
- Decisão sobre estratégia (A/B/C)
- Como acessar APIs externas (Viajante/SRM)

✅ Entendido:
- Omega ajuda na gerência da equipe
- Flávio não precisa de substituto (equipes já cobrem)
- Roteamento precisa considerar TEMPO REAL (não linha reta)
- Monitoramento hoje é manual (Zabbix + GLPI)
