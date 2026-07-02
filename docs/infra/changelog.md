# Changelog - OpenClaw Omega

## [2026-06-02] - Correção Crítica Crons

### 🚨 ISSUE RESOLVIDO
**Watchdog-crons 4 falhas consecutivas**

### ✅ MUDANÇAS

**1. Migração Crons para Ollama Local**
- **Antes:** ZAI GLM-4.5-flash (rate limit instável)
- **Depois:** ollama/qwen3:4b (32.7 tok/s, 100% estável)
- **Fallback:** ollama/qwen2.5-coder:3b (49.5 tok/s)

**2. Crons Atualizados**
- watchdog-crons
- omega:security-audit
- omega:auto-melhoria
- omega:docs-review
- omega:git-autosave
- atlas:git-autosave

**3. MEMORY.md Atualizado**
- Removida dependência ZAI para crons
- Adicionada nota sobre resolução do issue
- Atualizada seção de APIs configuradas

**4. Memory Diario**
- Criado `memory/2026-06-02.md` com diagnóstico completo

### 📊 IMPACTO
- **Antes:** Cronds falhando com rate limits ZAI
- **Depois:** 100% dependência local, sem APIs externas

### 🔧 ARQUIVOS MODIFICADOS
- `MEMORY.md` - atualizado seção crons e APIs
- `memory/2026-06-02.md` - novo (diagnóstico completo)
- `docs/changelog.md` - este arquivo

### 🎓 LIÇÕES APRENDIDAS
1. ZAI não é confiável para crons críticos
2. Ollama local é o caminho para operações estáveis
3. Timeout de rede ≠ crash de servidor
4. Sempre ter fallback local para APIs externas

---

## [2026-05-26] - Separação de Agentes

### ✅ CONCLUÍDO
- **Omega (.240)** → Profissional/infra/sistemas
- **Aleph (.250)** → Pessoal/Gabinete 360/finanças
- **Voyager (.251)** → Travel Specialist (viagens, milhas)

### 📦 BACKUP
- Criado `backups/2026-05-26/separacao/`

---

## [2026-05-23] - Dia Mais Produtivo

### ✅ CONQUISTAS
- 4 servidores trabalhados
- Travel Specialist operacional
- LLM server rodando (241)
- Tudo limpo e documentado

---

## [2026-05-21] - Regra Consulta Help-me

### 📝 DECISÃO
- **Única tabela autorizada:** `vw_cadastro_completo`
- **NÃO usar:** `tb_municipios`, `Municipity`
- **Motivo:** `tb_municipios` tem reached_votes desatualizado

---

Ω · Denalth
