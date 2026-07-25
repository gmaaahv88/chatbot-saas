# 🚀 Chatbot SaaS com IA (Claude) + WhatsApp + Stripe

![Node.js](https://img.shields.io/badge/Node.js-v20-green)
![Express](https://img.shields.io/badge/Express.js-Framework-blue)
![SQLite](https://img.shields.io/badge/SQLite-Database-lightgrey)
![Claude AI](https://img.shields.io/badge/Claude%20AI-Anthropic-purple)
![Stripe](https://img.shields.io/badge/Stripe-Payment-blue)
![License](https://img.shields.io/badge/License-MIT-yellow)

> Sistema completo e pronto para produção de **Chatbot com Inteligência Artificial** para automação de atendimento via WhatsApp. Integrado com **Stripe** para sistema de pagamento recorrente. Ideal para clínicas, salões, academias e qualquer negócio que precisa atender clientes 24/7.

---

## ✨ Features Principais

- ✅ **Inteligência Artificial** - Powered by Claude (Anthropic)
- ✅ **WhatsApp 24/7** - Integrado com Twilio
- ✅ **Multi-Tenant** - Múltiplas clínicas/empresas isoladas
- ✅ **Multi-Segmento** - Funciona para qualquer tipo de negócio
- ✅ **Sistema de Pagamento** - Stripe integrado (R$ 399/mês)
- ✅ **Dashboard Admin** - Painel completo com 4 abas
- ✅ **CRUD Procedimentos** - Gestão total de serviços/procedimentos
- ✅ **Banco de Dados SQLite** - Profissional e portável
- ✅ **Autenticação Segura** - Session + Hash SHA256
- ✅ **Resposta Automática** - IA gera respostas contextualizadas

---

## 📊 Screenshots & Demo

```
🔐 Login Admin
  ↓
📊 Dashboard (4 abas)
  ├─ Dados da Empresa
  ├─ Meus Procedimentos (CRUD)
  ├─ Agendamentos
  └─ WhatsApp Números
  ↓
💳 Checkout (Stripe)
  ↓
✅ Assinatura Ativa (30 dias)
  ↓
🤖 Chatbot Respondendo 24/7
```

---

## 🏗️ Arquitetura

```
┌─────────────────────────────────────────────┐
│          CLIENTE (Paciente/WhatsApp)        │
└──────────────────┬──────────────────────────┘
                   │
                   ↓
        ┌──────────────────────┐
        │  Twilio (WhatsApp)   │
        └──────────┬───────────┘
                   │
                   ↓
    ┌─────────────────────────────┐
    │     Server Node.js          │
    │  - Express.js               │
    │  - Session/Auth             │
    │  - Multi-tenant logic       │
    └──────────┬──────────────────┘
               │
         ┌─────┴─────────────────┐
         │                       │
         ↓                       ↓
    ┌────────────┐        ┌──────────────┐
    │ Claude API │        │  SQLite DB   │
    │   (IA)     │        │  (Local)     │
    └────────────┘        └──────────────┘
         │                       │
         └─────────┬─────────────┘
                   │
                   ↓
    ┌──────────────────────────┐
    │   Stripe (Pagamento)     │
    └──────────────────────────┘
                   │
                   ↓
    ┌──────────────────────────┐
    │  Resposta ao paciente    │
    │  via WhatsApp 24/7       │
    └──────────────────────────┘
```

---

## 🛠️ Stack Técnica

| Camada | Tecnologia | Versão |
|--------|-----------|--------|
| **Runtime** | Node.js | v20+ |
| **Framework** | Express.js | 4.x |
| **IA/LLM** | Claude (Anthropic) | claude-haiku-4-5 |
| **WhatsApp** | Twilio | v3.x |
| **Pagamento** | Stripe | v11+ |
| **Banco** | SQLite3 | 3.x |
| **Autenticação** | express-session | 1.x |
| **Segurança** | crypto (SHA256) | built-in |

---

## 📦 Instalação

### **Pré-requisitos**

- **Node.js** v20 ou superior
- **npm** 9+
- **SQLite3**
- Chaves API para:
  - Anthropic (Claude)
  - Twilio
  - Stripe

### **Setup Local (5 minutos)**

```bash
# 1. Clonar o repositório
git clone https://github.com/gmaaahv88/chatbot-saas.git
cd chatbot-saas

# 2. Instalar dependências
npm install

# 3. Criar arquivo .env (copie do .env.example)
cp .env.example .env

# 4. Editar .env com suas chaves
# ANTHROPIC_API_KEY=sk-ant-...
# TWILIO_ACCOUNT_SID=...
# STRIPE_SECRET_KEY=...

# 5. Rodar servidor
npm start
```

**Servidor rodará em:** `http://localhost:3000`

### **Deploy (DigitalOcean/Ubuntu)**

```bash
# Conectar ao servidor
ssh root@seu-ip

# Clonar repo
cd /root
git clone https://github.com/gmaaahv88/chatbot-saas.git
cd chatbot-saas

# Instalar dependências
npm install

# Configurar .env
nano .env
# Cole suas chaves

# Rodar com PM2
npm install -g pm2
pm2 start server.js --name chatbot-clinica
pm2 save
pm2 startup
```

---

## 🌐 Endpoints Principais

| URL | Descrição |
|-----|-----------|
| `/login.html` | 🔐 Login Admin |
| `/registrar` | 📝 Registrar nova clínica |
| `/dashboard.html` | 📊 Painel administrativo |
| `/checkout.html` | 💳 Página de pagamento Stripe |
| `/health` | 🏥 Health check API |
| `/api/auth/login` | POST - Autenticar |
| `/api/admin/procedimentos` | CRUD - Gerenciar procedimentos |
| `/whatsapp` | POST - Webhook Twilio |
| `/chat` | POST - Chat IA |

---

## 📚 Documentação Completa

- **[INSTALAÇÃO](./docs/INSTALACAO.md)** - Setup detalhado
- **[API COMPLETA](./docs/API.md)** - Endpoints e payloads
- **[DEPLOY](./docs/DEPLOY.md)** - Como colocar em produção
- **[ARQUITETURA](./docs/ARQUITETURA.md)** - Detalhes técnicos
- **[FAQ](./docs/FAQ.md)** - Perguntas frequentes

---

## 🚀 Como Usar

### **Para Pacientes (Via WhatsApp)**

1. Mandar mensagem para o número da clínica
2. Bot responde automaticamente
3. Conversam naturalmente
4. Podem agendar procedimentos
5. Tudo 24/7 sem atendente

**Exemplo de conversa:**
```
Paciente: "Quanto custa limpeza de pele?"
Bot: "Limpeza de pele custa R$ 150 e dura 60 min. Quer agendar?"
Paciente: "Sim, segunda às 19h"
Bot: "Agendado para segunda às 19:00. Confirma?"
```

### **Para Admin da Clínica**

1. Acessa: `http://seu-dominio.com/registrar`
2. Registra dados da clínica
3. Paga R$ 399/mês via Stripe
4. Acessa dashboard com 4 abas:
   - **Dados da Empresa** - Editar informações
   - **Meus Procedimentos** - Adicionar/editar serviços
   - **Agendamentos** - Ver histórico
   - **WhatsApp** - Gerenciar números

5. Bot começa a responder automaticamente!

---

## 💰 Modelo de Negócio

```
Preço: R$ 399/mês por clínica
Stripe taxa: R$ 14,50/cliente
Seu lucro: R$ 384,50/cliente

10 clientes   = R$ 3.845/mês
50 clientes   = R$ 19.225/mês
100 clientes  = R$ 38.450/mês
```

---

## 🧪 Testes

### **Dados de Teste (Stripe)**

```
Cartão: 4242 4242 4242 4242
Expiração: 12/25
CVC: 123
Resultado: Pagamento bem-sucedido ✅
```

### **Credenciais Teste**

```
Email: admin@test.com
Senha: senha123
```

---

## 🔒 Segurança

- ✅ Senhas armazenadas com hash SHA256
- ✅ Session segura com express-session
- ✅ Isolamento de dados multi-tenant
- ✅ Variáveis de ambiente (.env)
- ✅ Validação de entrada em todas as APIs
- ✅ Proteção contra SQL Injection

---

## 📊 Estrutura de Pastas

```
chatbot-saas/
├── server.js                 # Arquivo principal
├── chatbot.db               # Banco SQLite
├── package.json             # Dependências
├── .env.example             # Template de variáveis
├── .gitignore               # Arquivos ignorados
├── LICENSE                  # MIT License
│
├── config/
│   ├── env.js              # Carrega variáveis
│   └── twilio.js           # Cliente Twilio
│
├── routes/
│   ├── auth.js             # Login/Registro
│   ├── admin.js            # Dashboard
│   ├── api.js              # APIs
│   └── whatsapp.js         # Webhook WhatsApp
│
├── public/
│   ├── login.html          # Login
│   ├── registro.html       # Registro
│   ├── dashboard.html      # Painel admin
│   ├── checkout.html       # Stripe checkout
│   └── css/, js/           # Assets
│
└── docs/
    ├── INSTALACAO.md
    ├── API.md
    ├── DEPLOY.md
    └── FAQ.md
```

---

## 🎯 Funcionalidades por Versão

### **v1.0 (Atual) ✅**
- ✅ Chatbot com Claude IA
- ✅ WhatsApp integrado
- ✅ Multi-tenant completo
- ✅ Dashboard admin
- ✅ CRUD procedimentos
- ✅ Stripe integrado
- ✅ Autenticação

### **v1.1 (Próxima)**
- ⏳ Email automático
- ⏳ Domínio próprio
- ⏳ HTTPS/SSL
- ⏳ Relatórios avançados

### **v2.0 (Futuro)**
- ⏳ App mobile (iOS/Android)
- ⏳ API pública
- ⏳ Múltiplos idiomas
- ⏳ Integração Google Calendar
- ⏳ Planos diferenciados

---

## 📈 Roadmap

```
2026 Q3:  ✅ MVP produção
2026 Q4:  ⏳ Múltiplos planos de preço
2027 Q1:  ⏳ App mobile
2027 Q2:  ⏳ Marketplace templates
2027 Q3:  ⏳ Enterprise features
```

---

## 🤝 Como Contribuir

Contribuições são bem-vindas!

1. **Faça um Fork** do projeto
2. **Crie uma Branch** (`git checkout -b feature/MinhaFeature`)
3. **Commit** suas mudanças (`git commit -m 'Adiciona MinhaFeature'`)
4. **Push** para a Branch (`git push origin feature/MinhaFeature`)
5. **Abra um Pull Request**

---

## 📝 Licença

Este projeto está sob a licença **MIT**.

Veja [LICENSE](./LICENSE) para detalhes.

---

## 👤 Sobre o Autor

**Marcela Vieira**  
Full Stack Developer | Especialista em Chatbots & SaaS

- 📧 Email: g.maaah.v@gmail.com
- 🐙 GitHub: [@gmaaahv88](https://github.com/gmaaahv88)
- 💼 LinkedIn: [seu-profile]

---

## 🙏 Agradecimentos

- [Anthropic](https://anthropic.com) - Claude AI
- [Twilio](https://twilio.com) - WhatsApp API
- [Stripe](https://stripe.com) - Payment Processing
- [Express.js](https://expressjs.com) - Web Framework

---

## 📞 Suporte

### **Dúvidas ou Problemas?**

1. Verifique o [FAQ](./docs/FAQ.md)
2. Abra uma [Issue](https://github.com/gmaaahv88/chatbot-saas/issues)
3. Envie email: g.maaah.v@gmail.com

---

## ⭐ Se Curtiu, Deixa uma Star!

```
⭐ Isso ajuda muitas pessoas a descobrir o projeto!
```

---

**Desenvolvido com ❤️ em 2026**  
**Pronto para produção | Open Source | MIT License**

[⬆ Voltar ao Topo](#-chatbot-saas-com-ia-claude--whatsapp--stripe)
