# Troubleshooting - OpenSSH Windows

> Quando o serviço sshd não é encontrado após instalação
> Ω · Denalth
> 2026-07-02

## Problema: "Não é possível localizar qualquer serviço com o nome de serviço 'sshd'"

### Solução 1: Verificar nome correto do serviço

No PowerShell (ADMIN):

```powershell
# Listar todos os serviços SSH
Get-Service | Where-Object {$_.Name -like "*ssh*"}

# Ou verificar especificamente
Get-Service sshd -ErrorAction SilentlyContinue
Get-Service ssh-server -ErrorAction SilentlyContinue
Get-Service openssh -ErrorAction SilentlyContinue
```

### Solução 2: Verificar se OpenSSH foi realmente instalado

```powershell
# Listar capabilities instaladas
Get-WindowsCapability -Online | Where-Object {$_.Name -like "*OpenSSH*"}

# Deve mostrar algo como:
# Name                : OpenSSH.Server~~~~0.0.1.0
# State               : Installed
```

### Solução 3: Instalação alternativa via DISM

Se Add-WindowsCapability falhou:

```powershell
# Usar DISM
DISM /Online /Add-Capability /CapabilityName:OpenSSH.Server~~~~0.0.1.0

# Ou via Feature on Demand (FoD)
DISM /Online /Enable-Feature /FeatureName:OpenSSH-Server /All
```

### Solução 4: Instalação manual do OpenSSH

Se métodos acima não funcionarem:

```powershell
# Baixar OpenSSH do GitHub (última versão)
# https://github.com/PowerShell/Win32-OpenSSH/releases

# Ou usar winget
winget install --id Microsoft.OpenSSH.Server

# Ou usar chocolatey
choco install openssh
```

### Solução 5: Verificar arquivos de instalação

```powershell
# Verificar se os binários existem
Test-Path "C:\Windows\System32\OpenSSH\sshd.exe"
Test-Path "C:\Windows\System32\OpenSSH\ssh.exe"

# Se existirem, tentar registrar o serviço manualmente
cd "C:\Windows\System32\OpenSSH"
.\sshd.exe -install
```

## Após encontrar o serviço correto

```powershell
# Substitua pelo nome correto encontrado
Start-Service <NOME_DO_SERVICO>
Set-Service -Name <NOME_DO_SERVICO> -StartupType 'Automatic'
```
