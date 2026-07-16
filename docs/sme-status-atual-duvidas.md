# SME - Status Atual + Dúvidas + Próximos Passos para .249

> Ω · Denalth
> Secretaria Municipal de Educação - Fortaleza
> Data: 2026-07-04

## ✅ TUDO SALVO ESTRUTURADAMENTE

### Documentos Criados

1. **`docs/sme-tecnicos-organizacao.md`**
   - Lista completa de técnicos
   - Diretrizes do Denalth
   - 5 perguntas decisórias

2. **`docs/sme-opcoes-divisao.md`**
   - 3 opções (A: Especialistas, B: Geográfica, C: Híbrida)
   - Comparação detalhada
   - Recomendação: Opção C

3. **`docs/sme-contexto-completo.md`**
   - Contexto SME completo
   - 600+ escolas
   - Transporte gargalo
   - Divisão atual (3 duplas)

4. **`docs/sme-respostas-5-perguntas.md`**
   - Respostas às 5 perguntas
   - 4 de 5 respondidas

5. **`docs/sme-nomes-corretos.md`**
   - 15 técnicos com nomes oficiais
   - Correções aplicadas

6. **`docs/sme-contexto-divisao-roteamento.md`**
   - Divisão de técnicos
   - Roteamento inteligente
   - Monitoramento atual (Zabbix + GLPI)

7. **`docs/sme-roteamento-opcoes-tecnicas.md`**
   - OSRM pesquisado
   - Viajante=Waze (sem API)
   - Como implementar roteamento

### Git Commits

- `0b4162f` - Add SME contexto completo
- `677e13f` - Add SME nomes corretos
- `a60fa85` - Add enroladores + OSRM
- `28ed17c` - Add divisão + roteamento
- `8c65738` - Add técnicos organização
- `0b4162f` - Add contexto completo + respostas

---

## 🤔 DÚVIDAS PENDENTES

### 1. Acesso GLPI
- ❓ Você tem acesso GLPI hoje?
- ❓ API REST disponível?
- ❓ Acesso direto ao banco de dados?
- ❓ Credenciais de acesso?

### 2. SRM (Sistema Municipal)
- ❓ SRM existe?
- ❓ SRM tem API?
- ❓ O que SRM fornece? (geolocalização, rotas?)
- ❓ Como acessar SRM?

### 3. Dados Escolas
- ❓ Lista completa de 600+ escolas?
- ❓ Coordenadas GPS de cada escola?
- ❓ Distrito de educação de cada escola?
- ❓ Endereços atualizados?

### 4. Técnicos - Dados Faltantes
- ❓ Emails dos 15 técnicos?
- ❓ Telefones dos 15 técnicos?
- ❓ IDs no GLPI?
- ❓ Calendário de férias (mês de cada)?

### 5. Métricas Atuais
- ❓ Quantidade chamados/categoria (30 dias)?
- ❓ Performance individual (30 dias)?
- ❓ Taxa de resolução 1ª visita?
- ❓ SLA atual por categoria?

### 6. Enroladores
- ✅ Identificados: Kervin Rocha + Alisson Ferreira
- ❓ Como monitorar redes sociais?
- ❓ Como medir produtividade deles?
- ❓ Como corrigir comportamento?

### 7. Transporte
- ❓ Quantos carros oficiais além do 1?
- ❓ Orçamento para táxi por mês?
- ❓ Custo médio por viagem de táxi?
- ❓ Métricas para justificar mais carros?

### 8. Monitoramento (Zabbix + GLPI)
- ❓ Como automatizar Zabbix → GLPI?
- ❓ Quais alertas Zabbix são críticos?
- ❓ Quem abre chamados GLPI hoje?

---

## 🎯 PRÓXIMOS PASSOS PARA .249

### Fase 1: Dados Base (NECESSÁRIO)

**1. Mapear Escolas**
- Lista 600+ escolas
- Nome + Endereço + Coordenadas GPS
- Distrito de educação
- Salvar em banco de dados

**2. Mapear Técnicos**
- 15 técnicos com dados completos
- Email, telefone, ID GLPI
- Calendário de férias
- Perfil (senior/junior, resolutivo/enrolador)

**3. Extrair Métricas GLPI**
- Chamados por categoria (30 dias)
- Performance individual
- Tempo médio resolução
- Taxa de retornos

### Fase 2: Roteamento (.249)

**1. Instalar OSRM**
- Baixar OSRM para .249
- Baixar dados OpenStreetMap (Fortaleza/CE)
- Configurar servidor local
- Testar API

**2. Criar API Roteamento**
- Input: escola origem + escola destino
- Output: tempo real + distância real
- Considerar trânsito
- Endpoint: `POST /route/calculate`

**3. Integrar no .249**
- Consulta OSRM para cada rota
- Calcula tempo total (viagem + serviço)
- Otimiza ordem de visitas
- Maximiza atendimentos/dia

### Fase 3: Distribuição Inteligente

**1. Regras de Distribuição**
- Técnico disponível? (não de férias)
- Técnico capaz? (senior/junior)
- Distância razoável? (tempo real)
- Categoria compatível? (se especialista)

**2. Evitar Seleção Manual**
- Sistema atribui chamado
- Técnico NÃO escolhe
- Baseado em capacidade + disponibilidade
- Igualitário (não favoritismo)

**3. Monitorar Enroladores**
- Kervin Rocha + Alisson Ferreira
- Rastrear produtividade
- Alertar se baixo desempenho
- Métricas de redes sociais?

### Fase 4: Dashboard + Métricas

**1. Métricas do Frank**
- Atendimentos por mês
- Tempo médio atendimento
- Satisfação usuários
- Coordenadorias cobertas

**2. Métricas Gerais**
- Tempo espera (meta: reduzir 2 meses → dias)
- Taxa resolução 1ª visita
- Retornos por técnico
- Custo por atendimento

**3. Justificar Recursos**
- Mostrar valor da equipe
- Pedir mais carros oficiais
- Pedir mais técnicos se necessário
- Dados concretos para gestão

### Fase 5: Automação Zabbix → GLPI

**1. Alertas Automáticos**
- Zabbix detecta problema
- .249 recebe alerta
- Abre chamado GLPI automaticamente
- Distribui para técnico disponível

**2. Reduzir Trabalho Manual**
- Hoje: manual (abrir chamado)
- Futuro: automático
- Agilidade + precisão

---

## 📋 CHECKLIST PARA O .249

### Mínimo Viável (MVP)

- [ ] Dados escolas (nome, endereço, GPS)
- [ ] Dados técnicos (email, telefone, férias)
- [ ] Métricas GLPI (30 dias)
- [ ] OSRM instalado
- [ ] API roteamento funcionando
- [ ] Regras básicas de distribuição
- [ ] Evitar seleção manual

### Completo

- [ ] Dashboard métricas Frank
- [ ] Dashboard geral
- [ ] Monitorar enroladores
- [ ] Automação Zabbix → GLPI
- [ ] Otimizar rotas diárias
- [ ] Calcular custo/benefício

---

## ❓ MINHAS DÚVIDAS PRINCIPAIS

### CRÍTICAS (bloqueiam MVP)

1. **Acesso GLPI**
   - Como acessar? API? Banco?
   - Credenciais?

2. **Dados Escolas**
   - Tem lista 600+ escolas?
   - Coordenadas GPS?

3. **Dados Técnicos**
   - Emails/telefones/IDs?
   - Calendário férias?

### IMPORTANTES (aceleram)

4. **SRM**
   - Existe? API?
   - Fornece o que?

5. **Transporte**
   - Quantos carros?
   - Orçamento táxi?

6. **Métricas**
   - Tem hoje?
   - Como medir?

### DESEJÁVEIS (refinam)

7. **Zabbix**
   - Como automatizar?
   - Quais alertas?

8. **Enroladores**
   - Como monitorar?
   - Como corrigir?

---

## 🎯 RESPOSTA À PERGUNTA

**"Tudo salvo estruturado para .249?"**
- ✅ **SIM!** Todos documentos criados
- ✅ **SIM!** Git commits feitos
- ✅ **SIM!** 15 técnicos identificados
- ✅ **SIM!** Enroladores confirmados
- ✅ **SIM!** OSRM pesquisado
- ✅ **SIM!** Estratégia híbrida escolhida

**"Quais dúvidas tenho?"**
- 8 dúvidas listadas acima
- 3 críticas (bloqueiam MVP)
- 3 importantes (aceleram)
- 2 desejáveis (refinam)

**"O que é necessário para .249 ser o agente?"**
- Checklist completo acima
- MVP: 6 itens mínimos
- Completo: 12 itens

---

## 🚀 PRÓXIMO PASSO SUGERIDO

**Recomendação:**
1. Responder 3 dúvidas críticas (GLPI, escolas, técnicos)
2. Começar MVP (mínimo viável)
3. Iterar com feedback

**Posso começar AGORA se você der:**
- Acesso GLPI (ou banco de dados)
- Lista escolas (nome + endereço + GPS)
- Dados técnicos (email + telefone + férias)

**Ou aguardar mais contexto?** 🎯
