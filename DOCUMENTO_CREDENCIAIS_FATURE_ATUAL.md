# 🔐 DOCUMENTO DE CREDENCIAIS FATURE CPA - VERSÃO ATUAL
## Sistema Ativo - Apenas Componentes em Uso
### Data: 24 de junho de 2025

---

## ⚠️ **DOCUMENTO CONFIDENCIAL**
**Este documento contém as credenciais essenciais do Sistema Fature CPA em produção:**
- 🔑 Credenciais GitHub e Railway
- 🗄️ Banco de dados da operação
- 📋 Repositórios ativos
- 🚀 Microserviços em produção
- 🏗️ Arquitetura atual

**MANTENHA EM LOCAL SEGURO - NÃO COMPARTILHE**

---

## 🔑 **CREDENCIAIS GITHUB**

### **Informações da Conta:**
- **Usuário:** ederziomek
- **Email:** ederziomek@upbet.com
- **Token de Acesso:** `[GITHUB_TOKEN_DISPONIVEL_EM_AMBIENTE_SEGURO]`

### **Configuração Git:**
```bash
git config --global user.name "EderZiomek"
git config --global user.email "ederziomek@upbet.com"
export GITHUB_TOKEN=[GITHUB_TOKEN_AQUI]
```

### **Clone com Token:**
```bash
git clone https://[GITHUB_TOKEN_AQUI]@github.com/ederziomek/[repo-name].git
```

---

## 🚂 **CREDENCIAIS RAILWAY**

### **Informações da Conta:**
- **Email:** ederziomek@upbet.com
- **Senha:** [SENHA_DISPONIVEL_EM_AMBIENTE_SEGURO]
- **Workspace:** ederziomek's Pro Workspace
- **Token de API:** `[RAILWAY_TOKEN_DISPONIVEL_EM_AMBIENTE_SEGURO]`

### **Login Railway:**
```bash
# Via CLI
railway login

# Ou configurar token diretamente
export RAILWAY_TOKEN=[RAILWAY_TOKEN_AQUI]
```

---

## 🗄️ **BANCO DE DADOS DA OPERAÇÃO**

### **PostgreSQL Externo (FONTE DE DADOS PRINCIPAL):**
- **Host:** 177.115.223.216
- **Porta:** 5999
- **Database:** dados_interno
- **Usuário:** userschapz
- **Senha:** [SENHA_DB_DISPONIVEL_EM_AMBIENTE_SEGURO]
- **Status:** ✅ OPERACIONAL
- **Função:** Fonte de dados da operação principal

### **String de Conexão:**
```
postgresql://userschapz:[SENHA_DB_AQUI]@177.115.223.216:5999/dados_interno
```

### **Configuração de Ambiente:**
```bash
EXTERNAL_DB_HOST=177.115.223.216
EXTERNAL_DB_PORT=5999
EXTERNAL_DB_USER=userschapz
EXTERNAL_DB_PASSWORD=[SENHA_DB_AQUI]
EXTERNAL_DB_NAME=dados_interno
EXTERNAL_DB_SSL=false
```

---

## 📂 **REPOSITÓRIOS ATIVOS NO GITHUB**

### **🌐 Frontends:**

#### **1. Backoffice Principal**
- **Repositório:** `backoffice-final`
- **URL:** https://github.com/ederziomek/backoffice-final
- **Tecnologia:** React + TypeScript + Tailwind CSS
- **Função:** Interface administrativa principal
- **Status:** ✅ ATIVO - Em produção
- **Descrição:** Sistema de gestão completo para afiliados, com dashboard, relatórios e configurações CPA

#### **2. Frontend Afiliados**
- **Repositório:** `frontend-afiliados-fature`
- **URL:** https://github.com/ederziomek/frontend-afiliados-fature
- **Tecnologia:** Next.js + Tailwind CSS
- **Função:** Portal para afiliados
- **Status:** ✅ ATIVO

### **🔧 Microserviços CPA (Node.js - ATIVOS):**

#### **1. Config Service**
- **Repositório:** `fature-config-service`
- **URL:** https://github.com/ederziomek/fature-config-service
- **Tecnologia:** Node.js + Express + PostgreSQL
- **Função:** Configurações dinâmicas do sistema CPA
- **Status:** ✅ ATIVO - Produção Railway
- **Descrição:** Gerencia valores CPA por nível (R$ 50/20/5/5/5), critérios de validação e regras de negócio dinâmicas
- **Banco:** `fature-config-db` (Railway)

#### **2. MLM Service V2**
- **Repositório:** `fature-mlm-service-v2`
- **URL:** https://github.com/ederziomek/fature-mlm-service-v2
- **Tecnologia:** Node.js + Express + PostgreSQL
- **Função:** Processamento MLM e distribuição CPA automática
- **Status:** ✅ ATIVO - Produção Railway
- **Descrição:** Calcula distribuição de comissões por níveis MLM, processa hierarquias e aplica regras de negócio
- **Banco:** `fature-mlm-db` (Railway)

#### **3. Commission Service**
- **Repositório:** `fature-commission-service`
- **URL:** https://github.com/ederziomek/fature-commission-service
- **Tecnologia:** Node.js + Express + PostgreSQL
- **Função:** Cálculo e validação de comissões CPA
- **Status:** ✅ ATIVO - Produção Railway
- **Descrição:** Processa validações CPA baseadas em critérios dinâmicos, calcula comissões e integra com MLM Service
- **Banco:** `fature-commission-db` (Railway)

#### **4. Data Service V2**
- **Repositório:** `fature-data-service-v2`
- **URL:** https://github.com/ederziomek/fature-data-service-v2
- **Tecnologia:** Node.js + Express + PostgreSQL
- **Função:** Analytics e sincronização de dados
- **Status:** ✅ ATIVO - Produção Railway
- **Descrição:** Sincroniza dados do banco da operação, gera analytics e exporta relatórios (CSV, JSON, XLSX, PDF)
- **Banco:** `fature-data-db` (Railway)

#### **5. API Gateway (PARA FORTALECER)**
- **Repositório:** `fature-api-gateway`
- **URL:** https://github.com/ederziomek/fature-api-gateway
- **Tecnologia:** Node.js + Express
- **Função:** Gateway central de APIs (BÁSICO ATUALMENTE)
- **Status:** ⚠️ BÁSICO - Precisa ser fortalecido
- **Descrição:** Atualmente apenas health checks. Deve ser evoluído para roteamento inteligente, autenticação e cache

### **📋 Repositórios de Documentação:**

#### **1. Documentação Final**
- **Repositório:** `Documentacao_final_fature`
- **URL:** https://github.com/ederziomek/Documentacao_final_fature
- **Função:** Documentação técnica e estratégica
- **Status:** ✅ CRIADO
- **Conteúdo:** Estratégias, credenciais e documentação técnica

---

## 🚀 **URLS DOS MICROSERVIÇOS EM PRODUÇÃO**

### **Microserviços Railway (Node.js):**
```bash
# Config Service
https://fature-config-service-production.up.railway.app

# MLM Service V2
https://fature-mlm-service-v2-production.up.railway.app

# Commission Service
https://fature-commission-service-production.up.railway.app

# Data Service V2
https://fature-data-service-v2-production.up.railway.app
```

### **Health Checks:**
```bash
curl https://fature-config-service-production.up.railway.app/api/v1/health
curl https://fature-mlm-service-v2-production.up.railway.app/api/v1/health
curl https://fature-commission-service-production.up.railway.app/api/v1/health
curl https://fature-data-service-v2-production.up.railway.app/api/v1/health
```

---

## 🗄️ **BANCOS DE DADOS ATIVOS NO RAILWAY**

### **Bancos PostgreSQL em Produção:**
```bash
1. fature-config-db      # Config Service - Configurações dinâmicas
2. fature-mlm-db         # MLM Service V2 - Processamento MLM
3. fature-commission-db  # Commission Service - Cálculo de comissões
4. fature-data-db        # Data Service V2 - Analytics e sincronização
```

### **Configuração de Ambiente (Railway):**
```bash
DATABASE_URL=${{Postgres.DATABASE_URL}}
```

---

## 🔐 **API KEYS E SECRETS DOS MICROSERVIÇOS**

### **API Keys:**
```bash
# Config Service
CONFIG_SERVICE_API_KEY=[API_KEY_CONFIG_DISPONIVEL_EM_AMBIENTE_SEGURO]

# MLM Service V2
MLM_SERVICE_API_KEY=[API_KEY_MLM_DISPONIVEL_EM_AMBIENTE_SEGURO]

# Commission Service
COMMISSION_SERVICE_API_KEY=[API_KEY_COMMISSION_DISPONIVEL_EM_AMBIENTE_SEGURO]

# Data Service V2
DATA_SERVICE_API_KEY=[API_KEY_DATA_DISPONIVEL_EM_AMBIENTE_SEGURO]
```

---

## 🎯 **CONFIGURAÇÕES CPA ATUAIS**

### **Valores CPA por Nível:**
```json
{
  "cpa_level_amounts": {
    "level_1": 50.00,
    "level_2": 20.00,
    "level_3": 5.00,
    "level_4": 5.00,
    "level_5": 5.00
  }
}
```

### **Regras de Validação CPA (Modelo Flexível - ATIVO):**
```json
{
  "model_name": "Modelo Flexível (Padrão)",
  "model_description": "Depósito mínimo: 30 R$ AND Apostas mínimas: 10 apostas) OR (Depósito mínimo: 30 R$ AND GGR mínimo: 25 R$)",
  "status": "ATIVO",
  "global_operator": "OR",
  "groups": [
    {
      "group_id": 1,
      "group_name": "Depósito + Apostas",
      "operator": "E",
      "criteria": [
        {
          "type": "deposit",
          "label": "Depósito mínimo",
          "value": 30.00,
          "currency": "R$",
          "enabled": true
        },
        {
          "type": "bets_count",
          "label": "Apostas mínimas",
          "value": 10,
          "unit": "apostas",
          "enabled": true
        },
        {
          "type": "ggr",
          "label": "GGR mínimo",
          "value": 0,
          "currency": "R$",
          "enabled": false
        }
      ]
    },
    {
      "group_id": 2,
      "group_name": "Depósito + GGR",
      "operator": "E",
      "criteria": [
        {
          "type": "deposit",
          "label": "Depósito mínimo",
          "value": 30.00,
          "currency": "R$",
          "enabled": true
        },
        {
          "type": "bets_count",
          "label": "Apostas mínimas",
          "value": 0,
          "unit": "apostas",
          "enabled": false
        },
        {
          "type": "ggr",
          "label": "GGR mínimo",
          "value": 25.00,
          "currency": "R$",
          "enabled": true
        }
      ]
    }
  ]
}
```

---

## 🏗️ **ARQUITETURA ATUAL DO SISTEMA**

### **Fluxo de Dados:**
```
Frontend (Backoffice) → API Local → Banco da Operação
                     ↓
              Microserviços CPA (Railway)
                     ↓
              Bancos PostgreSQL (Railway)
```

### **Componentes Ativos:**
- ✅ **Frontend React** (backoffice-final)
- ✅ **API Local Node.js** (dentro do backoffice)
- ✅ **4 Microserviços CPA** (Railway)
- ✅ **4 Bancos PostgreSQL** (Railway)
- ✅ **Banco da Operação** (PostgreSQL externo)

### **Componentes Obsoletos (NÃO USAR):**
- ❌ **fature-api-proxy** (Python - Descontinuado)
- ❌ **fature-real-data-service** (Python - Descontinuado)
- ❌ **fature-database-melo-dados-simulados** (Banco não utilizado)

---

## 📋 **COMANDOS ÚTEIS PARA MANUS**

### **GitHub:**
```bash
# Configurar ambiente
git config --global user.name "EderZiomek"
git config --global user.email "ederziomek@upbet.com"

# Clonar repositório
git clone https://[GITHUB_TOKEN_AQUI]@github.com/ederziomek/[repo-name].git

# Push com token
git remote set-url origin https://[GITHUB_TOKEN_AQUI]@github.com/ederziomek/[repo-name].git
```

### **Railway:**
```bash
# Login
railway login

# Deploy
railway up

# Logs
railway logs

# Variáveis
railway variables set KEY=value
```

### **Teste de Integração:**
```bash
# Testar Config Service
curl -H "X-API-Key: [API_KEY_CONFIG_AQUI]" \
     https://fature-config-service-production.up.railway.app/api/v1/config/cpa_level_amounts/value
```

---

## 📊 **STATUS ATUAL DO PROJETO**

### **Implementado e Funcionando:**
- ✅ **4 Microserviços CPA** em produção
- ✅ **Backoffice React** operacional
- ✅ **Integração com banco da operação**
- ✅ **Sistema CPA** com configurações dinâmicas
- ✅ **Documentação estratégica** completa

### **Próximas Implementações (Conforme Estratégia):**
- 🔄 **Banco Robusto de Afiliados** (fature-affiliate-service)
- 🔄 **API Gateway Fortalecido** (roteamento inteligente)
- 🔄 **ETL Automatizado** (sincronização otimizada)

---

## 🎯 **INFORMAÇÕES PARA MANUS**

### **Contexto do Sistema:**
- **Projeto:** Sistema de afiliados CPA para operação de jogos
- **Arquitetura:** Microserviços + Frontend React
- **Banco Principal:** PostgreSQL externo da operação
- **Deploy:** Railway para microserviços
- **Linguagem:** Node.js + TypeScript

### **Dados Importantes:**
- **Total de Afiliados:** ~48,261
- **Total de Indicações:** ~614,944
- **Valores CPA:** R$ 50/20/5/5/5 por nível
- **Critérios CPA:** Depósito R$ 30 + 10 apostas + R$ 100 apostado + 3 dias ativo

### **Repositório Principal de Trabalho:**
- **backoffice-final** - Sistema principal onde a maioria das tarefas acontece
- Página principal: **Afiliados2** (RealAffiliatesPage.tsx)

---

## 🔒 **NOTA DE SEGURANÇA**

**TOKENS E SENHAS REAIS:** Por questões de segurança, os tokens e senhas reais foram substituídos por placeholders neste documento público. Os valores reais estão disponíveis em ambiente seguro e devem ser utilizados durante a implementação.

**ACESSO ÀS CREDENCIAIS REAIS:**
- Tokens GitHub e Railway disponíveis no ambiente de desenvolvimento
- Senhas de banco de dados em variáveis de ambiente seguras
- API Keys dos microserviços configuradas no Railway

---

**DOCUMENTO ATUALIZADO EM:** 24 de junho de 2025  
**VERSÃO:** 2.0 - Versão Segura para Repositório Público  
**PRÓXIMA REVISÃO:** Conforme necessidade

**ESTE DOCUMENTO DEVE SER USADO PARA INICIAR QUALQUER TAREFA NO MANUS**

