# 🚀 DEPLOY EM PRODUÇÃO - Chatbot SaaS

Guia passo-a-passo para colocar seu Chatbot em produção.

---

## 📋 Índice

1. [DigitalOcean (Recomendado)](#digitalocean-recomendado)
2. [AWS](#aws)
3. [Heroku](#heroku)
4. [Configurar Domínio](#configurar-domínio)
5. [HTTPS/SSL](#httpssl)
6. [Monitoramento](#monitoramento)

---

## 🔷 DigitalOcean (Recomendado)

### **Por que DigitalOcean?**

- ✅ Simples e barato (R$ 20-30/mês)
- ✅ Suporta Node.js nativo
- ✅ Bom para SaaS
- ✅ Suporte em português

### **Passo 1: Criar Droplet**

1. Acesse: https://digitalocean.com
2. Clique "Create" → "Droplets"
3. Escolha:
   - **Image:** Ubuntu 24.04
   - **Size:** R$ 30/mês (2GB RAM, 50GB SSD)
   - **Region:** São Paulo (SaoP)
   - **Hostname:** chatbot-saas

4. Clique "Create Droplet"

### **Passo 2: Conectar via SSH**

```bash
# Você receberá email com senha temporária
# Primeira conexão

ssh root@seu-novo-ip
# Cole a senha

# Alterar senha
passwd
# Digite nova senha 2x
```

### **Passo 3: Preparar Servidor**

```bash
# Update
apt update && apt upgrade -y

# Instalar Node.js v20
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
apt install -y nodejs

# Instalar Git
apt install -y git

# Instalar Nginx
apt install -y nginx

# Instalar PM2 global
npm install -g pm2
```

### **Passo 4: Clonar Projeto**

```bash
# Na pasta root
cd /root

# Clonar repo
git clone https://github.com/gmaaahv88/chatbot-saas.git
cd chatbot-saas

# Instalar dependências
npm install
```

### **Passo 5: Configurar .env**

```bash
# Copiar template
cp .env.example .env

# Editar com suas chaves
nano .env
```

Cole:
```env
ANTHROPIC_API_KEY=sk-ant-XXXXX
TWILIO_ACCOUNT_SID=ACXXXX
TWILIO_AUTH_TOKEN=XXXXX
TWILIO_WHATSAPP_FROM=whatsapp:+14155238886
STRIPE_PUBLISHABLE_KEY=pk_test_XXXXX
STRIPE_SECRET_KEY=sk_test_XXXXX
PORT=3000
```

Salve: `Ctrl+X`, `Y`, `Enter`

### **Passo 6: Rodar com PM2**

```bash
# Iniciar
pm2 start server.js --name chatbot-clinica

# Salvar
pm2 save
pm2 startup

# Verificar
pm2 logs chatbot-clinica
```

Deve aparecer:
```
✅ Chatbot SaaS com Stripe rodando na porta 3000
```

### **Passo 7: Configurar Nginx (Proxy)**

```bash
# Editar config
nano /etc/nginx/sites-available/default
```

Substitua o conteúdo por:

```nginx
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    server_name _;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Salve e restart:

```bash
nginx -t
systemctl restart nginx
```

### **Passo 8: Testar**

```bash
# No seu PC, acesse
http://seu-ip-do-servidor:80
```

Deve aparecer a página de login! ✅

---

## 🟠 AWS

### **Opção 1: EC2**

```bash
# Lançar instância EC2
# Image: Ubuntu 24.04 LTS
# Type: t2.micro (grátis 1 ano)
# Security Group: adicionar 80, 443, 22

# Conectar (via SSH)
ssh -i sua-chave.pem ubuntu@seu-ec2-ip

# Resto é igual ao DigitalOcean
sudo apt update
sudo apt install nodejs npm git nginx
# ... (resto dos passos)
```

### **Opção 2: Elastic Beanstalk**

```bash
# Mais simples (gerenciado)
# Suporta Node.js nativo

# Instalar CLI
pip install awsebcli

# Criar app
eb init -p "Node.js 20" chatbot-saas
eb create chatbot-saas-env
eb deploy
```

---

## 💜 Heroku

### **Passo 1: Preparar**

```bash
# Instalar Heroku CLI
# https://devcenter.heroku.com/articles/heroku-cli

# Login
heroku login

# Criar app
heroku create seu-chatbot-saas
```

### **Passo 2: Adicionar Variáveis**

```bash
heroku config:set ANTHROPIC_API_KEY=sk-ant-XXXXX
heroku config:set TWILIO_ACCOUNT_SID=ACXXXX
heroku config:set TWILIO_AUTH_TOKEN=XXXXX
heroku config:set STRIPE_SECRET_KEY=sk_test_XXXXX
```

### **Passo 3: Deploy**

```bash
# Na pasta do projeto
git push heroku main
```

**URL:** `https://seu-chatbot-saas.herokuapp.com`

### **⚠️ Limitações Heroku**

- Heroku desativa apps inativos
- SQLite não é ideal (use PostgreSQL)
- Mais caro para produção

---

## 🌐 Configurar Domínio

### **Passo 1: Comprar Domínio**

Opções:
- Namecheap: https://namecheap.com
- RegistroBR: https://registrobr.org.br
- GoDaddy: https://godaddy.com

Exemplo: `seudominio.com.br` ≈ R$ 50/ano

### **Passo 2: Apontar Domínio**

Na configuração do domínio, adicione DNS record:

```
Type: A
Name: @
Value: seu-ip-do-servidor
TTL: 3600
```

Ou aponte para seu servidor cloud (Heroku/AWS)

### **Passo 3: Verificar**

```bash
# Aguarde 24-48h para propagar
nslookup seudominio.com.br

# Acesse no navegador
http://seudominio.com.br
```

---

## 🔒 HTTPS/SSL

### **Usar Let's Encrypt (Grátis)**

```bash
# Instalar Certbot
apt install -y certbot python3-certbot-nginx

# Obter certificado
certbot --nginx -d seudominio.com.br

# Escolha "Redirect HTTP to HTTPS" (2)
```

Nginx será configurado automaticamente!

### **Renovação Automática**

```bash
# Verificar
certbot renew --dry-run

# Já está agendado (rodas daily)
```

---

## 📊 Monitoramento

### **PM2 Monitoring**

```bash
# Ativar monitoramento
pm2 web
```

Acesse: `http://seu-ip:9615`

### **Logs**

```bash
# Ver logs em tempo real
pm2 logs chatbot-clinica

# Salvar logs em arquivo
pm2 logs chatbot-clinica > /root/logs.txt
```

### **Alerts (Opcional)**

```bash
# Guardar email de alertas
pm2 set pm2:email seu-email@gmail.com

# Configurar webhooks
pm2 web
```

---

## 🔧 Manutenção

### **Atualizar Código**

```bash
cd /root/chatbot-saas

# Puxar atualizações
git pull origin main

# Se houver mudanças em package.json
npm install

# Reiniciar
pm2 restart chatbot-clinica
```

### **Backup do Banco**

```bash
# Copiar banco para local seguro
cp /root/chatbot-saas/chatbot.db /root/backups/chatbot-$(date +%Y-%m-%d).db

# Backup automático (cron)
0 0 * * * cp /root/chatbot-saas/chatbot.db /root/backups/chatbot-$(date +\%Y-\%m-\%d).db
```

### **Limpeza de Logs**

```bash
# PM2 logs crescem muito
pm2 logs --reset

# Ou configurar rotação
apt install -y logrotate
```

---

## 📈 Escalabilidade

### **Quando você tiver +100 clientes:**

1. **Banco maior**
   - Migrar SQLite → PostgreSQL
   - Usar RDS (AWS) ou DigitalOcean Managed DB

2. **Múltiplos servidores**
   - Load balancer (Nginx)
   - 2-3 instâncias Node.js

3. **CDN**
   - CloudFront (AWS)
   - Cloudflare (grátis)

4. **Cache**
   - Redis
   - Memcached

---

## 🎯 Checklist de Deploy

- [ ] Servidor criado (DigitalOcean/AWS)
- [ ] Node.js instalado
- [ ] Código clonado
- [ ] .env configurado com chaves reais
- [ ] PM2 rodando
- [ ] Nginx configurado
- [ ] Domínio apontado
- [ ] HTTPS ativado
- [ ] Páginas carregando
- [ ] Login funcionando
- [ ] Stripe testado
- [ ] WhatsApp webhook configurado
- [ ] Backups agendados

---

## 🆘 Troubleshooting

### **Página branca (500 erro)**

```bash
# Ver logs
pm2 logs chatbot-clinica

# Verificar arquivo .env
cat /root/chatbot-saas/.env
```

### **WhatsApp não responde**

```bash
# Verificar webhook Twilio
# Dashboard Twilio → Whatsapp → Webhook

# URL deve ser: https://seudominio.com/whatsapp
# (HTTPS, não HTTP!)
```

### **Stripe retorna erro**

```bash
# Verificar chaves .env
# Certificados SSL instalados
certbot renew
```

---

## 📞 Suporte

- DigitalOcean: https://digitalocean.com/support
- AWS: https://aws.amazon.com/support
- Let's Encrypt: https://letsencrypt.org/community/

---

[⬆ Voltar](#-deploy-em-produção---chatbot-saas)
