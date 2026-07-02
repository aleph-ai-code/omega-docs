# Infra SSH — Documentação de Acesso

> Ω · Denalth
> Última atualização: 2026-07-01

## Visão Geral

Todos os servidores CEOPS estão configurados com **SSH compartilhado** usando a chave do Omega. Isso permite que qualquer servidor possa se conectar em qualquer outro servidor sem senha.

---

## Servidores Configurados

| Hostname | IP | Papel | Chave SSH | Status |
|---|---|---|---|---|
| ceopssrv240 | 172.23.39.240 | Omega | id_ed25519 | ✅ Origem |
| ceopssrv241 | 172.23.39.241 | Sentinel | id_ed25519 | ✅ Configurado |
| ceopssrv249 | 172.23.39.249 | Gateway | id_ed25519 | ✅ Configurado |
| ceopssrv250 | 172.23.39.250 | Aleph | id_ed25519 | ✅ Configurado |
| ceopssrv251 | 172.23.39.251 | Voyager | id_ed25519 | ✅ Configurado |

---

## Chave SSH Compartilhada

### Chave Pública

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPpVW2Mkp1jElsKYWoO+mYju076YmwWUfSID5wjmb8LJ root@ceopssrv240
```

### Chave Privada

**Localização:** `/home/ceops/.ssh/id_ed25519` (em todos os servidores)

**Permissões:** 600 (-rw-------)

**Fingerprint:** SHA256:lumxZw0QqILrRgY6ZxYAV9s4g5I0sUaWrLQNDlUYI64

---

## Como Usar

### Conexão Básica

```bash
# De qualquer servidor para qualquer outro:
ssh ceops@172.23.39.<XXX>

# Exemplos:
ssh ceops@172.23.39.250  # .240 → .250
ssh ceops@172.23.39.241  # .250 → .241
ssh ceops@172.23.39.240  # .251 → .240
```

### Com SSH Config

Se você tiver o `~/.ssh/config` configurado:

```bash
# Adicione ao ~/.ssh/config:
Host ceops240
    HostName 172.23.39.240
    User ceops
    IdentityFile ~/.ssh/id_ed25519

Host ceops241
    HostName 172.23.39.241
    User ceops
    IdentityFile ~/.ssh/id_ed25519

Host ceops250
    HostName 172.23.39.250
    User ceops
    IdentityFile ~/.ssh/id_ed25519

Host ceops251
    HostName 172.23.39.251
    User ceops
    IdentityFile ~/.ssh/id_ed25519

# Use de forma simplificada:
ssh ceops240
ssh ceops241
ssh ceops250
ssh ceops251
```

---

## Estrutura de Arquivos

### Em Cada Servidor

```
/home/ceops/.ssh/
├── id_ed25519           # Chave privada do Omega (600)
├── id_ed25519.pub       # Chave pública do Omega (644)
├── authorized_keys      # Contém a chave pública (600)
├── config              # Configuração SSH (600)
└── known_hosts         # Hosts conhecidos (644)
```

### Permissões Corretas

```bash
# Diretório .ssh:
chmod 700 ~/.ssh

# Chave privada:
chmod 600 ~/.ssh/id_ed25519

# Chave pública:
chmod 644 ~/.ssh/id_ed25519.pub

# authorized_keys:
chmod 600 ~/.ssh/authorized_keys

# config:
chmod 600 ~/.ssh/config
```

---

## Exemplo de Config Completa

### ~/.ssh/config

```bash
# Configuração para servidores CEOPS
Host ceops240
    HostName 172.23.39.240
    User ceops
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
    StrictHostKeyChecking no

Host ceops241
    HostName 172.23.39.241
    User ceops
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
    StrictHostKeyChecking no

Host ceops250
    HostName 172.23.39.250
    User ceops
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
    StrictHostKeyChecking no

Host ceops251
    HostName 172.23.39.251
    User ceops
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
    StrictHostKeyChecking no

# Pattern para todos os servidores CEOPS
Host 172.23.39.*
    User ceops
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
    StrictHostKeyChecking no
```

---

## Troubleshooting

### "Too many authentication failures"

**Causa:** SSH está tentando múltiplas chaves antes da correta.

**Solução:** Adicione `IdentitiesOnly yes` ao `~/.ssh/config`:

```bash
Host *
    IdentitiesOnly yes
```

### "Host identification changed"

**Causa:** `known_hosts` tem chave antiga do host.

**Solução:** Limpar a entrada do host:

```bash
ssh-keygen -R <IP> -f ~/.ssh/known_hosts

# Exemplo:
ssh-keygen -R 172.23.39.250 -f ~/.ssh/known_hosts
```

### "Permission denied (publickey)"

**Causa:** Chave privada não está no servidor ou permissões incorretas.

**Solução:**

1. Verificar se a chave privada existe:
```bash
ls -la ~/.ssh/id_ed25519
```

2. Verificar permissões:
```bash
chmod 600 ~/.ssh/id_ed25519
```

3. Verificar se a chave pública está no `authorized_keys` do servidor de destino:
```bash
cat ~/.ssh/authorized_keys
```

---

## Segurança

### ⚠️ Aviso Importante

**Todos os servidores compartilham a mesma chave privada.**

**Implicações:**
- Se um servidor for comprometido, todos estão vulneráveis
- Sem auditoria de qual servidor fez a conexão
- Chave única = ponto único de falha

### Para Rede Interna Confiável (CEOPS)

- ✅ Aceitável para servidores em rede confiável
- ✅ Facilita automação e scripts
- ✅ Sem necessidade de senhas

### Para Produção Externa

- ❌ Não recomendado
- ⚠️ Considere chaves separadas por servidor
- ⚠️ Implemente autenticação de dois fatores

---

## Procedimento de Configuração

### Como Foi Configurado (2026-07-01)

1. **Gerar chave no .240 (Omega):**
```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519 -C "root@ceopssrv240"
```

2. **Distribuir chave privada para todos os servidores:**
```bash
# Copiar chave privada:
scp ~/.ssh/id_ed25519 ceops@<IP>:/home/ceops/.ssh/
ssh ceops@<IP> "chmod 600 /home/ceops/.ssh/id_ed25519"
```

3. **Distribuir chave pública para authorized_keys:**
```bash
# Copiar chave pública:
cat ~/.ssh/id_ed25519.pub | ssh ceops@<IP> "cat > /home/ceops/.ssh/authorized_keys"
ssh ceops@<IP> "chmod 600 /home/ceops/.ssh/authorized_keys"
```

4. **Configurar SSH config:**
```bash
# Criar config em cada servidor
cat > ~/.ssh/config << 'EOF'
Host 172.23.39.*
    User ceops
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
    StrictHostKeyChecking no
EOF
chmod 600 ~/.ssh/config
```

5. **Testar conexões bidirecionais:**
```bash
# De .240 para .250
ssh ceops@172.23.39.250 "hostname && whoami"

# De .250 para .240
ssh ceops@172.23.39.250 "ssh ceops@172.23.39.240 'hostname && whoami'"
```

---

## Referências

- Data de configuração: 2026-07-01
- Responsável: Omega (Ω)
- Documentação relacionada: `memory/2026-07-01.md`
- Status consolidado: `memory/2026-06-30-to-2026-07-01-consolidated.md`

---

_Ω · Denalth - 2026-07-01_
