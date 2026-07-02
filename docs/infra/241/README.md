# .241 - Sentinel (Gemma 4 Local)

> Servidor de LLM local + Dokploy
> Ω · Denalth

## Hardware
- **CPU:** i7-4770
- **RAM:** 16GB
- **GPU:** GTX 1650 (4GB)
- **Storage:** RAID1 2×1TB
- **Papel:** Sentinel — LLM local + Dokploy

## Portas e Serviços

| Porta | Serviço | Descrição | Status |
|---|---|---|---|
| 11434 | Ollama | LLM API local | ✅ Ativo |
| 3000 | Dokploy | Plataforma de deploy | ✅ Ativo |
| 8001 | RedisInsight | UI Redis | ✅ Ativo |
| 8088 | HTTP Server Python | Arquivo antigo | ❌ Removido |
| 8089 | Filebrowser | Gestão de arquivos | ✅ Ativo |
| 9000 | Portainer | Gestão Docker | ✅ Ativo |

## Modelos Ollama

### Principais (Usar)
- **phi4-mini:** 41.0 tok/s (chat geral)
- **qwen2.5-coder:3b:** 49.5 tok/s (código rápido)
- **qwen3:4b:** 32.7 tok/s (chat geral)
- **gemma4:e4b:** 13.6 tok/s (raciocínio+visão)
- **nomic-embed-text:** Embeddings locais (768 dims)

### Localização
- **Modelos:** `/home/ceops/data/ollama-models/` (RAID1)
- **Propriedade:** ollama:ollama

## Creds
- **SSH:** `sshpass -p '@t3nd1m3nT0' ssh ceops@172.23.39.241`
- **Senha:** `@t3nd1m3nT0` (2006-06-18)

## Documentação
- `limpeza-ferramentas-antigas.md` — Histórico de limpeza
- `comando-limpeza-manual.md` — Scripts de manutenção

## Scripts
- `scripts/sentinel/limpeza.sh` — Limpeza automatizada

## Manutenção
- [ ] Disk encryption
- [ ] Updates pendentes
- [ ] Chaves SSH dedicadas
