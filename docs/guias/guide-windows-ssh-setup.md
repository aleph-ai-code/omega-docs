# Como Configurar SSH Key Authentication no Windows

> Guia para configurar acesso SSH sem senha nos servidores Windows
> Ω · Denalth
> 2026-07-02

## Passo 1: Instalar OpenSSH Server no Windows

### Via PowerShell (Administrador):

```powershell
# Verificar se já está instalado
Get-WindowsCapability -Online -Name OpenSSH.Server~~~~

# Instalar (se necessário)
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

### Via GUI:
1. **Configurações** → **Aplicativos** → **Aplicativos e recursos** → **Recursos opcionais**
2. Buscar por **OpenSSH Server**
3. Instalar

---

## Passo 2: Configurar o Serviço SSH

### Iniciar e habilitar serviço (PowerShell como ADMIN):

```powershell
# Iniciar serviço
Start-Service sshd

# Habilitar início automático
Set-Service -Name sshd -StartupType 'Automatic'

# Confirmar que está rodando
Get-Service sshd
```

### Firewall (se necessário):

```powershell
# Confirmar regra de firewall existe
Get-NetFirewallRule -Name *ssh*

# Se não existir, criar:
New-NetFirewallRule -Name 'SSH Server' -DisplayName 'SSH Server' -Enabled True -Direction Inbound -Protocol TCP -LocalPort 22 -Action Allow
```

---

## Passo 3: Configurar Autenticação por Chave

### 3.1 No Linux (Omega), gerar chave se não existir:

```bash
# Já temos ~/.ssh/id_ed25519
# Verificar:
ls -la ~/.ssh/
```

### 3.2 Copiar chave pública para o Windows:

```bash
# Mostrar chave pública
cat ~/.ssh/id_ed25519.pub
```

**Copie essa chave!** Vai parecer algo como:
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEXAMPLE= root@ceopssrv240
```

### 3.3 No Windows, criar authorized_keys:

```powershell
# Criar diretório .ssh (se necessário)
New-Item -Path "C:\Users\SEU_USUARIO\.ssh" -ItemType Directory -Force

# Criar arquivo authorized_keys
# Substitua SEU_USUARIO pelo seu usuário Windows
# Cole a chave pública do Linux dentro das aspas abaixo
$sshKey = "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEXAMPLE= root@ceopssrv240"
Set-Content -Path "C:\Users\SEU_USUARIO\.ssh\authorized_keys" -Value $sshKey

# Ajustar permissões (IMPORTANTE!)
icacls "C:\Users\SEU_USUARIO\.ssh" /inheritance:r
icacls "C:\Users\SEU_USUARIO\.ssh" /grant:r "SEU_USUARIO:F"
icacls "C:\Users\SEU_USUARIO\.ssh\authorized_keys" /inheritance:r
icacls "C:\Users\SEU_USUARIO\.ssh\authorized_keys" /grant:r "SEU_USUARIO:F"
```

### 3.4 Configurar sshd para usar authorized_keys:

```powershell
# Editar config do sshd
notepad C:\ProgramData\ssh\sshd_config
```

**Descomentar/ajustar estas linhas:**

```
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
```

**Reiniciar serviço:**

```powershell
Restart-Service sshd
```

---

## Passo 4: Testar Acesso

### No Linux (Omega), testar conexão:

```bash
# Substitua SEU_USUARIO pelo usuário Windows
ssh -i ~/.ssh/id_ed25519 -o StrictHostKeyChecking=no SEU_USUARIO@172.23.39.10

# E para o Atlas
ssh -i ~/.ssh/id_ed25519 -o StrictHostKeyChecking=no SEU_USUARIO@172.23.39.39
```

**Se entrar sem pedir senha, está OK!** ✅

---

## Resumo - O Que Você Precisa Fazer

1. ✅ **Instalar OpenSSH Server** (PowerShell como ADMIN)
2. ✅ **Iniciar serviço sshd** e habilitar auto-start
3. ✅ **Copiar chave pública** do Omega (vou te mostrar)
4. ✅ **Criar authorized_keys** no Windows com essa chave
5. ✅ **Ajustar permissões** do arquivo .ssh
6. ✅ **Testar conexão**

---

## Chave Pública do Omega

Vou mostrar a chave pública para copiar:

```bash
cat ~/.ssh/id_ed25519.pub
```
