# 🔌 DOCUMENTAÇÃO DE APIs - Chatbot SaaS

Referência completa de todos os endpoints disponíveis.

---

## 📋 Índice

1. [Autenticação](#autenticação)
2. [Empresa](#empresa)
3. [Procedimentos](#procedimentos)
4. [Agendamentos](#agendamentos)
5. [WhatsApp](#whatsapp)
6. [Pagamento](#pagamento)
7. [Chat](#chat)

---

## 🔐 Autenticação

### **Login**

```http
POST /api/auth/login
Content-Type: application/json

{
  "usuario": "admin@clinica.com",
  "senha": "senha123"
}
```

**Response (200):**
```json
{
  "sucesso": true,
  "mensagem": "Login realizado"
}
```

**Response (401):**
```json
{
  "erro": "Usuário ou senha incorretos"
}
```

---

### **Logout**

```http
POST /api/auth/logout
```

**Response (200):**
```json
{
  "sucesso": true
}
```

---

### **Status**

```http
GET /api/auth/status
```

**Response (200 - Logado):**
```json
{
  "logado": true,
  "usuario": "admin@clinica.com",
  "clinica": "Clínica XYZ"
}
```

**Response (200 - Não logado):**
```json
{
  "logado": false
}
```

---

### **Registrar**

```http
POST /api/auth/registrar
Content-Type: application/json

{
  "clinica": "Clínica de Estética XYZ",
  "email": "admin@clinica.com",
  "senha": "senha123",
  "telefone": "+5511999999999",
  "endereco": "Rua X, 123"
}
```

**Response (200):**
```json
{
  "sucesso": true,
  "mensagem": "Registrado!",
  "clinicaId": 5,
  "redirect": "/checkout.html"
}
```

---

## 🏢 Empresa

### **Obter Dados**

```http
GET /api/admin/empresa
Authorization: Session (cookie)
```

**Response (200):**
```json
{
  "id": 1,
  "nome": "Clínica XYZ",
  "email": "admin@clinica.com",
  "telefone": "+5511999999999",
  "endereco": "Rua X, 123",
  "horario_abertura": "09:00",
  "horario_fechamento": "19:00",
  "dias_funcionamento": "Segunda a sábado",
  "stripe_customer_id": "cus_XXXXX",
  "subscription_status": "active",
  "subscription_expires": "2026-08-25T14:00:00.000Z"
}
```

---

### **Atualizar Dados**

```http
POST /api/admin/empresa
Authorization: Session
Content-Type: application/json

{
  "nome": "Clínica Nova",
  "email": "novo@clinica.com",
  "telefone": "+5511888888888",
  "endereco": "Avenida Y, 456"
}
```

**Response (200):**
```json
{
  "sucesso": true,
  "mensagem": "Empresa atualizada"
}
```

---

## 📋 Procedimentos

### **Listar Todos**

```http
GET /api/admin/procedimentos
Authorization: Session
```

**Response (200):**
```json
{
  "procedimentos": [
    {
      "id": 1,
      "clinica_id": 1,
      "nome": "Limpeza de Pele",
      "duracao_minutos": 60,
      "preco": 150.00,
      "descricao": "Limpeza profunda com extração"
    },
    {
      "id": 2,
      "clinica_id": 1,
      "nome": "Aplicação de Botox",
      "duracao_minutos": 30,
      "preco": 300.00,
      "descricao": "Aplicação profissional de Botox"
    }
  ]
}
```

---

### **Obter Um Procedimento**

```http
GET /api/admin/procedimentos/1
Authorization: Session
```

**Response (200):**
```json
{
  "id": 1,
  "clinica_id": 1,
  "nome": "Limpeza de Pele",
  "duracao_minutos": 60,
  "preco": 150.00,
  "descricao": "Limpeza profunda com extração"
}
```

---

### **Criar Novo**

```http
POST /api/admin/procedimentos-novo
Authorization: Session
Content-Type: application/json

{
  "nome": "Peeling Químico",
  "duracao_minutos": 45,
  "preco": 200.00,
  "descricao": "Peeling de ácido glicólico 30%"
}
```

**Response (200):**
```json
{
  "sucesso": true,
  "mensagem": "Procedimento criado!",
  "id": 3
}
```

---

### **Atualizar**

```http
PUT /api/admin/procedimentos/1/editar
Authorization: Session
Content-Type: application/json

{
  "nome": "Limpeza de Pele Profunda",
  "duracao_minutos": 75,
  "preco": 180.00,
  "descricao": "Limpeza profunda com extração manual"
}
```

**Response (200):**
```json
{
  "sucesso": true,
  "mensagem": "Procedimento atualizado!"
}
```

---

### **Deletar**

```http
DELETE /api/admin/procedimentos/1
Authorization: Session
```

**Response (200):**
```json
{
  "sucesso": true,
  "mensagem": "Procedimento deletado!"
}
```

---

## 📅 Agendamentos

### **Listar Todos**

```http
GET /api/admin/agendamentos
Authorization: Session
```

**Response (200):**
```json
[
  {
    "id": 1,
    "clinica_id": 1,
    "data": "2026-08-10",
    "hora": "14:00",
    "nome_cliente": "João Silva",
    "procedimento": "Limpeza de Pele",
    "status": "confirmado"
  },
  {
    "id": 2,
    "clinica_id": 1,
    "data": "2026-08-11",
    "hora": "15:30",
    "nome_cliente": "Maria Santos",
    "procedimento": "Botox",
    "status": "pendente"
  }
]
```

---

## 💬 WhatsApp

### **Listar Números**

```http
GET /api/admin/whatsapp-numeros
Authorization: Session
```

**Response (200):**
```json
[
  {
    "id": 1,
    "clinica_id": 1,
    "numero_whatsapp": "whatsapp:+5511999999999",
    "nome_numero": "Principal",
    "principal": 1,
    "ativa": 1
  }
]
```

---

### **Adicionar Número**

```http
POST /api/admin/whatsapp-numeros
Authorization: Session
Content-Type: application/json

{
  "numero_whatsapp": "whatsapp:+5511988888888",
  "nome_numero": "Secundário"
}
```

**Response (200):**
```json
{
  "sucesso": true,
  "mensagem": "Número adicionado"
}
```

---

### **Webhook WhatsApp (Twilio)**

```http
POST /whatsapp
Content-Type: application/x-www-form-urlencoded

From=whatsapp:+5511999999999&
Body=Olá, quanto custa a limpeza de pele?&
To=whatsapp:+14155238886
```

**Response (200):**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<Response>
  <Message>Limpeza de pele custa R$ 150 e dura 60 minutos...</Message>
</Response>
```

---

## 💳 Pagamento

### **Criar Subscription (Stripe)**

```http
POST /api/payment/create-subscription
Authorization: Session
Content-Type: application/json
```

**Response (200):**
```json
{
  "clientSecret": "pi_1234567890",
  "publishableKey": "pk_test_XXXXX",
  "sucesso": true
}
```

---

### **Webhook Stripe**

```http
POST /api/payment/webhook
Content-Type: application/json
stripe-signature: t=1234567890,v1=XXXXX

{
  "type": "payment_intent.succeeded",
  "data": {
    "object": {
      "id": "pi_1234567890",
      "status": "succeeded",
      "amount": 39900,
      "currency": "brl",
      "metadata": {
        "clinica_id": 1
      }
    }
  }
}
```

**Response (200):**
```json
{
  "received": true
}
```

---

## 🤖 Chat IA

### **Enviar Mensagem**

```http
POST /chat
Content-Type: application/json

{
  "sessionId": "usuario-123",
  "message": "Olá, qual é o preço da limpeza de pele?",
  "clinicaId": 1
}
```

**Response (200):**
```json
{
  "reply": "Olá! A limpeza de pele custa R$ 150 e dura 60 minutos. Oferece limpeza profunda com extração. Quer agendar?"
}
```

---

### **Fluxo de Chat**

```
Cliente: "Oi"
Bot: "Olá! Bem-vindo à Clínica XYZ. Como posso ajudar?"

Cliente: "Quanto custa a limpeza?"
Bot: "Limpeza de pele custa R$ 150, 60min. Quer agendar?"

Cliente: "Sim, segunda à noite"
Bot: "Perfeito! Agendado para segunda às 19:00. Confirma?"

Cliente: "Sim"
Bot: "✅ Agendamento confirmado! Você receberá uma mensagem de confirmação."
```

---

## 📊 Health Check

```http
GET /health
```

**Response (200):**
```json
{
  "status": "ok"
}
```

---

## 🔄 Padrão de Resposta

Todas as APIs retornam JSON com padrão:

**Sucesso (2xx):**
```json
{
  "sucesso": true,
  "mensagem": "Descrição",
  "dados": {}
}
```

**Erro (4xx/5xx):**
```json
{
  "erro": "Descrição do erro"
}
```

---

## 🔑 Autenticação

### **Headers Necessários**

```http
Authorization: Cookie (session cookie)
Content-Type: application/json
```

### **Session**

A autenticação usa `express-session` com cookie seguro.

Após login, todos os requests carregam a session automaticamente via cookie.

---

## 🛡️ Segurança

- ✅ Todas as endpoints (exceto `/whatsapp`, `/health`, `/login`, `/registrar`) requerem autenticação
- ✅ Isolamento de dados por `clinica_id`
- ✅ Validação de entrada em todas as APIs
- ✅ Hash SHA256 para senhas

---

## 📈 Rate Limiting

Não há rate limiting implementado. Para produção, adicione:

```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 100 // limite 100 requisições por IP
});

app.use(limiter);
```

---

## 🧪 Testar APIs

### **Com cURL**

```bash
# Login
curl -X POST http://localhost:3000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"usuario":"admin@test.com","senha":"senha123"}'

# Listar procedimentos
curl -X GET http://localhost:3000/api/admin/procedimentos
```

### **Com Postman**

1. Importe a collection (em breve disponível)
2. Configure ambiente com `BASE_URL=http://localhost:3000`
3. Execute requisições

---

[⬆ Voltar](#-documentação-de-apis---chatbot-saas)
