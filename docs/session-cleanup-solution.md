# Session Cleanup — Solução para Orphan Files ♾️

> Ω · Denalth
> Data: 2026-07-12

## Problema

**"18 orphan transcript files"** — arquivos de sessões que não estão mais no store mas ocupam espaço em disco.

### Sintomas
- Muitos arquivos `.jsonl`, `.trajectory.jsonl`, `.trajectory-path.json` no diretório
- Poucas sessões ativas no `sessions.json`
- Crescimento contínuo do diretório `~/.openclaw/agents/main/sessions/`

### Causa Raiz

O comando `openclaw sessions cleanup`:
- ✅ Remove sessões do **store** (`sessions.json`)
- ❌ **NÃO remove** os **arquivos físicos** associados

## Solução

### Script Atualizado

Arquivo: `/root/.openclaw/workspace/scripts/session-cleanup.sh`

**O que faz:**
1. Executa `openclaw sessions cleanup` (limpa o store)
2. Identifica sessionIds no store (incluindo `usageFamilySessionIds`)
3. Compara com arquivos físicos no diretório
4. Remove arquivos órfãos (que não estão no store)
5. Remove arquivos `.deleted.*` antigos (> 7 dias)
6. Remove checkpoints antigos (> 7 dias)

### Deploy

**Servidores:**
- ✅ ceopssrv240 (.240) - Omega
- ✅ ceopssrv241 (.241) - Sentinel
- ✅ ceopssrv249 (.249) - SME
- ✅ ceopssrv250 (.250) - Aleph

**Timer systemd:**
- Nome: `session-cleanup.timer`
- Agenda: **Bissemanal** (domingo + quarta 03:00)
- Serviço: `session-cleanup.service`
- Próxima execução: Quarta 15/07 03:00

## Resultados

### Cleanup Manual (2026-07-12)

| Servidor | Arquivos Antes | Arquivos Depois | Removidos | Espaço Antes | Espaço Depois | Recuperado |
|---|---|---|---|---|---|---|
| .240 | 158 | 22 | 136 | 110MB | 31MB | 79MB |
| .241 | 36 | 14 | 22 | 16MB | 1.2MB | 14.8MB |
| .249 | 361 | 58 | 303 | 104MB | 13MB | 91MB |
| .250 | 273 | 59 | 214 | 129MB | 7MB | 122MB |
| **Total** | **828** | **153** | **675** | **359MB** | **52.2MB** | **306.8MB** |

### Orphans Removidos

- .240: 103 sessões órfãs
- .241: 14 sessões órfãs
- .249: 245 sessões órfãs
- .250: 155 sessões órfãs
- **Total: 517 sessões órfãs removidas**

## Manutenção Contínua

### Timer Semanal

**Agenda:** Bissemanal - Domingo + Quarta 03:00 (antes do dreaming)

**Verificar:**
```bash
# Status do timer
systemctl status session-cleanup.timer

# Logs da última execução
journalctl -u session-cleanup.service --since "1 day ago"
```

### Monitoramento

**No heartbeat:**
- Tamanho do diretório de sessões (< 50MB ideal)
- Alertar se > 100MB

**Verificar manualmente:**
```bash
# Contar arquivos
find ~/.openclaw/agents/main/sessions -type f ! -name "*.deleted.*" | wc -l

# Ver tamanho
du -sh ~/.openclaw/agents/main/sessions/

# Comparar com sessões ativas
openclaw sessions list | grep "Sessions listed"
```

## Script Completo

Ver o código em: `/root/.openclaw/workspace/scripts/session-cleanup.sh`

**Componentes principais:**
- `openclaw sessions cleanup` — limpeza do store
- Lógica de orphans — compara store com filesystem
- Cleanup de arquivos `.deleted.*` — remove arquivos marcados para deleção
- Cleanup de checkpoints — remove checkpoints antigos

## Lições Aprendidas

1. **Orphan accumulation:** Sessões removidas do store mas arquivos físicos persistem
2. **Escalabilidade:** O problema acontece em todos os servidores OpenClaw
3. **Retenção:** Arquivos `.deleted.*` precisam de cleanup agressivo (7 dias)
4. **Monitoramento:** O diretório pode crescer silenciosamente sem monitoramento

## Referências

- Diário: `memory/2026-07-12.md`
- Script: `/root/.openclaw/workspace/scripts/session-cleanup.sh`
- Timer: `/etc/systemd/system/session-cleanup.{timer,service}`
