![Node.js](https://img.shields.io/badge/Node.js-v20-green)
![Express](https://img.shields.io/badge/Express.js-Framework-blue)
![SQLite](https://img.shields.io/badge/SQLite-Database-lightgrey)
![Stripe](https://img.shields.io/badge/Stripe-Payment-blue)
![License](https://img.shields.io/badge/License-MIT-yellow)

# 🚀 Chatbot SaaS com IA - Claude + WhatsApp + Stripe

Um sistema completo e pronto para produção de Chatbot com inteligência artificial para automação de atendimento via WhatsApp.

## ✨ Features

- ✅ **Inteligência Artificial** - Powered by Claude (Anthropic)
- ✅ **WhatsApp 24/7** - Integrado com Twilio
- ✅ **Multi-Tenant** - Múltiplas clínicas/empresas isoladas
- ✅ **Multi-Segmento** - Funciona para qualquer negócio
- ✅ **Sistema de Pagamento** - Stripe integrado (R$ 399/mês)
- ✅ **Painel Admin** - Dashboard completo com 4 abas
- ✅ **CRUD Procedimentos** - Gestão total de serviços
- ✅ **Banco de Dados** - SQLite profissional

## 🏗️ Arquitetura

Frontend (HTML5/CSS3/JS) ↓ Backend (Node.js/Express) ↓ AI Engine (Claude API) ↓ WhatsApp (Twilio) ↓ Payment (Stripe) ↓ Database (SQLite)

## 🛠️ Stack Técnica

| Camada | Tecnologia |
|--------|-----------|
| **Runtime** | Node.js v20 |
| **Framework** | Express.js |
| **IA** | Anthropic Claude API |
| **WhatsApp** | Twilio |
| **Pagamento** | Stripe |
| **Banco** | SQLite3 |
| **Autenticação** | Session + Hash |

## 📦 Instalação

### **Requisitos:**
- Node.js v20+
- npm
- SQLite3
- Chaves API: Anthropic, Twilio, Stripe

### **Setup Local:**

```bash
# 1. Clonar repositório
git clone https://github.com/seu-username/chatbot-saas.git
cd chatbot-saas

# 2. Instalar dependências
npm install

# 3. Configurar variáveis de ambiente
cp .env.example .env
# Edite .env com suas chaves API

# 4. Rodar
npm start
```

Servidor rodará em: `http://localhost:3000`

## 📚 Documentação

- **[Relatório Final](./docs/RELATORIO_FINAL.md)** - Arquitetura completa
- **[Guia de Vendas](./docs/GUIA_VENDAS.md)** - Scripts e estratégia
- **[APIs](./docs/APIs.md)** - Documentação técnica
- **[Deploy](./docs/DEPLOY.md)** - Como fazer deploy

## 🌐 URLs Principais

| Página | URL |
|--------|-----|
| Login | `/login.html` |
| Registro | `/registrar` |
| Dashboard | `/dashboard.html` |
| Checkout | `/checkout.html` |
| API Health | `/health` |

## 💡 Funcionalidades Principais

### **Para Pacientes (End Users)**
- Enviam mensagem no WhatsApp
- Recebem resposta automática do Chatbot em segundos
- Podem agendar, perguntar sobre preços, horários
- Tudo 24/7 sem esperar atendente

### **Para Admin da Clínica**
- Dashboard com 4 abas:
  - **Dados da Empresa** - Editar informações
  - **Meus Procedimentos** - CRUD completo
  - **Agendamentos** - Visualizar agendamentos
  - **WhatsApp** - Gerenciar números
- Sistema de pagamento integrado
- Relatórios de agendamentos

### **Para Você (Proprietário do SaaS)**
- Painel de admin (futura feature)
- Relatórios de clientes e receita
- Webhook de pagamentos Stripe
- Multi-tenant completo

## 💰 Modelo de Negócio

| Item | Valor |
|------|-------|
| **Preço por Clínica** | R$ 399/mês |
| **Taxa Stripe** | R$ 14,50/cliente |
| **Lucro Líquido** | R$ 384,50/cliente |
| **10 Clientes** | R$ 3.845/mês |
| **50 Clientes** | R$ 19.225/mês |

## 🧪 Testes

### **Dados de Teste (Stripe)**

Cartão válido: Número: 4242 4242 4242 4242 Expiração: 12/25 CVC: 123

Credenciais teste: Email: admin@test.com Senha: senha123

## 📊 Métricas Esperadas

Taxa de Conversão: 50% (registro → pagamento) Retenção: 90% (clientes mantêm assinatura) CAC: R$ 0 (você mesmo faz abordagem) LTV: R$ 2.394 (6 meses)

## 🚀 Roadmap

### **Curto Prazo (Próxima Sprint)**
- [ ] Adicionar domínio próprio
- [ ] HTTPS/SSL certificado
- [ ] Email de confirmação
- [ ] Dashboard com métricas

### **Médio Prazo (3 meses)**
- [ ] Planos diferenciados (Básico/Pro/Enterprise)
- [ ] Integração Google Calendar
- [ ] API pública
- [ ] App mobile

### **Longo Prazo (6+ meses)**
- [ ] IA mais avançada
- [ ] Múltiplos idiomas
- [ ] Marketplace de templates
- [ ] CRM integrado

## 📖 Como Usar como Freelancer

Este projeto é perfeito para:

1. **Entregar para clientes**
   - Customize para sua clínica/negócio
   - Implante em servidor próprio
   - Valor: R$ 12.000-20.000

2. **Usar como SaaS**
   - Hospede uma versão
   - Venda acesso para múltiplos clientes
   - Valor: R$ 399/mês por cliente

3. **Usar como Case Study**
   - Mostrar portfólio
   - Conseguir mais clientes
   - Comprovar expertise

## 🔒 Segurança

- ✅ Senhas com hash SHA256
- ✅ Session segura com express-session
- ✅ Isolamento de dados multi-tenant
- ✅ Variáveis de ambiente (.env)
- ✅ Validação de entrada

## 📝 Licença

MIT License - Use livremente!

## 👤 Autor

Desenvolvedor Full Stack  
Especialista em Chatbots e SaaS  
[LinkedIn](https://linkedin.com)  
[Email](mailto:seu-email@gmail.com)

## 📞 Contato

- Email: seu-email@gmail.com
- WhatsApp: (XX) XXXXX-XXXX
- LinkedIn: [seu-profile]
- Portfolio: [seu-site]

## 🤝 Contribuições

Contribuições são bem-vindas! Abra uma issue ou faça um pull request.

---

**Desenvolvido com ❤️ em 2026**
