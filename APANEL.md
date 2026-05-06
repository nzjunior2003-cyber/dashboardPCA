# Deploy na aPanel (Hostinger)

## Comando SSH na aPanel

### Conectar na VPS Hostinger
```bash
ssh seu_usuario@seu_dominio.com
# ou
ssh seu_usuario@seu_ip_vps
```

**Senha:** Use a mesma da aPanel

---

## Deploy Rápido (Copie & Cola)

### 1. Conectar
```bash
ssh seu_usuario@seu_dominio.com
```

### 2. Ir para pasta pública
```bash
cd ~/public_html
```
**Ou se houver subdomínio:**
```bash
cd ~/public_html/seu_subdominio
```

### 3. Remover arquivos antigos (se houver)
```bash
rm -rf *
```

### 4. Clonar repositório
```bash
git clone https://github.com/nzjunior2003-cyber/dashboardPCA.git .
```

### 5. Pronto! ✅
Seu dashboard está em: `http://seu_dominio.com`

---

## Setup mais Completo

Se quiser com cache e compressão:

```bash
# 1. Conectar
ssh seu_usuario@seu_dominio.com

# 2. Ir para pasta pública
cd ~/public_html

# 3. Garantir que está vazio
rm -rf *

# 4. Clonar repositório
git clone https://github.com/nzjunior2003-cyber/dashboardPCA.git .

# 5. Criar arquivo .htaccess para compressão e cache
cat > .htaccess << 'EOF'
# Ativar gzip
<IfModule mod_deflate.c>
  AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css text/javascript application/javascript
</IfModule>

# Cache de arquivos estáticos
<IfModule mod_expires.c>
  ExpiresActive On
  ExpiresByType text/html "access plus 1 hour"
  ExpiresByType text/css "access plus 1 year"
  ExpiresByType text/javascript "access plus 1 year"
  ExpiresByType application/javascript "access plus 1 year"
  ExpiresByType image/* "access plus 1 year"
  ExpiresByType font/* "access plus 1 year"
</IfModule>

# Rewrite para SPA
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule ^ index.html [QSA,L]
</IfModule>

# Segurança
<FilesMatch "\.git">
  Deny from all
</FilesMatch>
EOF

# 6. Definir permissões corretas
chmod 755 ~/public_html
chmod 644 ~/public_html/*.html
chmod 644 ~/public_html/*.js
chmod 644 ~/public_html/*.css

# 7. Pronto!
echo "✅ Dashboard em: http://seu_dominio.com"
```

---

## Atualizar código (Pull de mudanças)

```bash
cd ~/public_html
git pull origin main
```

---

## SSL/HTTPS (Let's Encrypt - Automático na aPanel)

1. Abra **aPanel** → Seu domínio
2. Clique em **SSL Certificate**
3. Clique em **Issue SSL Certificate**
4. Pronto! ✅ (automático)

Ou via SSH:
```bash
# Se cert.bot estiver instalado
certbot certonly --webroot -w ~/public_html -d seu_dominio.com
```

---

## Ver pasta pública na aPanel

```bash
# Listar arquivos
ls -la ~/public_html

# Ver espaço em disco
du -sh ~/public_html
```

---

## Estrutura de Pastas Hostinger

```
~/
├── public_html/          ← Seu website aqui
│   ├── index.html
│   ├── README.md
│   ├── config.json
│   └── ... (outros arquivos)
├── private_html/         ← Acesso privado
├── mail/                 ← E-mail
└── ...
```

---

## Troubleshooting

| Problema | Solução |
|----------|---------|
| Permissão negada | `chmod -R 755 ~/public_html` |
| Git não encontrado | Instalar: `apt install git` ou contato suporte |
| Página branca/erro | Verificar `.htaccess` e `index.html` |
| Dados não carregam | Verifique se Google Sheet está pública |

---

## Github Actions (Deploy Automático - Opcional)

Se quiser fazer deploy automático quando fazer `git push`:

Crie `.github/workflows/deploy.yml`:

```yaml
name: Deploy to Hostinger

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Deploy via SSH
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.HOST }}
          username: ${{ secrets.USERNAME }}
          password: ${{ secrets.PASSWORD }}
          script: |
            cd ~/public_html
            git pull origin main
```

**Para usar:**
1. Vá ao repositório GitHub → Settings → Secrets
2. Adicione:
   - `HOST`: seu_dominio.com
   - `USERNAME`: seu_usuario
   - `PASSWORD`: sua_senha

---

## Checklist Final

- [ ] SSH conectado na aPanel
- [ ] Repositório clonado em `~/public_html`
- [ ] Arquivo .htaccess criado
- [ ] Permissões corretas (755)
- [ ] Acesso HTTP funcionando
- [ ] SSL automático ativado
- [ ] Google Sheet compartilhada publicamente

**Dashboard pronto! 🚀**
