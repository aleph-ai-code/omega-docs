# PRD — OpenClaw: Configuração, Arquitetura de Agentes e Automação

**Autor:** Ω · Denalth
**Data:** 2026-05-16
**Fonte:** Transcrições de vídeos do canal Bruno Okamoto no YouTube

---

## 📋 Mapa dos Vídeos-Fonte

| # | Título | Tema Principal |
|---|--------|---------------|
| 1 | Do zero ao Multi-agente / OpenClaw | Minicurso completo: setup, identidade, memória, skills, multi-agente |
| 2 | OpenClaw do ZERO! Como ter seu próprio agente de IA fácil | Tutorial VPS + Hostinger + Telegram + MCP Zapper |
| 3 | OpenClaw do Zero: Crie seu Primeiro Agente com 8.000 Integrações | Setup passo a passo com integrações via Zapper/MCP |
| 4 | Montei minha própria empresa com IA utilizando o OpenClaw | Workflow multi-agente, arquitetura real de uso, custos |
| 5 | 30 dias com um agente de IA: o que ele aprendeu sobre meu negócio me assustou | Evolução da memória, multi-agente na prática, reports automáticos |
| 6 | O jeito certo de construir um agente de IA | Boas práticas, personalidade, treinamento, segurança |
| 7 | A FEBRE OPENCLAW! | Visão geral do ecossistema, nomenclatura "Claw", competições |
| 8 | Como estamos construindo uma nova Startup de SaaS (da ideia ao primeiros clientes) | Validação de ideia com OpenClaw, landing page, tráfego, SEO |
| 9 | Como montar seu time de agentes de IA do zero com OpenClaw (do ideia ao MVP) | Multi-agente, skills, Spec Kit, Get Shit Done, controle de missão |
| 10 | Como validar seu MVP e fidelizar seus clientes | Onboard, Aha Moment, retenção, comunidade, ICP |
| 11 | 7 milhões de views, 3 produtos e uma lição sobre monetizar com IA | Monetização, SaaS, Micro-SaaS, build in public |
| 12 | Tudo sobre Micro-SaaS: O guia completo | Guia completo Micro-SaaS: conceito, métricas, precificação, ferramentas |

---

## 1. Configurações Avançadas do OpenClaw

### 1.1 Infraestrutura e VPS

- **Não rodar no computador pessoal** — segurança e disponibilidade 24/7
- **VPS recomendada:** Hostinger (OpenClaw pré-instalado, medidas de segurança nativas)
- **Planos:** iniciar com plano básico (~R$ 30/mês, 8GB RAM) e escalar conforme necessidade
- **Rede:** Usar **Tailscale** para acesso seguro ao gateway (VPN mesh)
- **Auto-reset:** Configurar reset automático quando o agente bate no teto de contexto

### 1.2 Heartbeats

- Heartbeats são verificações periódicas automáticas do agente
- **Dica de modelos:** Usar modelos leves (Haiku) para heartbeats → economia de tokens
- Heartbeats podem verificar: emails, calendário, métricas, integridade de serviços
- Frequência típica: a cada 30 minutos, com rodízio de tarefas
- **`HEARTBEAT.md`** — arquivo de checklist para o agente consultar durante heartbeats
- **`heartbeat-state.json`** — rastrear última verificação de cada tarefa

### 1.3 Cron / Agendamentos

- **Cron jobs** para tarefas de horário exato (ex: "todo dia às 7h, resumo de emails")
- Diferença de heartbeat: cron é isolado, timing exato, pode usar modelo diferente
- Configurado via conversa natural: "Me envie todo dia às 7 da manhã um resumo dos últimos 5 emails"
- O OpenClaw cria jobs nomeados (ex: "daily 5 email summary for André")

### 1.4 Skills (Super Habilidades)

- **Skills são plugins** que dão capacidades extras ao agente
- Podem ser instaladas via comando natural: "Instale a skill XYZ"
- **Skills mencionadas:**
  - **Spec Kit** — ajuda a desenvolver MVPs (requirements, roadmap, planning)
  - **Get Shit Done** — limita contexto, ajuda a construir produtos passo a passo
  - **Browser/Chrome** — permite ao agente navegar na web, logar em ferramentas
  - **SEO Skills** — palavras-chave, análise de mercado
  - **Skills de voz/TTS** — storytelling com diferentes vozes
- Repositório de skills curadas pela comunidade no GitHub
- Skills são compartilháveis e a comunidade contribui com novas

### 1.5 Plugins e MCP

- **MCP (Model Context Protocol)** — protocolo que permite conexão com ferramentas externas
- **Zapper** — principal MCP usado, conecta com **8.000+ aplicativos**
- Configuração: conectar URL do Zapper no OpenClaw via dashboard
- Permissões granulares: pode dar/tirar acesso a ferramentas específicas
- MCP Porter — para instalar e gerenciar conexões MCP

### 1.6 Multi-Agente

- **Arquitetura:** Agente principal (Squad Lead) + agentes especialistas
- Agentes podem ser independentes (topic files separados) ou compartilhados
- **Casos de uso por agente:**
  - **Back-end** — código, APIs, banco de dados
  - **Front-end** — UI, UX, design
  - **Customer Support** — tickets, WhatsApp, respostas
  - **Content Producer** — roteiro, posts, carrosséis, vídeos
  - **Email Marketing** — campanhas, automações
  - **Social Media** — posts, agendamento, engajamento
  - **Product Manager** — PRDs, roadmap, sprints
  - **SEO Specialist** — palavras-chave, analytics
  - **Sales/Outbound** — prospecção, LinkedIn, CRM
- **Controle de Missão:** painel tipo Kanban para visualizar todos os agentes
- Baseado no artigo do Banu (Squad Lead Jarvis)

---

## 2. Patterns e Arquiteturas

### 2.1 Como Montar um Agente (Zero ao MVP)

1. **Contratar VPS** com OpenClaw pré-instalado (Hostinger)
2. **Configurar segurança** — senhas, firewall, Tailscale
3. **Conectar Telegram** — via BotFather, token de API, código de pareamento
4. **Criar identidade** — nome, personalidade, pronome, tom de voz (SOUL.md, IDENTITY.md)
5. **Estruturar memória** — AGENTS.md, MEMORY.md, arquivos de contexto
6. **Adicionar ferramentas** — MCP Zapper, APIs, skills
7. **Treinar o agente** — dar contexto sobre empresa, processos, preferências
8. **Configurar proatividade** — heartbeats, cron, permissões de ação autônoma

### 2.2 Arquitetura Multi-Agente

```
┌─────────────────────────────────────┐
│         Squad Lead (Main)           │
│    Coordena, delega, supervisiona    │
├──────────┬──────────┬───────────────┤
│ Content  │ Support  │   Product     │
│ Amora    │ Agent    │   Manager     │
├──────────┼──────────┼───────────────┤
│ Marketing│ SEO      │   Dev Agent   │
│ Amora    │ Agent    │               │
└──────────┴──────────┴───────────────┘
```

- **Squad Lead** é o agente principal que coordena os demais
- Cada agente tem seu **topic file** (contexto isolado)
- Memória pode ser compartilhada ou isolada por agente
- Comunicação entre agentes via **subagent spawning**

### 2.3 Delegação

- O agente principal pode criar subagentes para tarefas específicas
- Pode passar PRDs e prompts para subagentes implementarem
- "Pegue esse conteúdo e implemente tudo" — delegação total
- **Cuidado:** Recomenda-se treinar antes de delegar totalmente

---

## 3. Integrações

### 3.1 Ferramentas Conectadas

O OpenClaw se integra com **8.000+ aplicações** via Zapper/MCP:

| Categoria | Ferramentas |
|-----------|------------|
| **Email** | Gmail, Outlook |
| **Calendário** | Google Calendar |
| **Planilhas** | Google Sheets |
| **CRM** | RD Station, HubSpot, Pipedrive |
| **CMS** | WordPress |
| **Social Media** | Instagram, LinkedIn, X/Twitter |
| **Pagamento** | Stripe, Supabase |
| **Analytics** | Google Analytics, Google Search Console |
| **SEO** | DataForSEO |
| **Comunicação** | Telegram, WhatsApp (API oficial) |
| **Automação** | N8N, Zapier, Make |
| **No-Code** | Bubble, Replit, Lovable |
| **Suporte** | Crisp |
| **Banco de Dados** | Supabase, PostgreSQL |
| **Browser** | Chrome (navegação autônoma) |
| **Imagens** | Gemini (geração de imagens para ads) |

### 3.2 Como Conectar

- **Via MCP:** Adicionar URL do serviço no OpenClaw, dar permissões
- **Via API:** Colocar chave de API na pasta de APIs do agente
- **Via Cookie:** Para serviços sem API, o agente captura cookie de sessão e loga diretamente
- **Via Browser:** O agente usa Chrome para navegar e interagir com qualquer site

### 3.3 Integrações por Linguagem Natural

- Basta pedir: "Conecte meu Gmail" e o agente guia o processo
- "Pegue a lista de clientes da RD Station e envie mensagem X"
- "Gere relatório de métricas do Google Analytics"

---

## 4. Automações Práticas

### 4.1 Marketing e Aquisição

- **Social Media automático** — agenda posts, responde DMs, coleta comentários, gera relatórios de engajamento
- **Prospecção automática** — entra no LinkedIn via Rapid API, extrai empresas de um nicho, monta lista de prospects
- **SEO automático** — pesquisa palavras-chave de cauda longa, analisa concorrência
- **Conteúdo em escala** — corta vídeos longos em shorts, gera carrosséis para LinkedIn
- **Crescimento de perfil** — replies automáticos em posts, comentários estratégicos
- **Criação de landing pages** — via conversa natural
- **Testes de tráfego pago** — criar e gerenciar campanhas

### 4.2 Operações Internas

- **Secretária eletrônica** — agendar tarefas, gestão de projetos, lembretes
- **Resumo diário de emails** — "Me envie todo dia às 7h um resumo dos últimos 5 emails"
- **Análise de CRM** — quais leads estão parados, quais converter, sugerir ações
- **Monitoramento de funil** — visualizar gargalos, sugerir melhorias
- **Gestão de backlog** — manter lista de tarefas organizada e priorizada

### 4.3 Conteúdo e Produção

- **Roteiro de vídeos** — gerar roteiro completo com base em transcrições
- **Transcrição automática** — gravou vídeo > agente transcreve e processa
- **Geração de carrosséis** — automaticamente a partir de transcrições
- **Post scheduling** — agendar para todas as redes sociais
- **Análise de performance** — quais categorias de conteúdo performam melhor
- **Tom de voz adaptativo** — tom diferente para YouTube vs LinkedIn

### 4.4 Produto e Dev

- **Code review** — agente lê repositório, analisa branch, revisa PRs, detecta código problemático
- **Geração de PRDs** — a partir de conversas, o agente monta PRDs completos
- **MVP em 48h** — caso real de MVP construído em 2 dias
- **Testes automatizados** — rodar K antes de merge

### 4.5 Suporte e Customer Success

- **Respostas automáticas** — via WhatsApp/Telegram
- **Visão 360 do cliente** — cruzar dados de CRM, suporte, pagamentos
- **Suporte proativo** — detectar churn antes, sugerir ações
- **Onboard guiado** — agent pode conduzir novo usuário pelo sistema

---

## 5. Lições Aprendidas

### 5.1 Tokens e Custos

- **Problema:** Documentos de memória muito grandes (10MB+) = estouro de tokens
- **Solução:** Usar topic files separados, limitar contexto por agente
- **Dica de modelos (Alex Fin):**
  - **Opus/Kim** para cérebro (raciocínio complexo)
  - **Haiku** para heartbeats (automações leves)
  - **Codex/MiniMax** para codar
  - **Opus** para web search e conteúdo
- **VPS Hostinger plano R$ 110/mês** — nunca estourou o limite
- **Economia:** substituir 10 ferramentas SaaS por um único OpenClaw
- **Dica:** Passar print da tabela de modelos pro agente e dizer "Implemente essas regras"

### 5.2 O Que Funciona

- ✅ Tratar o agente como funcionário — treinar, dar contexto, estabelecer limites
- ✅ Começar simples — um agente, poucas tarefas
- ✅ Usar auto-reset para evitar crash de contexto
- ✅ Templates de memória — comparar evolução ao longo do tempo
- ✅ PRDs + Prompts — documentar tudo para reprodutibilidade
- ✅ Build in public — compartilhar jornada gera audiência e feedback
- ✅ Personalidade definida — SOUL.md bem construído = respostas melhores
- ✅ Skills prontas (Spec Kit, Get Shit Done) — aceleram MVP

### 5.3 O Que NÃO Funciona

- ❌ Dar todas as ferramentas sem treinamento — "se vira aí" não funciona
- ❌ Ignorar a curva de aprendizado — precisa investir tempo ensinando
- ❌ Memória única gigante — topic files separados são essenciais
- ❌ Delegar tudo de uma vez — treinar primeiro, delegar depois
- ❌ Instalar no computador pessoal — risco de segurança, não é 24/7
- ❌ Não monitorar — precisa de heartbeat e reports diários

### 5.4 Evolução da Memória (Fases do Bruno)

1. **Sem memória** — agente nasce zerado
2. **Contextos manuais** — colar contexto a cada conversa
3. **Topic files** — arquivos separados por assunto
4. **Sistema de memória inteligente** — automático, categorizado, persistente

---

## 6. Estratégias de Memória

### 6.1 Arquivos Core do Agente

| Arquivo | Função |
|---------|--------|
| **AGENTS.md** | Regras gerais, convenções, diretrizes de comportamento |
| **SOUL.md** | Personalidade, tom de voz, estilo, opiniões |
| **IDENTITY.md** | Nome, emoji, assinatura, frase, papel |
| **USER.md** | Quem é o humano, preferências, contexto |
| **TOOLS.md** | Notas sobre ferramentas locais, credenciais |
| **MEMORY.md** | Memória de longo prazo curada |
| **memory/YYYY-MM-DD.md** | Notas diárias brutas |
| **HEARTBEAT.md** | Checklist de verificações periódicas |

### 6.2 Organização

- **Diário vs Longo Prazo:** daily notes são logs brutos; MEMORY.md é a destilação
- **Revisão periódica:** durante heartbeats, revisar dailies e atualizar MEMORY.md
- **Limpeza:** remover informação obsoleta do MEMORY.md
- **Compartilhamento entre agentes:** memória pode ser compartilhada ou isolada por topic

### 6.3 Dicas de Memória

- Escrever tudo — "mental notes" não sobrevivem a restart
- Contexto do projeto em arquivo separado para economizar tokens
- Templates de memória permitem comparar evolução
- Conhecimento sobre o guia do OpenClaw como base de pesquisa embutida

---

## 7. Segurança

### 7.1 Boas Práticas Mencionadas

- **VPS dedicada** — nunca rodar no PC pessoal
- **Tailscale** — rede VPN mesh para acesso seguro
- **Senha forte** no gateway — não compartilhar com ninguém
- **Token de API secreto** — guardar como senha, não divulgar
- **Permissões granulares** — dar acesso apenas ao necessário via Zapper
- **Blindagem** — configurar para que pessoas não soltem prompts arbitrários
- **Firewall da VPS** — mecanismos de segurança que bloqueiam acessos indevidos
- **Pareamento** — código de verificação para conectar Telegram

### 7.2 Riscos Identificados

- **PC pessoal = acesso total** — o agente tem acesso a senhas, arquivos, tudo
- **Token exposto** — se vazar, qualquer um controla o agente
- **Prompt injection** — pessoas podem tentar manipular o agente
- **Cookie de sessão** — expira em 1-2 meses, precisa renovar

---

## 8. Monetização e Negócios

### 8.1 Modelos de Negócio com OpenClaw

- **Micro-SaaS com agentes** — produto simples, nichado, com agente embutido
- **Claw-as-a-Service** — vender agentes configurados para clientes
- **Social Media automation** — gerenciamento automático de perfis
- **Customer Support bots** — WhatsApp/Telegram automatizado
- **Prospecção automática** — outbound com agente no LinkedIn
- **Mentoria/Consultoria** — ensinar a montar agentes

### 8.2 Micro-SaaS + OpenClaw = Combo Ideal

- **No-Code + OpenClaw** — Bubble, N8N, Replit para produto
- **MVP em 48h** — velocidade de validação absurda
- **Agente como diferencial** — resumo de WhatsApp, suporte automático, insights
- **Caso MGM (Bruno):** agente que resume mensagens de grupos WhatsApp, plugado via N8N

### 8.3 Lições de Monetização

- **Precificação:** nunca vender barato (< R$ 50/mês), seguir benchmarks e proposta de valor
- **Onboard manual** — no início, fazer tudo na mão para entender jornada do cliente
- **Aha Moment** — identificar quando o cliente percebe valor = churn cai drasticamente
- **ICP focado** — começar amplo, funilar para 1-2 personas
- **Comunidade** — grupo WhatsApp/Telegram com primeiros clientes = evangelistas
- **Build in public** — compartilhar jornada = distribuição + audiência
- **Métricas-chave:** MRR, churn rate, ticket médio, taxa de conversão, NPS, CAC

### 8.4 Números do Bruno (Referência)

- Micro-SaaS faturando ~R$ 11.000/mês (~R$ 136.000/ano)
- Minicurso: 3.000 alunos a R$ 90, taxa de reembolso < 2%
- Comunidade Micro-SaaS: 3.000+ membros, ~20 transações de compra/venda de SaaS
- 62% da comunidade são desenvolvedores

---

## 9. Conceitos-Chave e Glossário

| Termo | Definição |
|-------|-----------|
| **Claw** | Agente pessoal de IA (nomenclatura de Andrej Karpathy) |
| **OpenClaw** | Framework open-source para construir agentes pessoais de IA |
| **MCP** | Model Context Protocol — protocolo de integração com ferramentas |
| **Skill** | Plugin/capacidade extra instalável no agente |
| **Heartbeat** | Verificação periódica automática do agente |
| **Topic File** | Arquivo de contexto isolado por assunto/agente |
| **VPS** | Virtual Private Server — servidor virtual privado |
| **Auto-reset** | Reset automático quando contexto atinge limite |
| **Squad Lead** | Agente principal que coordena multi-agentes |
| **PRD** | Product Requirements Document — documento de requisitos |
| **MRR** | Monthly Recurring Revenue — receita mensal recorrente |
| **ICP** | Ideal Customer Profile — perfil ideal de cliente |
| **Aha Moment** | Momento em que o cliente percebe o valor do produto |

---

## 10. Checklist Rápido: Do Zero ao Agente Funcionando

1. [ ] Contratar VPS com OpenClaw pré-instalado (Hostinger)
2. [ ] Configurar Tailscale para acesso seguro
3. [ ] Configurar senha forte e firewall
4. [ ] Conectar Telegram via BotFather
5. [ ] Criar identidade (SOUL.md, IDENTITY.md, USER.md)
6. [ ] Estruturar memória (AGENTS.md, MEMORY.md, pasta memory/)
7. [ ] Configurar fuso horário e idioma
8. [ ] Conectar MCP Zapper + APIs necessárias
9. [ ] Instalar skills (Chrome, SEO, etc.)
10. [ ] Treinar o agente com contexto da empresa
11. [ ] Configurar heartbeat (HEARTBEAT.md)
12. [ ] Configurar cron jobs para tarefas diárias
13. [ ] Testar e iterar

---

*Documento gerado a partir da análise de 12 transcrições de vídeo do canal Bruno Okamoto.*
*Ω · Denalth — 2026-05-16*
