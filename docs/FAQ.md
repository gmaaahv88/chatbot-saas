# ❓ FAQ - Perguntas Frequentes

Respostas para as dúvidas mais comuns sobre o Chatbot SaaS.

---

## 📋 Índice

1. [Geral](#geral)
2. [Instalação & Setup](#instalação--setup)
3. [Funcionamento](#funcionamento)
4. [Pagamento](#pagamento)
5. [WhatsApp](#whatsapp)
6. [Desenvolvimento](#desenvolvimento)

---

## 🎯 Geral

### **O que é Chatbot SaaS?**

É um sistema completo de chatbot com IA que você pode:

1. **Usar como SaaS** - Vender acesso para clínicas (R$ 399/mês)
2. **Vender como freelancer** - Implementar customizado (R$ 12.000+)
3. **Usar como base** - Hackear e criar seu próprio produto

---

### **Quanto custa rodar?**

```
Servidor: R$ 20-30/mês (DigitalOcean)
APIs: Grátis (limite de testes)
Domínio: R$ 50/ano
Total: ≈ R$ 100/mês
```

---

### **Posso vender isso?**

✅ **SIM!** É MIT License - use comercialmente!

```
Você pode:
✅ Vender como SaaS
✅ Vender customizações
✅ Vender como serviço
✅ Usar como base para seu produto
```

---

### **Qual o diferencial vs Manychat/ManyChat?**

```
ManyChat/Typebot/Chatfuel:
❌ Caros (R$ 1000+/mês)
❌ Genéricos (não customizáveis)
❌ Sem IA real
❌ Sem payment integrado

Nosso Chatbot:
✅ Barato (R$ 399/mês)
✅ Totalmente customizável
✅ IA real (Claude)
✅ Stripe integrado
✅ Multi-tenant
```

---

## 🔧 Instalação & Setup

### **Quanto tempo para instalar?**

```
Windows local: 5 minutos
Linux/Production: 20 minutos
Com domínio + HTTPS: 1 hora
```

---

### **Preciso de conhecimentos técnicos?**

✅ **Para usar:** Não precisa de nada!
⚠️ **Para instalar:** Conhecimento básico de terminal
🚀 **Para customizar:** Conhecimento de Node.js/JavaScript

---

### **Dá para rodar no Windows?**

✅ **SIM!** 

```
npm start  (na pasta do projeto)
http://localhost:3000
```

Funciona 100%!

---

### **Posso rodar sem Stripe/Twilio?**

✅ **SIM!** Mas você perderá:

```
Sem Stripe:
- Sem pagamento
- Sem sistema de assinatura

Sem Twilio:
- Sem WhatsApp
- Sem chatbot automático

Sem Claude:
- Sem IA real
- Respostas hardcoded
```

---

### **SQLite é bom para produção?**

✅ **Sim, até 100+ clientes**

```
SQLite é ótimo para:
✅ MVP (Minimum Viable Product)
✅ Até 1M+ registros
✅ Sem servidor externo
✅ Backup fácil

Limitações:
❌ Sem replicação
❌ Sem backup automático
❌ Sem escalabilidade horizontal
```

Depois migre para PostgreSQL se crescer.

---

## 🤖 Funcionamento

### **Como o bot funciona?**

```
1. Paciente manda mensagem
2. Twilio recebe
3. Servidor busca dados da clínica
4. Cria prompt dinâmico
5. Envia para Claude IA
6. Claude processa
7. Retorna resposta contextualizada
8. Bot envia de volta
9. Paciente recebe ✅
```

---

### **O bot pode se conectar a meu sistema?**

✅ **SIM!** Com customização:

```
Exemplo:
- Buscar agendamentos em seu CRM
- Verificar disponibilidade de horários
- Criar agendamentos automáticos
- Integrar com seu sistema de faturamento

Requer: Conhecimento de APIs
```

---

### **Como o bot aprende?**

O bot **não aprende** (não há machine learning).

Ele usa:
- Prompt dinâmico com dados da clínica
- Histórico da conversa (últimas 10 mensagens)
- Inteligência pré-treinada do Claude

---

### **Posso usar outro LLM (GPT, Gemini)?**

✅ **SIM!** É só mudar a integração:

```javascript
// Atual (Claude)
const response = await anthropic.messages.create({ ... })

// OpenAI (GPT)
const response = await openai.chat.completions.create({ ... })

// Google (Gemini)
const response = await gemini.generateContent({ ... })
```

---

### **O chat funciona 24/7?**

✅ **SIM!** Completamente automático.

```
Se seu servidor:
✅ Roda 24/7
✅ Tem internet estável
✅ APIs externas OK

Então bot responde:
✅ A qualquer hora
✅ Sem atendente
✅ Indefinidamente
```

---

## 💳 Pagamento

### **Como Stripe funciona?**

```
1. Clínica registra
   └─ Sistema cria Customer no Stripe

2. Clínica vai para checkout
   └─ Vê formulário de cartão

3. Clínica paga R$ 399
   └─ Stripe processa
   └─ Tira do cartão dela

4. Você recebe na conta
   └─ Stripe menos 3,69% + R$ 0,50

5. Subscription dura 30 dias
   └─ Depois clínica precisa renovar
```

---

### **Preciso de conta comercial no Stripe?**

✅ **Não** para testes.

```
Versão teste:
✅ Usar em desenvolvimento
✅ Testar com cartão 4242 4242...
❌ Não processa dinheiro real

Produção:
⚠️ Stripe Connect (recomendado)
   - Clínica paga para você
   - Você fica com 80-90%
   - Stripe fica com 10-20%

OU:

⚠️ Seu próprio Stripe
   - Clínica paga para você
   - Você fica com 96,31%
   - Stripe fica com 3,69%
```

---

### **E se a clínica não pagar?**

```
Status da subscription:

✅ 'active'   → Tudo OK, acesso liberado
⏳ 'pending'  → Aguardando pagamento
❌ 'expired'  → Expirado, redireciona para checkout
```

Sistema automaticamente bloqueia acesso se expirar.

---

## 📱 WhatsApp

### **Como integrar meu número de WhatsApp?**

```
Passo 1: Ambiente Sandbox (teste)
- Usar número de teste do Twilio
- Pronto na instalação

Passo 2: Número de Negócio (produção)
- Comprar número no Twilio
- Ativar WhatsApp Business
- Aguardar aprovação Meta
- Apontar webhook

Custo: ≈ R$ 200/mês no Twilio
```

---

### **Posso ter múltiplos números?**

✅ **SIM!** Multi-números integrado:

```
Clínica 1:
└─ Número A
└─ Número B
└─ Número C (opcional)

Clínica 2:
└─ Número D
└─ Número E

Sistema roteia automaticamente!
```

---

### **O chat pode enviar mídia (imagens, PDF)?**

Atualmente: ❌ Não

Implementação futura:
```
POST /whatsapp
Media: Image, Document, Audio

Bot pode responder com:
- Imagens
- PDFs
- Áudio
- Vídeos
```

---

### **WhatsApp pode bloquear o bot?**

⚠️ **Possível se:**

```
❌ Spam frequente
❌ Muitas mensagens automáticas
❌ Padrão suspeito

✅ Melhor prática:
✅ Esperar resposta do usuário
✅ Não bombardear mensagens
✅ Usar a API oficial (Twilio)
✅ Respeitar limites de taxa
```

---

## 💻 Desenvolvimento

### **Posso customizar o código?**

✅ **SIM!** É open source (MIT License)

```
Você pode:
✅ Mudar cores
✅ Adicionar campos
✅ Criar novas páginas
✅ Integrar APIs
✅ Vender como seu produto
```

---

### **Como criar um novo procedimento?**

Dashboard → Meus Procedimentos → "+ Novo"

Ou via API:

```bash
curl -X POST http://localhost:3000/api/admin/procedimentos-novo \
  -H "Content-Type: application/json" \
  -d '{
    "nome": "Limpeza de Pele",
    "preco": 150,
    "duracao_minutos": 60,
    "descricao": "Limpeza profunda com extração"
  }'
```

---

### **Como resetar o banco de dados?**

```bash
# Deletar banco
rm chatbot.db

# Reiniciar servidor
npm start

# Sistema cria banco novo (vazio)
```

---

### **Como backupear dados?**

```bash
# Copiar arquivo
cp chatbot.db chatbot-backup-$(date +%Y-%m-%d).db

# Ou em Linux
cp chatbot.db /backup/
```

SQLite é um arquivo único - muito fácil fazer backup!

---

### **Posso rodar em Docker?**

✅ **SIM!** Com Dockerfile:

```dockerfile
FROM node:20

WORKDIR /app

COPY . .

RUN npm install

EXPOSE 3000

CMD ["npm", "start"]
```

Depois:

```bash
docker build -t chatbot-saas .
docker run -p 3000:3000 chatbot-saas
```

---

### **Como fazer deploy automático (CI/CD)?**

```
GitHub Actions:

1. Código novo no main
2. Testes automáticos
3. Deploy automático
4. Servidor atualizado

Template disponível em breve!
```

---

## 🐛 Troubleshooting

### **"Port 3000 already in use"**

```bash
# Windows
netstat -ano | findstr :3000
taskkill /PID XXXXX /F

# Linux
lsof -i :3000
kill -9 PID
```

---

### **"Cannot find module 'express'"**

```bash
npm install
```

---

### **WhatsApp não responde**

```
Verificar:
✅ Webhook configurado no Twilio
✅ URL é HTTPS (não HTTP)
✅ Número está mapeado no banco
✅ Clínica tem subscription ativa
✅ Claude API key é válida
```

---

### **Stripe retorna erro de autenticação**

```bash
# Verificar chaves .env
cat .env

# Deve ter:
STRIPE_PUBLISHABLE_KEY=pk_test_...
STRIPE_SECRET_KEY=sk_test_...

# Se não, gerar novas no dashboard Stripe
```

---

## 📞 Precisa de Mais Ajuda?

1. **Leia a documentação:**
   - [INSTALACAO.md](./INSTALACAO.md)
   - [API.md](./API.md)
   - [DEPLOY.md](./DEPLOY.md)

2. **Abra uma issue no GitHub:**
   - https://github.com/gmaaahv88/chatbot-saas/issues

3. **Envie email:**
   - g.maaah.v@gmail.com

---

[⬆ Voltar](#-faq---perguntas-frequentes)
