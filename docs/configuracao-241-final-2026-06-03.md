# Configuração 241 - LLMs - 2026-06-03

> Servidor: ceopssrv241 (172.23.39.241)
> Hardware: i7-4770, GTX 1650 4GB, 16GB RAM, RAID1 916GB (/data)
> Ω · Denalth

## Decisões Finais

### Modelos Principais (Usar)

| Modelo | Parâmetros | Velocidade | Uso | Status |
|---|---|---|---|---|
| **phi4-mini** | 3.8B | **41.0 tok/s** | Chat geral (OpenClaw) | ✅ PRINCIPAL |
| **qwen2.5-coder:3b** | 3.1B | **49.5 tok/s** | Coder (crônicas) | ✅ PRINCIPAL |
| **qwen3:4b** | 4.0B | 32.7 tok/s | Backup chat | ✅ BACKUP |
| **gemma4:e4b** | 8.0B | 14.3 tok/s | Visão + raciocínio | ✅ ÚNICO VISÃO |
| **qwen3.5:4b** | 4.0B | ~35-40 tok/s | Coder CLI (código complexo) | ✅ INTERATIVO |
| **nomic-embed-text** | 137M | N/A | Embeddings (768 dims) | ✅ PRINCIPAL |

### Modelos Removidos (Incompatíveis/Não Usados)

- **llava:latest** - 4.1GB (OOM na GTX 1650 4GB) ❌
- **mxbai-embed-large** - Contexto 512 tokens (quebra indexação) ❌

## Configuração Ollama

### Localização Modelos (Após Migração)

```
/data/ollama/models → ~/.ollama/models (symlink)
```

### Variável Ambiente

```bash
export OLLAMA_MODELS=/data/ollama/models
```

Adicionado em: ~/.bashrc

## Configuração Crons

### Atualizados (2026-06-03)

| Job | Horário | Modelo Principal | Fallback | Status |
|---|---|---|---|---|
| omega:git-autosave | 08:50, 22:50 | phi4-mini | qwen2.5-coder:3b | ✅ Atualizado |
| omega:auto-melhoria | Dom 07:00 | phi4-mini | qwen2.5-coder:3b | ✅ Atualizado |
| omega:docs-review | Dom 07:00 | phi4-mini | qwen2.5-coder:3b | ✅ Atualizado |
| omega:security-audit | Dia 1/15 07:00 | phi4-mini | qwen2.5-coder:3b | ✅ Atualizado |

### Não Alterados

- Memory Dreaming Promotion - 03:00 (sem modelo específico)
- atlas:git-autosave - 08:30, 22:30 (qwen3:4b - outro servidor)

## Uso Recomendado por Categoria

### Chat (OpenClaw - Uso Interativo)

```bash
ollama run phi4-mini
```

**Velocidade:** 41.0 tok/s
**Backup:** qwen3:4b (32.7 tok/s)

### Coder (Crons & Automação)

```bash
ollama run qwen2.5-coder:3b
```

**Velocidade:** 49.5 tok/s
**Uso:** Crônicas, watchdog, healthcheck

### Coder (CLI - Código Complexo)

```bash
ollama run qwen3.5:4b
```

**Velocidade:** ~35-40 tok/s
**Timeout:** 5-10min (aceitável para uso interativo)
**Vantagem:** 30-40% mais inteligente (32.4% vs ~20-25%)

### Visão

```bash
ollama run gemma4:e4b
```

**Velocidade:** 14.3 tok/s
**Único compatível:** GTX 1650 4GB (llava OOM)

## Scripts Disponíveis

### Migração Completa

```bash
/tmp/complete-ollama-setup-241.sh
```

**Ações:**
1. Criar /data/ollama/models
2. Parar Ollama
3. Copiar modelos para /data
4. Configurar ~/.bashrc
5. Criar symlink
6. Remover llava e mxbai-embed-large
7. Reiniciar Ollama
8. Testar

### Limpeza Apenas

```bash
/tmp/cleanup-ollama-241.sh
```

**Ações:**
1. Remover llava:latest
2. Remover mxbai-embed-large:latest

## Ranking Velocidade (Testado)

1. qwen2.5-coder:3b — 49.5 tok/s ⚡
2. phi4-mini — 41.0 tok/s ⭐
3. qwen3:4b — 32.7 tok/s
4. gemma4:e4b — 14.3 tok/s

## Espaço Disco

### Principal (/)

- Tamanho: 107GB
- Usado: 34GB
- Disponível: 69GB
- Uso: 34%

### RAID1 (/data)

- Tamanho: 916GB
- Usado: 2.1MB
- Disponível: 870GB
- Uso: 1%

## Comandos Úteis

### Ver Modelos

```bash
ollama list
curl -s http://172.23.39.241:11434/api/tags | jq .
```

### Remover Modelo

```bash
ollama rm <modelo>
```

### Testar Modelo

```bash
time ollama run <modelo> "Teste"
```

### Ver Uso GPU

```bash
nvidia-smi
watch -n 1 nvidia-smi
```

## Troubleshooting

### Modelo OOM

**Sintoma:** "cudaMalloc failed: out of memory"
**Causa:** Modelo muito grande para GTX 1650 4GB
**Solução:** Usar modelo menor (3-4B params)

### Timeout Cron

**Sintoma:** "cron: job execution timed out"
**Causa:** Modelo lento ou offline
**Solução:** Usar phi4-mini + fallback qwen2.5-coder:3b

### 241 Offline

**Sintoma:** "connect EHOSTUNREACH 172.23.39.241:11434"
**Causa:** Servidor offline ou Ollama parado
**Solução:** Checar SSH, reiniciar Ollama

## Referências

- Análise completa: memory/analysis-241-llm-2026-06-03.md
- Benchmark phi4-mini: memory/benchmark-phi4-mini-2026-06-03.md
- Benchmark visão: memory/benchmark-vision-models-2026-06-03.md
- Diário: memory/2026-06-03.md

---

**Data:** 2026-06-03
**Status:** Configuração completa, aguardando execução migração no 241
