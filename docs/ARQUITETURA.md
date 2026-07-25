# 🏗️ ARQUITETURA - Chatbot SaaS

Documentação técnica completa da arquitetura do sistema.

---

## 📋 Índice

1. [Visão Geral](#visão-geral)
2. [Componentes Principais](#componentes-principais)
3. [Fluxo de Dados](#fluxo-de-dados)
4. [Banco de Dados](#banco-de-dados)
5. [Segurança](#segurança)
6. [Performance](#performance)

---

## 🎯 Visão Geral

```
                    ┌─────────────────────┐
                    │   Paciente/Cliente  │
                    │    (WhatsApp)       │
                    └──────────┬──────────┘
                               │
                    ┌──────────▼──────────┐
                    │  Twilio WhatsApp    │
                    │      Gateway        │
                    └──────────┬──────────┘
                               │
                    ┌──────────▼──────────────────┐
                    │   Node.js Server            │
                    │   (Express.js Framework)    │
                    └──────────┬──────────────────┘
                               │
        ┌──────────────────────┼──────────────────────┐
        │                      │                      │
        ▼                      ▼                      ▼
   ┌─────────┐          ┌──────────┐          ┌─────────┐
   │ Claude  │          │ SQLite   │          │ Stripe  │
   │   AI    │          │   DB     │          │Payment  │
   └─────────┘          └──────────┘          └─────────┘
        │                      │                      │
        └──────────────────────┼──────────────────────┘
                               │
                    ┌──────────▼──────────┐
                    │   Resposta ao       │
                    │   Paciente (IA)     │
                    └─────────────────────┘
```

---

## 🔧 Componentes Principais

### **1. Frontend (Client-Side)**

```
HTML5 + CSS3 + JavaScript (Vanilla)
    │
    ├─ login.html          → Autenticação admin
    ├─ registro.html       → Registro de clínica
    ├─ dashboard.html      → Painel principal (4 abas)
    ├─ checkout.html       → Pagamento Stripe
    │
    └─ public/js/          → Lógica de frontend
       ├─ api.js           → Chamadas AJAX
       └─ utils.js         → Funções auxiliares
```

**Responsabilidades:**
- Renderizar UI
- Validação de forma (client-side)
- Chamadas AJAX para APIs
- Gerenciar tokens de session (cookie)

---

### **2. Backend (Server-Side)**

```
Node.js v20 + Express.js 4.x
    │
    ├─ server.js           → Arquivo principal
    │
    ├─ config/             → Configurações
    │   ├─ env.js          → Variáveis de ambiente
    │   └─ twilio.js       → Cliente Twilio
    │
    ├─ routes/             → Rotas HTTP
    │   ├─ auth.js         → Login, Registro, Logout
    │   ├─ admin.js        → Dashboard APIs
    │   ├─ api.js          → APIs gerais
    │   └─ whatsapp.js     → Webhook Twilio
    │
    └─ modules/            → Lógica de negócio
        ├─ procedimentos.js → CRUD procedimentos
        ├─ auth.js         → Autenticação
        └─ utils.js        → Funções auxiliares
```

**Responsabilidades:**
- Autenticação e autorização
- Processamento de requisições HTTP
- Orquestração de APIs externas
- Gerenciamento de sessão
- Validação de dados

---

### **3. Inteligência Artificial**

```
Claude API (Anthropic)
    │
    └─ claude-haiku-4-5-20251001
       │
       ├─ Input: Prompt dinâmico + Histórico
       ├─ Processing: Processamento de linguagem natural
       └─ Output: Resposta contextualizada
```

**Fluxo:**

```
1. Paciente manda mensagem no WhatsApp
2. Twilio recebe e envia para servidor
3. Servidor busca dados da clínica
4. Cria prompt dinâmico com:
   - Nome da clínica
   - Procedimentos cadastrados
   - Preços
   - Horários
   - Políticas
5. Envia para Claude API
6. Claude processa e retorna resposta
7. Servidor devolve para Twilio
8. Twilio envia de volta para paciente
```

---

### **4. Banco de Dados**

```
SQLite3 (Local, Portável)
    │
    ├─ clinicas              → Dados das clínicas
    ├─ usuarios              → Credenciais admin
    ├─ procedimentos         → Serviços cadastrados
    ├─ agendamentos          → Histórico de agendamentos
    ├─ politicas             → Políticas da clínica
    └─ whatsapp_numeros      → Números associados
```

**Características:**
- ✅ Arquivo único (chatbot.db)
- ✅ Totalmente portável
- ✅ Sem servidor externo
- ✅ Ideal para MVP/SaaS pequeno

---

### **5. Pagamento**

```
Stripe Payment Gateway
    │
    ├─ Criação de Customer   → Ao registrar
    ├─ Payment Intent        → Ao fazer checkout
    ├─ Processamento         → Validação de cartão
    └─ Webhook               → Confirmação de pagamento
```

**Fluxo:**

```
1. Clínica registra
   └─ Sistema cria Customer no Stripe

2. Clínica clica "Pagar"
   └─ Cria Payment Intent
   └─ Mostra formulário Stripe

3. Clínica preenche cartão
   └─ Stripe processa
   └─ Retorna sucesso/erro

4. Servidor recebe webhook
   └─ Confirma pagamento
   └─ Ativa subscription (30 dias)
```

---

### **6. WhatsApp Integration**

```
Twilio WhatsApp API
    │
    ├─ Webhook entrada       → POST /whatsapp
    ├─ Processamento         → Rotear por clínica
    ├─ IA                    → Gerar resposta
    └─ Envio resposta        → Via Twilio API
```

**Características:**
- ✅ Sandbox para testes
- ✅ Produção com número de negócio
- ✅ Suporta mídia (imagens, documentos)
- ✅ Webhooks para eventos

---

## 📊 Fluxo de Dados

### **Fluxo 1: Registro de Clínica**

```
Paciente acessa /registrar
    ↓
Preenche formulário (nome, email, senha)
    ↓
POST /api/auth/registrar
    ↓
Validação de dados
    ↓
Cria Customer no Stripe
    ↓
Insere clinica na tabela clinicas
    ↓
Insere usuario na tabela usuarios
    ↓
Insere 3 políticas padrão
    ↓
Faz login automático
    ↓
Redireciona para /checkout.html
```

---

### **Fluxo 2: Pagamento (Stripe)**

```
Cliente no /checkout.html
    ↓
Clica "Iniciar Assinatura"
    ↓
POST /api/payment/create-subscription
    ↓
Cria Payment Intent (R$ 399)
    ↓
Retorna clientSecret para frontend
    ↓
Stripe.js mostra formulário de cartão
    ↓
Cliente preenche cartão
    ↓
Stripe processa (backend)
    ↓
Webhook /api/payment/webhook
    ↓
UPDATE clinicas SET subscription_status = 'active'
    ↓
Subscription_expires = hoje + 30 dias
    ↓
Frontend mostra "✅ Pagamento realizado!"
```

---

### **Fluxo 3: Chatbot WhatsApp**

```
Paciente manda mensagem no WhatsApp
    ↓ (via Twilio)
POST /whatsapp (webhook)
    ↓
Extrai From, Body
    ↓
Busca clínica pelo número (whatsapp_numeros)
    ↓
Verifica se subscription está ativa
    ↓
Busca procedimentos da clínica
    ↓
Cria prompt dinâmico
    ↓
Envia para Claude API
    ↓
Claude processa e retorna resposta
    ↓
Salva na session (histórico)
    ↓
Envia de volta via Twilio
    ↓ (via WhatsApp)
Paciente recebe resposta IA
```

---

## 🗄️ Banco de Dados

### **Estrutura de Tabelas**

#### **clinicas**

```sql
CREATE TABLE clinicas (
  id INTEGER PRIMARY KEY,
  nome TEXT NOT NULL,
  email TEXT UNIQUE,
  telefone TEXT,
  endereco TEXT,
  horario_abertura TEXT DEFAULT '09:00',
  horario_fechamento TEXT DEFAULT '19:00',
  dias_funcionamento TEXT DEFAULT 'Segunda a sábado',
  ativa INTEGER DEFAULT 1,
  
  -- Stripe
  stripe_customer_id TEXT,
  subscription_status TEXT DEFAULT 'pending', -- pending, active, canceled
  subscription_expires TEXT,
  
  created_at TEXT DEFAULT CURRENT_TIMESTAMP
);
```

#### **usuarios**

```sql
CREATE TABLE usuarios (
  id INTEGER PRIMARY KEY,
  clinica_id INTEGER NOT NULL,
  email TEXT UNIQUE NOT NULL,
  senha_hash TEXT NOT NULL,
  nome TEXT,
  ativo INTEGER DEFAULT 1,
  
  FOREIGN KEY (clinica_id) REFERENCES clinicas(id)
);
```

#### **procedimentos**

```sql
CREATE TABLE procedimentos (
  id INTEGER PRIMARY KEY,
  clinica_id INTEGER NOT NULL,
  nome TEXT NOT NULL,
  duracao_minutos INTEGER,
  preco REAL,
  descricao TEXT,
  
  FOREIGN KEY (clinica_id) REFERENCES clinicas(id)
);
```

#### **agendamentos**

```sql
CREATE TABLE agendamentos (
  id INTEGER PRIMARY KEY,
  clinica_id INTEGER NOT NULL,
  data TEXT,
  hora TEXT,
  nome_cliente TEXT,
  procedimento TEXT,
  status TEXT DEFAULT 'confirmado',
  
  FOREIGN KEY (clinica_id) REFERENCES clinicas(id)
);
```

#### **politicas**

```sql
CREATE TABLE politicas (
  id INTEGER PRIMARY KEY,
  clinica_id INTEGER NOT NULL,
  descricao TEXT,
  
  FOREIGN KEY (clinica_id) REFERENCES clinicas(id)
);
```

#### **whatsapp_numeros**

```sql
CREATE TABLE whatsapp_numeros (
  id INTEGER PRIMARY KEY,
  clinica_id INTEGER NOT NULL,
  numero_whatsapp TEXT,
  nome_numero TEXT,
  principal INTEGER DEFAULT 0,
  ativa INTEGER DEFAULT 1,
  
  FOREIGN KEY (clinica_id) REFERENCES clinicas(id)
);
```

---

### **Relacionamentos**

```
clinicas (1) ──── (N) usuarios
clinicas (1) ──── (N) procedimentos
clinicas (1) ──── (N) agendamentos
clinicas (1) ──── (N) politicas
clinicas (1) ──── (N) whatsapp_numeros
```

---

## 🔒 Segurança

### **Autenticação**

```
Fluxo:
1. Usuário digita email + senha
2. Sistema faz hash SHA256 da senha
3. Compara com hash armazenado
4. Se bater, cria session
5. Session armazenada em cookie seguro
6. Todas requisições verificam session
```

### **Autorização**

```
Multi-tenant:
- Cada query inclui WHERE clinica_id = ?
- Usuário só vê dados de sua clínica
- Impossível acessar dados de outro tenant
```

### **Proteção de Dados**

```
✅ Senhas: SHA256
✅ Session: express-session (cookie seguro)
✅ Variáveis: .env (não versionado)
✅ Validação: Entrada validada antes de processar
✅ SQL Injection: Queries parametrizadas (?)
```

---

## ⚡ Performance

### **Otimizações Implementadas**

```
✅ Session caching (não refetch de DB a cada request)
✅ Histórico de chat limitado (últimas 10 mensagens)
✅ SQLite índices nas colunas mais usadas
✅ Static files servidos por Express
✅ JSON responses comprimidas
```

### **Métricas Esperadas**

```
Tempo de resposta: < 200ms
Concorrência: até 100 simultâneas
Throughput: 1000+ requests/min
Uptime: 99.9%
```

### **Limitações Conhecidas**

```
⚠️ SQLite: Ideal até 10k+ registros
   → Migrar para PostgreSQL se > 1M registros

⚠️ Sessão em memória: Reinicia ao restart
   → Usar Redis em produção se precisar persistência

⚠️ Single thread Node.js: CPU-bound limitado
   → Worker threads para processamento pesado
```

---

## 🚀 Escalabilidade Futura

### **Fase 1 (Atual - MVP)**
```
Single server + SQLite
Bom para: Até 100 clientes
```

### **Fase 2 (5-50 clientes)**
```
DigitalOcean + PostgreSQL + Redis
Bom para: Até 1000 clientes
```

### **Fase 3 (50+ clientes)**
```
Load Balancer + 3 servidores + PostgreSQL
+ Redis + CDN
Bom para: Até 10k+ clientes
```

---

[⬆ Voltar](#-arquitetura---chatbot-saas)
