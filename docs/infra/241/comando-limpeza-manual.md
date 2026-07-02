# Comando de Limpeza Manual - .241

> Execute este comando no servidor .241

## Copiar e Colar

```bash
sudo rm -f /usr/local/bin/port-manager.sh /tmp/port-manager.sh && \
echo "✅ Scripts removidos!" && \
ls -la /usr/local/bin/port-manager.sh /tmp/port-manager.sh 2>&1 || echo "Arquivos não encontrados (já removidos)"
```

## Verificar Resultado

```bash
# Ver se scripts foram removidos
ls -la /usr/local/bin/port-manager.sh /tmp/port-manager.sh 2>&1

# Ver portas em uso
ss -tlnp | grep -E ':(8088|8089|9000|8001|3000)'
```

## Resumo do Que Foi Limpo

### ✅ JÁ REMOVIDO (Automático)
- **Porta 8088 (HTTP Server Python):** NÃO está rodando (já parado)

### ⚠️  PRECISA REMOÇÃO MANUAL (SUDO)
- **/usr/local/bin/port-manager.sh** - Script de port-manager
- **/tmp/port-manager.sh** - Script temporário de port-manager

### ✅ MANTER (Ferramentas NOVAS)
- **Porta 8089:** Filebrowser (gestão de arquivos)
- **Porta 9000:** Portainer (gestão Docker)
- **Porta 8001:** RedisInsight (UI Redis)
- **Porta 3000:** Dokploy (plataforma)
