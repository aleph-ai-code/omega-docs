# Ideias Futuras - Gabinete 360 (Supabase)

> POC validada em 17/05/2026
> Status: Funcional, expansões pendentes

## O que já funciona ✅
- Conexão Supabase (357 lideranças)
- Dashboard HTML interativo
- Relatório formatado via Telegram
- Tokens seguros no Bitwarden

## Expansões planejadas 📋

### 1. Automação
- [ ] Cron job diário/semanal para envio automático
- [ ] Escolher melhor horário (evitar horas de pico)
- [ ] Envio por responsável (JUSSARA, RUDI, CAPITÃO)

### 2. Personalização por responsável
- [ ] JUSSARA só vê o que precisa
- [ ] RUDI só vê o que precisa
- [ ] CAPITÃO só vê o que precisa
- [ ] Relatório consolidado pra Denalth

### 3. Alertas em tempo real
- [ ] Novos FECHADOS no dia → notificação
- [ ] Mudança de status (COBRAR → FECHADO)
- [ ] Metas atingidas (votos prometidos)
- [ ] Alertas por município

### 4. Relatórios específicos
- [ ] Por município (escolher um)
- [ ] Por tipo de liderança
- [ ] Por status
- [ ] Comparativo (dia X vs dia Y)

### 5. Dashboard ao vivo
- [ ] Atualização automática (websocket/polling)
- [ ] Filtros interativos
- [ ] Exportar CSV/Excel
- [ ] Gráficos adicionais (evolução temporal)

## Requisitos técnicos
- Bot Telegram configurado: ✅
- Supabase API key: ✅
- Bitwarden tokens seguros: ✅
- Script de geração de relatório: ✅
- HTML dashboard: ✅

## Próximos passos quando retomar
1. Definir frequência de envio (diário? semanal?)
2. Definir horário ideal
3. Criar versões personalizadas por responsável
4. Implementar alertas

## Notas
- User 8519008174 precisa iniciar conversa com @aleph_ai_bot antes de receber mensagens
- Bot só envia pra usuários que deram /start
- Dashboard HTML tá em docs/dashboard-liderancas.html
