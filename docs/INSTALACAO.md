# 📦 GUIA DE INSTALAÇÃO - Chatbot SaaS

Instruções completas para instalar e rodar o Chatbot SaaS em diferentes ambientes.

---

## 📋 Índice

1. [Windows Local](#windows-local)
2. [Linux/Ubuntu (DigitalOcean)](#linuxubuntu-digitalocean)
3. [Configurar Variáveis de Ambiente](#configurar-variáveis-de-ambiente)
4. [Verificar Instalação](#verificar-instalação)
5. [Troubleshooting](#troubleshooting)

---

## 🪟 Windows Local

### **Pré-requisitos**

- Windows 10/11
- Node.js v20+ → https://nodejs.org/
- Git → https://git-scm.com/
- Chaves API prontas (ver seção abaixo)

### **Passo 1: Clonar Repositório**

```powershell
# Abrir PowerShell ou CMD

# Escolher pasta
cd C:\app

# Clonar
git clone https://github.com/gmaaahv88/chatbot-saas.git
cd chatbot-saas
```

### **Passo 2: Instalar Dependências**

```powershell
npm install
```

Espera terminar (leva 1-2 minutos).

### **Passo 3: Configurar .env**

```powershell
# Copiar template
copy .env.example .env

# Abrir com editor (Notepad ou VS Code)
notepad .env
```

Edite com suas chaves (ver seção abaixo).

### **Passo 4: Rodar Servidor**

```powershell
npm start
```

**Acessar em:** `http://localhost:3000`

```
✅ Chatbot SaaS com Stripe rodando na porta 3000
🔐 Login: http://localhost:3000/login.html
💳 Checkout: http://localhost:3000/checkout.html
📊 Dashboard: http://localhost:3000/dashboard.html
```

---

## 🐧 Linux/Ubuntu (DigitalOcean)

### **Pré-requisitos**

- Ubuntu 20.04 ou superior
- root ou sudo access
- Chaves API prontas

### **Passo 1: Conectar ao Servidor**

```bash
# SSH do seu PC
ssh root@SEU_IP_DO_SERVIDOR
```

### **Passo 2: Instalar Node.js**

```bash
# Update sistema
apt update && apt upgrade -y

# Instalar Node.js v20
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
apt install -y nodejs

# Verificar
node --version
npm --version
```

### **Passo 3: Clonar Repositório**

```bash
cd /root
git clone https://github.com/gmaaahv88/chatbot-saas.git
cd chatbot-saas
```

### **Passo 4: Instalar Dependências**

```bash
npm install
```

### **Passo 5: Configurar .env**

```bash
# Copiar template
cp .env.example .env

# Editar (use nano ou vi)
nano .env
```

Cole suas chaves, depois:
- Pressione `Ctrl+X`
- Digite `Y`
- Pressione `Enter`

### **Passo 6: Instalar PM2 (Process Manager)**

```bash
npm install -g pm2
```

### **Passo 7: Rodar com PM2**

```bash
# Iniciar
pm2 start server.js --name chatbot-clinica

# Salvar para restart automático
pm2 save
pm2 startup

# Ver status
pm2 logs chatbot-clinica
```

### **Passo 8: Configurar Nginx (Opcional, para domínio)**

```bash
apt install -y nginx

# Editar config
nano /etc/nginx/sites-enabled/default
```

Adicione:

```nginx
server {
    listen 80;
    server_name seu-dominio.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
    }
}
```

Restart:

```bash
systemctl restart nginx
```

---

## 🔑 Configurar Variáveis de Ambiente

### **Arquivo `.env.example`:**

```env
# === ANTHROPIC (Claude AI) ===
ANTHROPIC_API_KEY=sk-ant-v0-XXXXX

# === TWILIO (WhatsApp) ===
TWILIO_ACCOUNT_SID=ACXXXX
TWILIO_AUTH_TOKEN=XXXXXX
TWILIO_WHATSAPP_FROM=whatsapp:+14155238886

# === STRIPE (Pagamento) ===
STRIPE_PUBLISHABLE_KEY=pk_test_XXXXX
STRIPE_SECRET_KEY=sk_test_XXXXX

# === SERVIDOR ===
PORT=3000
```

### **Como Obter Cada Chave:**

#### **1. Anthropic API Key**

```
1. Acesse: https://console.anthropic.com/
2. Login com sua conta
3. Dashboard → API Keys
4. Copie a chave
5. Cole em ANTHROPIC_API_KEY
```

#### **2. Twilio (WhatsApp)**

```
1. Acesse: https://www.twilio.com/
2. Login/Signup
3. Console → Auth Token (copie)
4. Account SID (copie)
5. WhatsApp Sandbox (setup)
6. Cole em TWILIO_*
```

#### **3. Stripe**

```
1. Acesse: https://stripe.com/
2. Dashboard → API Keys
3. Copie Publishable Key (pk_test_)
4. Copie Secret Key (sk_test_)
5. Cole em STRIPE_*
```

---

## ✅ Verificar Instalação

### **Teste Local (Windows)**

```powershell
# Terminal 1 - rodar servidor
cd C:\app\chatbot-saas
npm start

# Terminal 2 - testar API
curl http://localhost:3000/health
```

Deve aparecer:
```json
{"status":"ok"}
```

### **Teste Servidor (Linux)**

```bash
# Testar API
curl http://localhost:3000/health

# Ver logs
pm2 logs chatbot-clinica

# Listar processos
pm2 list
```

---

## 🚀 Rodar em Background

### **Windows (PowerShell)**

```powershell
# Rodar em background
Start-Process powershell -ArgumentList "cd C:\app\chatbot-saas; npm start" -NoNewWindow
```

### **Linux (PM2)**

```bash
# Já está configurado com PM2
pm2 start server.js --name chatbot-clinica

# Ver status
pm2 status

# Parar
pm2 stop chatbot-clinica

# Reiniciar
pm2 restart chatbot-clinica

# Ver logs
pm2 logs chatbot-clinica --lines 100
```

---

## 📊 Estrutura de Pastas Após Install

```
chatbot-saas/
├── node_modules/              # Dependências (criada após npm install)
├── public/                     # Páginas HTML
│   ├── login.html
│   ├── dashboard.html
│   ├── checkout.html
│   └── css/, js/
├── config/                     # Configurações
├── server.js                   # Arquivo principal
├── chatbot.db                  # Banco de dados SQLite
├── package.json                # Dependências
├── .env                        # Variáveis (não commitar!)
├── .env.example               # Template (compartilhar)
├── .gitignore                 # Arquivos ignorados
└── docs/                       # Documentação
```

---

## 🔄 Atualizar Código

Quando houver atualizações:

```bash
# Puxar atualizações
git pull origin main

# Reinstalar dependências (se necessário)
npm install

# Reiniciar servidor
npm start        # local
pm2 restart chatbot-clinica  # linux
```

---

## 🛠️ Troubleshooting

### **Erro: "Cannot find module 'express'"**

```bash
# Solução: reinstalar dependências
npm install
```

### **Porta 3000 já está em uso**

```bash
# Opção 1: usar outra porta
PORT=3001 npm start

# Opção 2: matar processo
# Windows:
netstat -ano | findstr :3000
taskkill /PID XXXXX /F

# Linux:
lsof -i :3000
kill -9 PID
```

### **Erro de SQLite**

```bash
# Reinstalar sqlite3
npm rebuild sqlite3
```

### **Erro de Autenticação (npm install)**

```bash
# Limpar cache
npm cache clean --force

# Tentar novamente
npm install
```

### **Servidor roda mas páginas retornam 404**

```bash
# Verificar se pasta public/ existe
ls -la public/

# Verificar se arquivos .html estão lá
ls -la public/*.html
```

---

## 📞 Suporte

Se tiver dúvida:

1. Verifique os logs:
   ```bash
   npm start  # vê mensagens de erro em tempo real
   ```

2. Abra uma issue no GitHub

3. Envie email: g.maaah.v@gmail.com

---

[⬆ Voltar](#-guia-de-instalação---chatbot-saas)
