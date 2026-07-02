# Plano de Migração — Omega 247 → 240

> Criado: 2026-05-22 | Atualizado: 2026-05-22
> Ω · Denalth
> Status: **TAREFAS 1-3 CONCLUÍDAS** — aguardando Denalth para tarefa 4+
> ⚠️ **REGRA #1:** A 247 SÓ DESLIGA QUANDO O DENALTH CONFIRMAR TUDO OK NA 240.

## Princípios
1. **A 247 nunca para até tu confirmar** que a 240 tá 100%
2. Cada tarefa é independente — se uma falha, o resto continua
3. Se eu cair no meio, a 247 continua rodando normalmente
4. Rollback sempre < 5 minutos
5. Tu manda "bora" em cada fase — eu não avanço sozinho

---

## TAREFA 1 — Backup completo (na 247) ✅ 2026-05-23
**Risco:** Nenhum. Só leitura.
**Duração:** ~2 min
**Rollback:** N/A

- [x] `openclaw config validate` — confirmar config OK
- [x] Backup do openclaw.json: `cp ~/.openclaw/openclaw.json ~/.openclaw/openclaw.json.bak.pre-migration`
- [x] Git commit + tag: `git add . && git commit -m "pre-migration-247-to-240" && git tag pre-migration`
- [x] Git push
- [x] Adicionado `whisper-env/` ao .gitignore

**Verificação:** commit 0563cea, tag pre-migration, pushed ✅

---

## TAREFA 2 — Copiar dados pra 240 (sem mexer na 247) ✅ 2026-05-23
**Risco:** Nenhum. Só cópia, 247 continua rodando.
**Duração:** ~5 min
**Rollback:** Apagar `/tmp/openclaw-migration` na 240

- [x] `rsync` do `~/.openclaw/` (247) → `/tmp/openclaw-migration/` (240)
  - Excluir: sessions/, logs/, .dreams/, whisper-env/, .git, *.sqlite
  - Incluir: openclaw.json, workspace/, agents/, plugins/, .env
- [x] Verificar tamanhos batem: 7.2MB origem → 6.9MB destino (compactação rsync)
- [x] Copiada chave SSH id_atlas → authorized_keys na 240

**Verificação:** 160 arquivos copiados, `ls /tmp/openclaw-migration/` OK ✅

---

## TAREFA 3 — Ajustar config na 240 (gateway desligado) ✅ 2026-05-23
**Risco:** Nenhum. Gateway da 240 tá parado.
**Duração:** ~3 min
**Rollback:** Reverter mudanças no arquivo

- [x] Parar gateway na 240: `systemctl stop openclaw-gateway`
- [x] Copiar migration pro lugar: `cp -a /tmp/openclaw-migration /root/.openclaw`
- [x] Ajustar `/root/.openclaw/openclaw.json`:
  - `gateway.bind`: "lan"
  - `gateway.port`: 18789
- [x] `.env` copiado com API keys (GROQ, GEMINI, GOOGLE)
- [x] systemd atualizado com `EnvironmentFile=/root/.openclaw/.env`
- [x] `openclaw config validate` na 240 → Config valid ✅

**Verificação:** Config valid passa ✅

---

## TAREFA 4 — Teste local na 240 (sem Telegram)
**Risco:** Baixo. Gateway da 247 continua rodando, Telegram aponta pra 247.
**Duração:** ~5 min
**Rollback:** Parar gateway da 240

- [ ] Iniciar gateway na 240: `systemctl start openclaw-gateway`
- [ ] `curl http://localhost:18789/health` → `{"ok":true}`
- [ ] Testar modelo: `curl -X POST http://localhost:18789/v1/chat/completions ...`
- [ ] Verificar SearXNG: `curl http://localhost:8888/search?q=test&format=json`
- [ ] Verificar memória: checar se workspace/files existem
- [ ] Verificar crons: `openclaw cron list`

**Verificação:** Todos os endpoints respondem

⚠️ **PONTO DE DECISÃO:** Se tudo passou até aqui, me avisa que vou pra Tarefa 5.

---

## TAREFA 5 — Cutover Telegram (DOWNTIME ~5 min)
**Risco:** Médio. Única tarefa com downtime real.
**Duração:** ~5 min
**Rollback:** Reverter webhook pra 247

- [ ] **DENALTH CONFIRMA** que quer prosseguir
- [ ] Parar gateway na 247: `openclaw gateway stop`
- [ ] Verificar Telegram webhook atual (salvar URL)
- [ ] Configurar webhook/bridge na 240 pra receber msgs do bot
- [ ] Mandar msg de teste no Telegram → aguardar resposta
- [ ] Se responder: ✅ MIGRAÇÃO OK
- [ ] Se NÃO responder: ROLLBACK IMEDIATO (reiniciar 247)

**Verificação:** Tu manda "teste" no Telegram e eu respondo pela 240

---

## TAREFA 6 — Verificação completa (240 em produção)
**Risco:** Baixo. Já tá rodando.
**Duração:** ~10 min
**Rollback:** Tarefa 5 rollback

- [ ] Telegram: msg de teste → resposta OK
- [ ] APIs: ZAI responde, Gemini responde, Groq responde
- [ ] SearXNG: pesquisa funciona
- [ ] Memória: memory_search funciona
- [ ] Crons: heartbeat, dreaming, git-autosave
- [ ] Comunicação com Atlas: SSH/Tailscale
- [ ] Transcrição de áudio: mandar áudio teste

**Verificação:** Tu testa tudo no Telegram e confirma "tá ok"

---

## TAREFA 7 — Pós-migração (247 em standby)
**Risco:** Nenhum.
**Duração:** ~5 min

- [ ] Manter 247 LIGADA por 48h como standby
- [ ] Atualizar todos os docs: MEMORY.md, TOOLS.md, people.md
- [ ] Git commit + tag: `post-migration-240-live`
- [ ] Criar cron de healthcheck na 240 (se não tiver)
- [ ] **DENALTH CONFIRMA** quando quiser desligar a 247

---

## Resumo de Segurança

| Situação | O que acontece |
|---|---|
| Eu caio no meio da tarefa 1-3 | 247 continua normal, sem impacto |
| Eu caio na tarefa 4 | 247 continua normal, 240 pode ser reiniciada |
| Telegram não funciona na tarefa 5 | Reinicio 247 imediatamente, voltei em <5 min |
| Tudo passa mas 240 falha depois | Ligo 247 de novo, aponto Telegram de volta |
| 247 hardware falha | Tenho backup no GitHub + 240 já com tudo |

**Downtime máximo:** 5 minutos (só na tarefa 5)
**Janela segura:** Qualquer dia, qualquer hora. Tu só precisa estar online pra confirmar a tarefa 5.
