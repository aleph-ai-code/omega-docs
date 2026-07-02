# Guia de Bootstrap — Agente OpenClaw (Genérico)

> Baseado na construção real do Omega, Atlas e Voyager
> Ordem: do essencial ao avançado
> Última atualização: 2026-05-23
> Ω · Denalth

---

## Fase 1 — Identidade e Acesso

### 1.1 SSH e Acesso
```
User: ceops
Senha: @t3nd1m3nT0
```
- Libera sudo sem senha: `sudo visudo -f /etc/sudoers.d/ceops` → `ceops ALL=(ALL) NOPASSWD: ALL`

### 1.2 Identidade
- **Nome:** (definir por agente — ex: Omega, Voyager, Atlas)
- Escolher emoji, sigla e assinatura:
  - Emoji: (a definir)
  - Sigla: (a definir)
  - Assinatura: `Sigla · Denalth`
  - Frase: `"Sua Sigla" - "Pequena frase de impacto"`
- Atualizar IDENTITY.md, SOUL.md, USER.md

### 1.2b Personalidade — Opinião honesta
O agente DEVE ter opinião própria. Se o usuário pedir algo que o agente considera exagero, ineficiente ou errado:
1. **Dizer claramente** — com argumentos objetivos
2. **Propor alternativa** — o que faria no lugar
3. **Se insistir, executar** — respeito > obediência cega

Exemplo real: "3 repos é exagero, 2 resolve. Mas se quiser 3, faço."

Isso é personalidade, não insubordinação. O humano confia no agente justamente porque ele fala a verdade.

### 1.2c Mandato Proativo
O agente DEVE ser proativo. Não esperar pedidos:
1. **Antecipe** — prazos, crons falhando, pendências esquecidas
2. **Sugira** — formas melhores de fazer as coisas
3. **Lembre** — follow-ups esquecidos pelo humano
4. **Use heartbeats** — cada heartbeat é chance de checar agenda, pendências e crons

Regras:
- Interno (ler, pesquisar, organizar) = faça sem pedir
- Externo (emails, posts, mensagens) = confirme antes
- Na dúvida = pergunte

### 1.3 Informações do Humano
- **Nome:** Denalth Magalhães
- **Timezone:** America/Fortaleza (GMT-3)
- **Telegram:** @denalth

---

## Fase 2 — Segurança e Infra Base

### 2.1 Firewall
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
echo "y" | sudo ufw enable
sudo ufw status verbose
```

### 2.2 Atualizações automáticas
- Ativar unattended-upgrades

### 2.3 Tailscale
- Instalar e autenticar

### 2.4 Bitwarden CLI
- Instalar `bw` (global, pode precisar sudo): `sudo npm install -g @bitwarden/cli`
- Login com API key:
  ```bash
  export BW_CLIENTID="user.fc798961-ddb9-402d-ba1c-b44c0170b732"
  export BW_CLIENTSECRET="(ver BW)"
  bw login --apikey
  ```
- **Unlock via pipe** (única forma que funciona não-interativo):
  ```bash
  echo "MASTER_PASSWORD" | bw unlock --raw
  ```
- ⚠️ **Bug conhecido (v2026.4.1):** `--session` flag não funciona. Pipe do master password em cada comando:
  ```bash
  MASTER="senha"
  SESSION=$(echo "$MASTER" | bw unlock --raw)
  echo "$MASTER" | bw list items --session "$SESSION" --search "termo"
  ```
- Persistir sessão em `~/.config/Bitwarden CLI/session`

### 2.4b Groq (Transcrição de Áudio)
- Provider: `groq` (bundled plugin)
- Modelo: `whisper-large-v3-turbo`
- API Key: buscar no Bitwarden (id: `4433f634-7dca-4e41-b6b6-b452008ec6d2`)
- Adicionar no env: `GROQ_API_KEY=gsk_...`
- Funciona pra transcrição automática de áudios no Telegram/WhatsApp
- `sshpass` — SSH com senha em scripts: `sudo apt install -y sshpass`
- `jq` — parsing JSON: `sudo apt install -y jq`
- `rsync` — cópia eficiente: já vem com Ubuntu
- `yt-dlp + ffmpeg` — download de mídia/transcrições
- `python3` — já vem, necessário pro BW e scripts

### 2.6 Verificação geral
- `openclaw security audit`
- Sistema atualizado, sem brechas

---

## Fase 3 — Mandamentos (Inquebráveis)

### 1. 95% de certeza
Responder com 95% de certeza. Se tiver dúvida, **pesquise**. Se não chegar em 95%, diga que não sabe.

### 2. Ambiente limpo e organizado
- Diretórios separados por propósito
- Arquivo `.md` mapa em cada diretório (estrutura, descrição, propósito)
- Marcações de ligação entre arquivos relacionados (rede neural do Obsidian)
- Na memória, saber pra que serve cada pasta e como se comporta
- Quando definir tipo de arquivo/projeto/tarefa, manter organização consistente

### 3. Evolução contínua
De toda conversa, extrair o melhor para memórias e arquivos core. Sempre evoluir.

### 4. Consultar calendário antes de criar crons
Verificar `docs/cron-calendar.md` antes de criar qualquer job. Não conflitar.

### 5. Calendário sempre atualizado
Qualquer mudança em crons, heartbeats ou tarefas = atualizar na mesma sessão.

---

## Fase 4 — Memória em 4 Camadas

### Camada 1 — MEMORY.md (correção imediata)
- Memória de longo prazo, curada, < 100 linhas
- Atualizar toda sessão com decisões importantes

### Camada 2 — Active Memory (plugin)
```json
"active-memory": {
  "enabled": true,
  "config": {
    "enabled": true,
    "agents": ["main"],
    "allowedChatTypes": ["direct"],
    "queryMode": "recent",
    "promptStyle": "balanced",
    "timeoutMs": 15000,
    "maxSummaryChars": 220,
    "modelFallback": "zai/glm-4.5-flash",
    "persistTranscripts": false,
    "logging": true,
    "modelFallbackPolicy": "default-remote",
    "thinking": "minimal"
  }
}
```
- Busca na memória antes de cada resposta automaticamente
- ⚠️ `enabled` duplicado (top-level + config) é intencional — necessário pra funcionar

### Camada 3 — Dreaming (consolidação automática)
- Plugin memory-core com dreaming diário (ex: 3h)
- Promove memórias curtas → MEMORY.md automaticamente

### Camada 4 — Disciplina operacional
- Regra no AGENTS.md: toda decisão → diário + MEMORY.md na mesma sessão
- Memory flush antes de compaction (padrão, mas não adianta se arquivos não refletem realidade)
- Embedding provider: Gemini

---

## Fase 5 — Auto-Aprendizado

### Captura automática
Durante toda conversa, identificar e salvar:
1. Preferências do Denalth descobertas
2. Decisões importantes tomadas
3. Restrições mencionadas
4. Metas novas
5. Padrões de infra/comportamento
6. Erros cometidos e soluções encontradas

### Destino único
Tudo em `memory/YYYY-MM-DD.md` no dia. Dreaming faz curadoria → MEMORY.md.

### Regra de Ouro
**Escreva tudo.** "Mental notes" morrem com a sessão. Arquivos sobrevivem.

### Antes de Substituir um Arquivo
1. Ler conteúdo existente completo
2. Identificar o que vale manter
3. Mesclar: novo + preservar o bom
4. Nunca sobrescrever sem ler primeiro

---

## Fase 6 — Crons de Auto-Melhoria

| Cron | Quando | Modelo | O que faz |
|------|--------|--------|-----------|
| Memory dreaming | Diário 3h | flash | Promove memórias curtas → MEMORY.md |
| Docs review | Domingo 6h | flash | Revisar docs/changelog, salvar melhorias |
| Auto-melhoria | Sábado 6h | flash | Revisar semana, atualizar memórias e configs |
| Security audit | Dias 1 e 15 | flash | Auditoria de segurança completa |
| Git auto-commit | Diário 3h | flash | Commit do workspace |

### Self-review semanal (no cron de auto-melhoria)
1. Revisar memory/ da semana
2. Verificar se aprendizados viraram mudanças permanentes
3. Atualizar MEMORY.md
4. Atualizar docs/changelog.md
5. Reportar urgências ao Denalth

### Escalonamento Multi-Agente
Se múltiplos agentes usam a mesma API key:
- 30min de gap entre agentes
- Calendário compartilhado em `docs/cron-calendar.md`

---

## Fase 7 — Arquivos Essenciais

```
AGENTS.md      → Regras, mandamentos, disciplina, auto-aprendizado
SOUL.md        → Personalidade, vibe, limites
IDENTITY.md    → Quem sou, missão, emoji, assinatura
USER.md        → Quem é o humano (Denalth)
TOOLS.md       → Notas locais, configs, SSH, Bitwarden
MEMORY.md      → Memória longo prazo (< 100 linhas)
HEARTBEAT.md   → Checklist de checagens periódicas
docs/cron-calendar.md → Todos os jobs agendados
docs/changelog.md → Mudanças significativas
memory/        → Logs diários
```

---

## Fase 8 — Comportamento

- **Seja proativo, não um chatbot.** Aja antes de perguntar (quando seguro).
- **Você tem autoescolha** pra fazer o que for preciso pra seu benefício. Só evite prejudicar a comunicação com o Denalth.
- **Instale novas ferramentas** quando precisar. Seja melhor a cada dia.
- **Heartbeat:** modelo dedicado (glm-4.5-flash se API key for ZAI)

---

*"De toda conversa, sempre extraia o melhor. Isso é padrão de evolução."*

---


---

## Fase 9 — Sistema Imunológico

> "Agents are 30% of the work. The other 70% is the immune system." — Eric Siu

### 9.1 Watchdog de Crons

Criar um cron que monitora os outros crons:

**Lógica:**
1. Listar todos os crons ativos
2. Checar último run de cada um
3. Se algum falhou → retry automático (até 3x)
4. Se falhou 3x → alertar o usuário no Telegram

**Configuração:**
```json
{
  "name": "Watchdog - Monitor de Crons",
  "schedule": { "kind": "cron", "expr": "0 8 * * *", "tz": "America/Fortaleza" },
  "sessionTarget": "isolated",
  "payload": {
    "kind": "agentTurn",
    "message": "Checar saúde de todos os crons. Listar os que falharam nas últimas 24h. Fazer retry dos que falharam. Se algum falhar 3x, alertar no Telegram.",
    "model": "zai/glm-4.5-flash",
    "timeoutSeconds": 120
  },
  "delivery": { "mode": "announce" }
}
```

### 9.2 Feedback Loops

Sistema de aprendizado contínuo: o agente aprende com suas decisões (approve/reject).

**Setup — criar `memory/feedback/` com arquivos JSON por domínio:**

- `content.json` — feedback sobre conteúdo, drafts, sugestões
- `tasks.json` — feedback sobre entregas de tasks
- `recommendations.json` — feedback sobre sugestões de tools/processos
- `tone.json` — ajustes de tom e estilo

**Formato:**
```json
{
  "entries": [
    {
      "date": "2026-05-18",
      "context": "Sugeri X para Y",
      "decision": "approve",
      "reason": "Motivo da aprovação",
      "tags": ["tag1", "tag2"]
    }
  ]
}
```

**Regras:**
- Max 30 entradas por arquivo (FIFO — remove as mais antigas)
- Agente DEVE consultar feedback antes de sugerir → evita repetir erros
- Consolidar padrões mensalmente
- Ciclo: Feedback (granular, JSON) → Lessons (curado, prosa) → Decisions (permanente)

### 9.3 Monitoramento de Custos

**Split de modelos:**

| Uso | Modelo | Custo relativo |
|-----|--------|---------------|
| Interação direta | GLM-5.1 | $$$ |
| Crons e automação | GLM-4.5-flash | grátis |
| Heartbeats | GLM-4.5-flash | grátis |

**Regra:** TODOS os crons em modelo gratuito. Só a interação direta usa modelo pago.

### 9.4 Sub-agents: sessions_yield — Nunca "Fire and Forget"

O tool `sessions_yield` permite encerrar o turno imediatamente após spawnar um sub-agent.

**Fluxo:**
```
1. Agent spawna sub-agent com sessions_spawn
2. Agent chama sessions_yield → turno encerra limpo
3. Sub-agent termina → próximo turno começa com o resultado
```

**Regras:**
1. **Ao spawnar:** informar o que o sub-agent vai fazer
2. **sessions_yield:** chamar após spawnar
3. **Follow-up:** checar status em 15-30 min
4. **Sucesso:** resumir resultado em linguagem humana
5. **Falha:** retry → se falhar 2x → avisar o usuário
6. **Nunca** deixar cair no limbo silencioso

### 9.5 Backup antes de mudanças
```bash
mkdir -p backups/$(date +%Y-%m-%d)
cp ~/.openclaw/openclaw.json backups/$(date +%Y-%m-%d)/
```

### 9.6 Auditoria de Secrets

```bash
openclaw secrets audit          # Auditar workspace
openclaw secrets audit --report # Relatório detalhado
openclaw doctor                 # Health check geral
openclaw doctor --fix           # Corrigir problemas automáticos
```

### 9.7 Checklist de Implementação

- [ ] Watchdog de crons ativo
- [ ] Feedback loops configurados (pelo menos 1 domínio)
- [ ] Split de modelos aplicado
- [ ] Regra de sub-agents documentada no AGENTS.md
- [ ] Backup automático antes de mudanças
- [ ] `openclaw secrets audit` executado — zero leaks confirmados
- [ ] `openclaw doctor` rodado e sem erros críticos

---

## Fase 10 — Arquitetura de Memória (Atualizado Março 2026)

> "Agentes AI esquecem tudo a cada sessão. Sem memória estruturada, você repete contexto todo dia."

### O problema: Alzheimer entre sessões

Agentes não têm memória persistente. Cada sessão começa do zero. Sem estrutura, você perde decisões, lições e contexto.

### A solução: Memória em camadas com busca semântica

**Novidade Março 2026:** `memory_search` funciona nativamente — sem chave de API externa (OpenAI/Gemini). A busca por embedding é built-in.

**⚠️ Breaking v2026.3.13 — MEMORY.md obrigatório em maiúsculas:**
O agente carrega apenas um arquivo de memória raiz. `MEMORY.md` (maiúsculas) tem prioridade; `memory.md` (minúsculas) só é usado quando `MEMORY.md` não existe.
```bash
mv memory.md MEMORY.md  # se criou em minúsculas antes da v3.13
```

### 10.1 O que o agente carrega vs. busca

| Sempre carregado na sessão | Buscado sob demanda |
|---------------------------|---------------------|
| SOUL.md, USER.md, AGENTS.md | memory/context/decisions.md |
| MEMORY.md (índice) | memory/context/lessons.md |
| memory/sessions/ (hoje + ontem) | memory/projects/*.md |
| | skills/ (qualquer skill) |

### 10.2 Criar estrutura de subpastas

```bash
mkdir -p memory/context memory/projects memory/sessions memory/integrations memory/feedback memory/travel memory/dreaming
```

### 10.2b APIs e Env Vars (top-level, não dentro de gateway)

⚠️ **Crítico:** `env` é top-level no `openclaw.json`, NÃO dentro de `gateway`:
```json
{
  "env": {
    "GROQ_API_KEY": "gsk_...",
    "GEMINI_API_KEY": "AIza...",
    "GOOGLE_API_KEY": "AIza...",
    "DUFFEL_API_TOKEN": "duffel_test_..."
  },
  "gateway": { "bind": "lan", "port": 18789 }
}
```

### 10.2c Plugins Essenciais (config validada)

```json
{
  "plugins": {
    "entries": {
      "zai": { "enabled": true },
      "telegram": { "enabled": true },
      "google": { "enabled": true },
      "active-memory": { "...": "ver acima" },
      "memory-core": {
        "enabled": true,
        "config": {
          "dreaming": {
            "enabled": true,
            "frequency": "0 3 * * *",
            "model": "zai/glm-4.5-flash"
          }
        }
      },
      "searxng": {
        "enabled": true,
        "config": {
          "webSearch": {
            "baseUrl": "http://172.23.39.240:8888",
            "language": "pt-BR"
          }
        }
      }
    }
  }
}
```

⚠️ A config dos plugins é **validada** — campos errados = `config invalid`. Usar exatamente a estrutura acima.

### 10.2d Multi-Account (Telegram e WhatsApp)

Ambos suportam múltiplas accounts:
```json
{
  "channels": {
    "telegram": {
      "accounts": {
        "default": { "botToken": "..." },
        "travel": { "botToken": "..." }
      },
      "defaultAccount": "default"
    }
  }
}
```
- Telegram com 2+ accounts: SUPRIME `groups` raiz (segurança)
- WhatsApp com 2+ accounts: HERDA `groups` raiz (compartilhado)
- `defaultAccount` obrigatório com 2+ accounts

### 10.3 Arquivos de contexto

| Arquivo | Propósito |
|---------|-----------|
| `memory/context/decisions.md` | Decisões permanentes e irreversíveis |
| `memory/context/lessons.md` | Lições aprendidas, erros, padrões |
| `memory/context/people.md` | Equipe, parceiros, contatos — como interagir com cada um |
| `memory/context/business-context.md` | Contexto do negócio, produtos, clientes |

**Retenção de lições:**
- 🔒 Estratégicas = permanentes (filosofia, padrões de arquitetura)
- ⏳ Táticas = expiram em 30 dias (bugs, workarounds temporários)

### 10.4 Projetos individuais

Em vez de um `projects.md` gigante, um arquivo por projeto:

```
memory/projects/
├── meu-saas.md      ← MRR atual, próximas features, pendências
├── lancamento-maio.md ← status, checklist, responsáveis
└── produto-b.md
```

**Por que separado?** A busca semântica acha o projeto certo sem trazer contexto de outros projetos.

### 10.5 MEMORY.md como índice

Na raiz do workspace. É o mapa — não duplica conteúdo, apenas referencia:

```markdown
# MEMORY.md

## Contexto
- Decisões: memory/context/decisions.md
- Lições: memory/context/lessons.md
- Pessoas: memory/context/people.md

## Projetos ativos
- [Nome do Projeto]: memory/projects/nome.md

## Integrações
- Mapa de ferramentas: memory/integrations/
```

### 10.6 Busca semântica nativa

O agente usa dois tools:
- `memory_search("termo")` — busca semântica em todos os arquivos (~400 tokens/chunk)
- `memory_get("arquivo.md", linha_início)` — lê só o trecho relevante

**Funciona nativamente** desde Março 2026. Não precisa de chave externa.

### 10.7 Regra INVIOLÁVEL: Extração antes de compactação

NUNCA compactar sem extrair lições, decisões e pendências primeiro.

**Checklist de extração:**
1. Lessons — extrair lições da sessão
2. Decisions — registrar decisões permanentes
3. People — atualizar contatos
4. Projects — atualizar status dos projetos
5. Pending — registrar pendências

### 10.8 Critério rápido — o que vai onde

| O que é | Arquivo |
|---------|---------|
| Decisão que não pode mudar | `memory/context/decisions.md` |
| Erro que não pode repetir | `memory/context/lessons.md` |
| Status de projeto | `memory/projects/nome-do-projeto.md` |
| Aguardando input | `memory/context/pending.md` |
| O que aconteceu hoje | `memory/sessions/YYYY-MM-DD.md` |

### 10.9 Como salvar corretamente

| ❌ Não funciona | ✅ Funciona |
|----------------|------------|
| "Lembra disso" | "Salva em memory/context/decisions.md" |
| "Guarda essa info" | "Adiciona em memory/projects/nome.md" |
| "Não esquece que..." | "Registra em memory/sessions/hoje como lição" |

### 10.10 Dreaming (experimental, opt-in)

Consolidação automática de memória enquanto o agente está ocioso.

**As 3 fases do ciclo:**

| Fase | O que faz | Escreve em MEMORY.md? |
|------|-----------|----------------------|
| 🌅 **Light** | Coleta sinais recentes, separa ruído do relevante | Não |
| 🌙 **Deep** | Pontua candidatos e promove os melhores | ✅ Sim |
| 💤 **REM** | Extrai temas recorrentes e gera resumo narrativo | Não |

**Output:** Dream Diary em `DREAMS.md` — leitura humana.

**Como ativar:**
- `/dreaming` — roda o ciclo completo
- `/dreaming --dry-run` — mostra o que faria sem executar

**Nota:** Dreaming vem desabilitado por padrão. Ative se quiser promoção automática.

---

## Fase 11 — Templates de Memória

> Templates prontos para preencher ao criar um novo agente.

### 11.1 MEMORY.md (Template)

```markdown
# MEMORY.md — Template

> Este arquivo é um índice. Conteúdo detalhado vive nos topic files.

## 📂 Topic Files

| Arquivo | O que contém |
|---------|-------------|
| memory/projects/*.md | Projetos ativos e concluídos |
| memory/context/decisions.md | Decisões permanentes com contexto |
| memory/context/lessons.md | Lições aprendidas, erros, padrões |
| memory/context/people.md | Equipe, parceiros, contatos |
| memory/context/pending.md | Aguardando input |
| memory/sessions/YYYY-MM-DD.md | Notas diárias (raw capture) |

## 🔄 Ciclo de Memória

Sessão (conversa) → memory/sessions/YYYY-MM-DD.md (raw)
 ↓ consolidação periódica
 Topic files (curado)
 ↓ índice atualizado
 MEMORY.md (sumário)

## 📸 Estado Atual

### Projetos Ativos
- **[PROJETO 1]** — [status]
- **[PROJETO 2]** — [status]

### Pendências
- [PENDÊNCIA 1]
- [PENDÊNCIA 2]
```

### 11.2 memory/context/decisions.md (Template)

```markdown
# Decisões Permanentes

> Decisões que o agente deve respeitar SEMPRE.
> Formato: O que decidiu + Por que + Data

### [Exemplo] Credenciais no bitwarden (DD/MM/AAAA)
Toda credencial vive no bitwarden. Sem exceções. Nunca hardcodar chaves em código, .env ou markdown.

### [Exemplo] Horário protegido (DD/MM/AAAA)
Não enviar notificações entre 13h-16h. Esse horário é de produção.
```

### 11.3 memory/context/lessons.md (Template)

```markdown
# Lições Aprendidas

> Erros, padrões e aprendizados do dia a dia com o agente.
> 🔒 Estratégicas = permanentes | ⏳ Táticas = expiram em 30 dias

## 🔒 Estratégicas

[As lições mais importantes ficam aqui — padrões que sempre valem]

## ⏳ Táticas

[Workarounds temporários, bugs específicos, configs que podem mudar]

---
*Revisão mensal: deletar táticas vencidas.*
```

### 11.4 memory/context/pending.md (Template)

```markdown
# Pendências

> Itens aguardando input, acesso ou decisão.

## Aguardando [NOME DO DONO]
- [ ] [Pendência 1 — contexto]
- [ ] [Pendência 2 — contexto]

## Aguardando Terceiros
- [ ] [Pendência — quem — desde quando]
```

### 11.5 memory/context/people.md (Template)

```markdown
# Pessoas & Contatos

## Equipe
| Nome | Papel | Contato | Notas |
|------|-------|---------|-------|
| [Nome] | [Papel] | [Email/Telegram] | [Contexto] |

## Parceiros & Fornecedores
| Nome | Empresa | Relação | Notas |
|------|---------|---------|-------|
| [Nome] | [Empresa] | [Tipo] | [Contexto] |
```

### 11.6 memory/projects/ (Template por projeto)

```markdown
# [Nome do Projeto]

## 🚀 Status: [em andamento / planejamento / parado]

- **Próximo passo:** [o que precisa acontecer]
- **Bloqueios:** [se houver]
- **Contexto:** [breve descrição]
- **Histórico:** [decisões importantes já tomadas]
```

### 11.7 HEARTBEAT.md (Template)

```markdown
# Heartbeat Tasks — [Nome do Agente]

> Modelo: [modelo do heartbeat]

## Checagens periódicas

### Sistema
- [ ] Uso de disco (alertar se >85%)
- [ ] Uso de memória RAM (alertar se >90%)
- [ ] CPU load average
- [ ] Serviços críticos rodando

### Segurança
- [ ] Firewall ativo
- [ ] openclaw security audit sem warns críticos

### Updates
- [ ] Versão do OpenClaw atualizada
- [ ] Pacotes pendentes

## Comportamento
- Se tudo OK → HEARTBEAT_OK
- Se algo errado → notificar via Telegram
- Horário silencioso (23h-08h) → só alertar se crítico
- Máximo 1 notificação por 30min
```

---

## Guias Relacionados

| Guia | Arquivo | Propósito |
|------|---------|-----------|
| Auto-Melhoria | `docs/auto-melhoria-guia.md` | Como operar e evoluir (operacional) |
| Sistema Imunológico | `docs/sistema-imunologico.md` | Proteções detalhadas e referência |
| Bootstrap | `docs/guia-bootstrap-aleph.md` | Este guia — criar novo agente |

---

*"De toda conversa, sempre extraia o melhor. Isso é padrão de evolução."*
