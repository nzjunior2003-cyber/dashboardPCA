# Deploy na VPS KVM2

## Pré-requisitos

- SSH acesso à VPS
- Nginx ou Apache instalado
- Git instalado
- Domínio ou IP da VPS

---

## Opção 1: Stack Mínimo (Recomendado)

### 1. Conectar na VPS
```bash
ssh root@seu_ip_vps
```

### 2. Atualizar sistema
```bash
apt update && apt upgrade -y
```

### 3. Instalar Nginx
```bash
apt install nginx -y
systemctl start nginx
systemctl enable nginx
```

### 4. Clonar repositório
```bash
cd /var/www
git clone https://github.com/nzjunior2003-cyber/dashboardPCA.git
cd dashboardPCA
```

### 5. Configurar Nginx

Crie o arquivo `/etc/nginx/sites-available/dashboard`:

```nginx
server {
    listen 80;
    server_name seu_dominio.com www.seu_dominio.com;
    
    # Redirecionar HTTP para HTTPS (opcional)
    # return 301 https://$server_name$request_uri;
    
    root /var/www/dashboardPCA;
    index index.html;
    
    # Cache de arquivos estáticos
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot)$ {
        expires 30d;
        add_header Cache-Control "public, immutable";
    }
    
    # Servir arquivos estáticos
    location / {
        try_files $uri $uri/ /index.html;
    }
    
    # Bloquear acesso a arquivos sensíveis
    location ~ /\. {
        deny all;
    }
}
```

### 6. Habilitar site
```bash
ln -s /etc/nginx/sites-available/dashboard /etc/nginx/sites-enabled/
nginx -t
systemctl restart nginx
```

### 7. Acessar
- HTTP: `http://seu_dominio.com`
- HTTPS: Veja seção SSL abaixo

---

## Opção 2: Com SSL/HTTPS (Let's Encrypt)

### 1. Instalar Certbot
```bash
apt install certbot python3-certbot-nginx -y
```

### 2. Obter certificado
```bash
certbot --nginx -d seu_dominio.com -d www.seu_dominio.com
```

### 3. Auto-renew
```bash
systemctl enable certbot.timer
```

---

## Opção 3: Usando Node.js + PM2 (Para SPA/SSR)

Se precisar de processamento backend:

### 1. Instalar Node.js
```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
apt install nodejs -y
npm install -g pm2
```

### 2. Instalar dependências
```bash
cd /var/www/dashboardPCA
npm install
```

### 3. Criar servidor Express (opcional)
Crie `server.js` na raiz do projeto (se precisar de backend).

### 4. Iniciar com PM2
```bash
pm2 start npm --name dashboard -- start
pm2 startup
pm2 save
```

---

## Opção 4: Usando Docker (Mais Fácil)

### 1. Instalar Docker
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

### 2. Criar Dockerfile
```dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
EXPOSE 80
```

### 3. Build e Run
```bash
cd /var/www/dashboardPCA
docker build -t dashboard-pca .
docker run -d -p 80:80 --name dashboard-pca dashboard-pca
```

---

## Configurar Google Sheets API (Importante)

### ✅ Opção Recomendada: Compartilhamento Público

1. Abra sua Google Sheet
2. Clique em **Compartilhar**
3. Selecione **"Qualquer pessoa com link"** ou **Público**
4. **NÃO altere** o `SHEET_ID` em `index.html`

### ⚠️ CORS Issue
Se aparecer erro de CORS, use um proxy:

Edite `index.html` e altere:
```javascript
// De:
const CSV_URL = `https://docs.google.com/spreadsheets/d/${SHEET_ID}/export?format=csv`;

// Para (usando proxy):
const CSV_URL = `https://cors-anywhere.herokuapp.com/https://docs.google.com/spreadsheets/d/${SHEET_ID}/export?format=csv`;
```

Ou configure seu próprio proxy CORS no servidor.

---

## Monitoramento & Manutenção

### Ver logs do Nginx
```bash
tail -f /var/log/nginx/access.log
tail -f /var/log/nginx/error.log
```

### Atualizar código
```bash
cd /var/www/dashboardPCA
git pull origin main
```

### Testes de performance
```bash
# Verificar tempo de acesso
curl -w "Tempo: %{time_total}s\n" http://seu_dominio.com

# Usando Apache Bench
apt install apache2-utils
ab -n 100 -c 10 http://seu_dominio.com/
```

---

## Segurança Recomendada

### 1. Firewall (UFW)
```bash
ufw allow 22
ufw allow 80
ufw allow 443
ufw enable
```

### 2. Proteção DDoS
```bash
# Limite de conexões simultâneas no Nginx
limit_conn_zone $binary_remote_addr zone=addr:10m;
limit_conn addr 10;
```

### 3. Headers de Segurança (adicione ao Nginx)
```nginx
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-Content-Type-Options "nosniff" always;
add_header X-XSS-Protection "1; mode=block" always;
add_header Referrer-Policy "no-referrer-when-downgrade" always;
```

---

## Backup Automático

### Cron job para backup diário
```bash
crontab -e
```

Adicione:
```bash
# Backup diário às 2 da manhã
0 2 * * * cd /var/www/dashboardPCA && git pull && tar -czf ~/backups/dashboard-$(date +\%Y\%m\%d).tar.gz .
```

---

## Troubleshooting

| Problema | Solução |
|----------|---------|
| Nginx retorna 404 | Verifique `root` path correto em nginx.conf |
| CORS error | Configure proxy CORS ou compartilhe Google Sheet publicamente |
| Dados não carregam | Verifique se Google Sheet está pública |
| Pagina carrega lento | Abilite cache, minify CSS/JS, use CDN |
| SSL error | Execute `certbot renew --dry-run` |

---

## Verificação Final

```bash
# 1. Testar Nginx
nginx -t

# 2. Verificar status
systemctl status nginx

# 3. Acessar dashboard
curl http://seu_dominio.com

# 4. Ver processo
ps aux | grep nginx
```

---

## GitHub Actions (CI/CD Automático - Opcional)

Crie `.github/workflows/deploy.yml`:

```yaml
name: Deploy to VPS

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
          host: ${{ secrets.VPS_HOST }}
          username: ${{ secrets.VPS_USER }}
          key: ${{ secrets.VPS_SSH_KEY }}
          script: |
            cd /var/www/dashboardPCA
            git pull origin main
            systemctl restart nginx
```

---

## Suporte

Para dúvidas:
- Verifique QUICKSTART.md
- Veja DEVELOP.md para customizações
- Leia README.md para funcionalidades

**Dashboard pronto para produção!** 🚀
