# 🚀 ESTRATÉGIA ARQUITETURAL FATURE CPA
## Banco de Dados Robusto para Afiliados e Arquitetura de Microsserviços

---

**Documento Estratégico Completo**  
**Data:** 24 de junho de 2025  
**Versão:** 1.0  
**Status:** Estratégia Definida - Pronto para Implementação

---

## 📋 SUMÁRIO EXECUTIVO

Este documento apresenta a estratégia completa para evolução da arquitetura do sistema Fature CPA, focando na criação de um **banco de dados robusto para afiliados** e na otimização da **arquitetura de microsserviços** existente.

### 🎯 Objetivos Principais

1. **Eliminar processamento desnecessário** através de dados pré-agregados
2. **Otimizar performance** das consultas de afiliados e MLM
3. **Centralizar informações** de afiliados em estrutura dedicada
4. **Manter arquitetura de microsserviços** escalável e eficiente
5. **Facilitar desenvolvimento** de novas funcionalidades

### 🔑 Princípios Fundamentais

- **"Armazenamento é mais barato que processamento"**
- **Dados pré-calculados e agregados**
- **Evitar consultas complexas em tempo real**
- **Microsserviços especializados com bancos dedicados**
- **API Gateway como ponto único de entrada**

---

## 🏗️ ARQUITETURA ATUAL vs ARQUITETURA PROPOSTA

### 📊 Situação Atual

**Problemas Identificados:**
- Processamento intensivo para gerar dados de afiliados
- Consultas complexas ao banco da operação
- Dados dispersos entre múltiplos serviços
- Falta de otimização para consultas MLM
- Dependência excessiva do banco externo

**Componentes Ativos:**
- ✅ **4 Microsserviços CPA (Node.js):** Config, MLM, Commission, Data
- ✅ **API Local do Backoffice:** Endpoints básicos
- ✅ **Banco PostgreSQL Externo:** Dados da operação (177.115.223.216)
- ❌ **Serviços Python:** Obsoletos (fature-api-proxy, fature-real-data-service)

### 🎯 Arquitetura Proposta

**Estrutura Otimizada:**

```
┌─────────────────────────────────────────────────────────────┐
│                    FRONTEND LAYER                           │
├─────────────────────────────────────────────────────────────┤
│  Backoffice React  │  Portal Afiliados  │  Mobile App      │
└─────────────────┬───────────────────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────────────────┐
│                 API GATEWAY ROBUSTO                         │
│  • Roteamento Inteligente  • Autenticação JWT              │
│  • Rate Limiting          • Cache de Respostas             │
│  • Agregação de Dados     • Monitoramento                  │
└─────────────────┬───────────────────────────────────────────┘
                  │
    ┌─────────────┼─────────────┬─────────────┬─────────────┐
    │             │             │             │             │
┌───▼────┐ ┌─────▼────┐ ┌──────▼───┐ ┌──────▼───┐ ┌──────▼───┐
│Config  │ │Affiliate │ │   MLM    │ │Commission│ │   Data   │
│Service │ │ Service  │ │ Service  │ │ Service  │ │ Service  │
└───┬────┘ └─────┬────┘ └──────┬───┘ └──────┬───┘ └──────┬───┘
    │            │             │            │            │
┌───▼────┐ ┌─────▼────┐ ┌──────▼───┐ ┌──────▼───┐ ┌──────▼───┐
│Config  │ │AFFILIATE │ │   MLM    │ │Commission│ │   Data   │
│   DB   │ │ DB ROBUSTO│ │    DB    │ │    DB    │ │    DB    │
└────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘
                  ▲
                  │ ETL/Sincronização
                  │
           ┌──────▼──────┐
           │   BANCO DA  │
           │  OPERAÇÃO   │
           │(PostgreSQL) │
           └─────────────┘
```

---



## 🗄️ BANCO DE DADOS ROBUSTO PARA AFILIADOS

### 🎯 Conceito e Justificativa

O **Banco de Dados Robusto para Afiliados** é o componente central da nova estratégia, projetado especificamente para:

- **Armazenar dados pré-processados** de toda a rede MLM
- **Eliminar consultas complexas** em tempo real
- **Centralizar informações** de afiliados em estrutura otimizada
- **Facilitar relatórios** e dashboards instantâneos

### 📊 Estrutura de Dados Detalhada

#### 1. **Tabela Principal: `affiliates`**
**Função:** Dados cadastrais e métricas agregadas de cada afiliado

**Campos Principais:**
- `affiliate_id` - Identificador único
- `name`, `email`, `phone` - Dados cadastrais
- `status` - Status do afiliado (active, inactive, suspended)
- `total_referrals` - Total de indicações (pré-calculado)
- `total_validated_referrals` - Indicações validadas (pré-calculado)
- `total_deposits`, `total_bets`, `total_ggr` - Valores financeiros agregados
- `total_cpa_earned`, `total_rev_earned` - Comissões totais

#### 2. **Estrutura MLM: `affiliate_mlm_structure`**
**Função:** Dados da rede MLM pré-calculados por afiliado

**Breakdown por Nível (1-5):**
- `level_X_count` - Quantidade de indicações por nível
- `level_X_deposits` - Total de depósitos por nível
- `level_X_bets` - Total de apostas por nível
- `level_X_ggr` - GGR total por nível
- `level_X_cpa` - CPA pago por nível
- `level_X_rev` - REV pago por nível

**Exemplo de Dados:**
```sql
affiliate_id: 1622968
level_1_count: 757
level_1_deposits: 2,450,000.00
level_1_cpa: 37,850.00
level_2_count: 3478
level_2_deposits: 8,950,000.00
level_2_cpa: 69,560.00
-- ... até level_5
total_network_size: 95558
```

#### 3. **Indicações Detalhadas: `affiliate_referrals`**
**Função:** Registro completo de todas as indicações

**Informações por Indicação:**
- `referrer_id` / `referred_id` - Relacionamento
- `mlm_level` - Nível na rede (1-5)
- `referral_date` - Data da indicação
- `validation_date` - Data da validação CPA
- `status` - Status da indicação (pending, validated, etc.)
- `total_deposits`, `total_bets` - Dados financeiros do indicado
- `cpa_amount` - Valor CPA gerado
- `validation_criteria` - Critérios atendidos (JSON)

#### 4. **Transações Financeiras: `affiliate_financial_transactions`**
**Função:** Histórico completo de transações por afiliado e nível

**Tipos de Transação:**
- `deposit` - Depósitos
- `withdrawal` - Saques
- `bet` - Apostas
- `win` - Prêmios
- `bonus` - Bônus
- `cpa` - Pagamentos CPA
- `rev` - Pagamentos REV

**Campos por Transação:**
- `affiliate_id` - Afiliado responsável
- `user_id` - Usuário que fez a transação
- `mlm_level` - Nível na rede
- `transaction_type` - Tipo da transação
- `amount` - Valor
- `transaction_date` - Data da transação

#### 5. **Validações CPA: `affiliate_cpa_validations`**
**Função:** Histórico de todas as validações CPA

**Dados da Validação:**
- `affiliate_id` - Afiliado que receberá o CPA
- `referred_user_id` - Usuário que foi validado
- `validation_date` - Data da validação
- `validation_criteria` - Critérios atendidos (JSON)
- `user_deposits`, `user_bets`, `days_active` - Métricas no momento da validação
- `cpa_level` - Nível do CPA (1-5)
- `cpa_amount` - Valor do CPA
- `cpa_status` - Status do pagamento

#### 6. **Resumos Periódicos: `affiliate_periodic_summaries`**
**Função:** Dados agregados por período para relatórios rápidos

**Períodos Suportados:**
- `daily` - Resumos diários
- `weekly` - Resumos semanais
- `monthly` - Resumos mensais
- `yearly` - Resumos anuais

**Métricas por Período:**
- `new_referrals` - Novas indicações
- `validated_referrals` - Indicações validadas
- `total_deposits`, `total_bets` - Valores financeiros
- `cpa_earned`, `rev_earned` - Comissões do período
- `level_breakdown` - Detalhes por nível (JSON)

### 🔄 Processo ETL (Extract, Transform, Load)

#### **Fonte de Dados: Banco da Operação**

**Tabelas Analisadas:**
1. **`cadastro`** - Dados cadastrais dos usuários
2. **`casino_bets_v`** - Apostas e jogos
3. **`depositos`** - Depósitos realizados
4. **`saque`** - Saques solicitados
5. **`tracked`** - Relacionamentos de indicação

#### **Processo de Sincronização:**

1. **Extração (Extract):**
   - Conectar ao banco da operação (177.115.223.216:5999)
   - Extrair dados incrementais (últimas 24h)
   - Identificar novos usuários, transações e relacionamentos

2. **Transformação (Transform):**
   - Calcular métricas agregadas por afiliado
   - Processar estrutura MLM por níveis
   - Validar critérios CPA automaticamente
   - Gerar resumos periódicos

3. **Carregamento (Load):**
   - Atualizar tabelas do banco robusto
   - Manter histórico de alterações
   - Atualizar cache de metadados

#### **Frequência de Atualização:**
- **Dados críticos:** A cada 15 minutos
- **Resumos diários:** 1x por dia (00:30)
- **Resumos semanais/mensais:** 1x por semana/mês
- **Recálculo completo:** 1x por semana (domingo)

---


## 🏗️ ARQUITETURA DE MICROSSERVIÇOS OTIMIZADA

### 🎯 Estratégia de Microsserviços

**Princípio:** Cada microsserviço é responsável por um domínio específico com seu próprio banco de dados, garantindo **isolamento**, **escalabilidade** e **manutenibilidade**.

### 🔧 Componentes da Arquitetura

#### 1. **API Gateway Robusto** (`fature-api-gateway`)

**Responsabilidades Atuais:**
- ❌ Apenas health checks básicos

**Responsabilidades Propostas:**
- ✅ **Roteamento Inteligente** - Direcionar requisições para microsserviços
- ✅ **Autenticação JWT** - Validação de tokens e permissões
- ✅ **Rate Limiting** - Controle de taxa de requisições
- ✅ **Cache de Respostas** - Cache inteligente para consultas frequentes
- ✅ **Agregação de Dados** - Combinar dados de múltiplos serviços
- ✅ **Monitoramento** - Logs centralizados e métricas

**Tecnologia:** Node.js + Express + Redis (cache)

#### 2. **Affiliate Service** (`fature-affiliate-service`)

**Função:** Gerenciamento completo de afiliados e rede MLM

**Responsabilidades:**
- Gerenciar dados de afiliados
- Processar estrutura MLM
- Calcular métricas de performance
- Gerar relatórios de rede
- Validar critérios CPA

**Banco de Dados:** **Banco Robusto de Afiliados** (PostgreSQL dedicado)

**APIs Principais:**
```
GET /api/v1/affiliates/{id}
GET /api/v1/affiliates/{id}/mlm-structure
GET /api/v1/affiliates/{id}/referrals
GET /api/v1/affiliates/{id}/financial-summary
POST /api/v1/affiliates/{id}/validate-cpa
```

#### 3. **Config Service** (`fature-config-service`)

**Função:** Configurações dinâmicas do sistema CPA

**Responsabilidades:**
- Valores CPA por nível
- Critérios de validação
- Regras de negócio
- Configurações de comissão

**Status:** ✅ **Implementado e Ativo**

#### 4. **MLM Service V2** (`fature-mlm-service-v2`)

**Função:** Processamento de distribuição MLM

**Responsabilidades:**
- Calcular distribuição de comissões
- Processar hierarquia MLM
- Integrar com Affiliate Service
- Aplicar regras de negócio

**Status:** ✅ **Implementado e Ativo**

#### 5. **Commission Service** (`fature-commission-service`)

**Função:** Cálculo e pagamento de comissões

**Responsabilidades:**
- Calcular comissões CPA/REV
- Processar pagamentos
- Gerar relatórios financeiros
- Integrar com sistema de pagamentos

**Status:** ✅ **Implementado e Ativo**

#### 6. **Data Service V2** (`fature-data-service-v2`)

**Função:** Analytics e sincronização de dados

**Responsabilidades:**
- **ETL do banco da operação**
- Gerar analytics avançados
- Exportar relatórios
- Sincronizar dados entre serviços

**Status:** ✅ **Implementado e Ativo**

### 🔄 Fluxo de Dados Otimizado

#### **Cenário: Consulta de Dashboard de Afiliado**

**Fluxo Atual (Problemático):**
```
Frontend → API Local → Banco Operação → Processamento Complexo → Resposta
Tempo: 5-15 segundos
```

**Fluxo Proposto (Otimizado):**
```
Frontend → API Gateway → Affiliate Service → Banco Robusto → Resposta
Tempo: 100-500ms
```

#### **Cenário: Validação de CPA**

**Fluxo Proposto:**
```
1. Data Service → Detecta nova transação no banco da operação
2. Data Service → Envia evento para Commission Service
3. Commission Service → Valida critérios via Config Service
4. Commission Service → Atualiza Affiliate Service
5. Affiliate Service → Atualiza banco robusto
6. MLM Service → Processa distribuição
```

### 📊 Benefícios da Arquitetura Proposta

#### **Performance:**
- **Consultas 10-50x mais rápidas** (dados pré-agregados)
- **Redução de 90% na carga** do banco da operação
- **Cache inteligente** no API Gateway
- **Escalabilidade independente** por serviço

#### **Manutenibilidade:**
- **Isolamento de responsabilidades**
- **Desenvolvimento paralelo** de equipes
- **Deploy independente** de serviços
- **Facilidade de debugging**

#### **Escalabilidade:**
- **Escalar apenas serviços necessários**
- **Bancos dedicados** por domínio
- **Load balancing** por serviço
- **Horizontal scaling** facilitado

---

## 🚀 PLANO DE IMPLEMENTAÇÃO

### 📅 Cronograma Sugerido

#### **Fase 1: Preparação (Semana 1-2)**
- [ ] Criar banco PostgreSQL para Affiliate Service
- [ ] Implementar estrutura do banco robusto
- [ ] Configurar ambiente de desenvolvimento
- [ ] Definir APIs do Affiliate Service

#### **Fase 2: Desenvolvimento Core (Semana 3-4)**
- [ ] Desenvolver Affiliate Service básico
- [ ] Implementar ETL inicial do Data Service
- [ ] Criar endpoints principais de consulta
- [ ] Testes de integração com banco da operação

#### **Fase 3: API Gateway (Semana 5-6)**
- [ ] Fortalecer fature-api-gateway
- [ ] Implementar roteamento inteligente
- [ ] Adicionar autenticação JWT
- [ ] Configurar cache Redis

#### **Fase 4: Integração (Semana 7-8)**
- [ ] Integrar todos os microsserviços
- [ ] Atualizar frontend para usar API Gateway
- [ ] Implementar monitoramento
- [ ] Testes de performance

#### **Fase 5: Go-Live (Semana 9-10)**
- [ ] Deploy em produção
- [ ] Migração gradual de dados
- [ ] Monitoramento intensivo
- [ ] Ajustes de performance

### 🔧 Requisitos Técnicos

#### **Infraestrutura:**
- **5 Bancos PostgreSQL** no Railway (1 novo para Affiliate Service)
- **Redis** para cache do API Gateway
- **Monitoramento** com logs centralizados
- **Backup automático** dos bancos

#### **Desenvolvimento:**
- **Node.js 18+** para todos os serviços
- **TypeScript** para type safety
- **Express.js** para APIs
- **Prisma** para ORM
- **Jest** para testes

#### **DevOps:**
- **Docker** para containerização
- **Railway** para deploy
- **GitHub Actions** para CI/CD
- **Monitoring** com métricas customizadas

---


## 📊 ESPECIFICAÇÕES TÉCNICAS DETALHADAS

### 🗄️ Estrutura do Banco Robusto

#### **Tabelas Principais:**

1. **`affiliates`** - 48,261 registros estimados
   - Dados cadastrais + métricas agregadas
   - Índices: status, performance, datas
   - Atualização: Tempo real via ETL

2. **`affiliate_mlm_structure`** - 48,261 registros
   - Estrutura MLM pré-calculada
   - 30 campos por afiliado (6 níveis x 5 métricas)
   - Recálculo: Diário

3. **`affiliate_referrals`** - ~614,944 registros estimados
   - Todas as indicações detalhadas
   - Histórico completo de validações
   - Crescimento: ~1,000 novos/dia

4. **`affiliate_financial_transactions`** - Milhões de registros
   - Todas as transações por afiliado/nível
   - Particionamento por data recomendado
   - Retenção: 2 anos online, arquivo histórico

5. **`affiliate_cpa_validations`** - ~50,000 registros estimados
   - Histórico de validações CPA
   - Critérios detalhados em JSON
   - Auditoria completa

#### **Views Otimizadas:**

```sql
-- Dashboard principal (consulta mais frequente)
CREATE VIEW v_affiliate_dashboard AS
SELECT 
    a.affiliate_id,
    a.name,
    a.total_validated_referrals,
    a.total_cpa_earned,
    m.total_network_size,
    m.level_1_count + m.level_2_count + m.level_3_count + 
    m.level_4_count + m.level_5_count as direct_network
FROM affiliates a
LEFT JOIN affiliate_mlm_structure m ON a.affiliate_id = m.affiliate_id
WHERE a.status = 'active';

-- Ranking de performance
CREATE VIEW v_affiliate_ranking AS
SELECT 
    affiliate_id,
    name,
    total_cpa_earned,
    RANK() OVER (ORDER BY total_cpa_earned DESC) as rank_cpa,
    RANK() OVER (ORDER BY total_validated_referrals DESC) as rank_referrals
FROM affiliates 
WHERE status = 'active';
```

### 🔄 Processo ETL Detalhado

#### **Extração (Extract):**
```python
# Pseudo-código do processo ETL
def extract_operation_data():
    # Conectar ao banco da operação
    conn = connect_to_operation_db()
    
    # Extrair dados incrementais
    new_users = extract_new_users(last_sync_time)
    new_transactions = extract_new_transactions(last_sync_time)
    new_bets = extract_new_bets(last_sync_time)
    
    return {
        'users': new_users,
        'transactions': new_transactions,
        'bets': new_bets,
        'timestamp': current_time()
    }
```

#### **Transformação (Transform):**
```python
def transform_affiliate_data(raw_data):
    # Processar cada afiliado
    for affiliate_id in get_active_affiliates():
        # Calcular métricas agregadas
        metrics = calculate_affiliate_metrics(affiliate_id, raw_data)
        
        # Processar estrutura MLM
        mlm_structure = calculate_mlm_structure(affiliate_id)
        
        # Validar novos CPAs
        new_cpas = validate_cpa_criteria(affiliate_id, raw_data)
        
        yield {
            'affiliate_id': affiliate_id,
            'metrics': metrics,
            'mlm_structure': mlm_structure,
            'new_cpas': new_cpas
        }
```

#### **Carregamento (Load):**
```python
def load_to_robust_db(transformed_data):
    # Atualizar em transação
    with robust_db.transaction():
        # Atualizar tabela principal
        update_affiliates_table(transformed_data['metrics'])
        
        # Atualizar estrutura MLM
        update_mlm_structure(transformed_data['mlm_structure'])
        
        # Inserir novas validações CPA
        insert_cpa_validations(transformed_data['new_cpas'])
        
        # Atualizar cache metadata
        update_cache_metadata(affiliate_id, current_time())
```

### 📈 Métricas de Performance Esperadas

#### **Antes (Situação Atual):**
- **Tempo de resposta:** 5-15 segundos
- **Consultas complexas:** 50-100 JOINs
- **Carga do banco:** 80-90% CPU
- **Disponibilidade:** 95% (timeouts frequentes)

#### **Depois (Com Banco Robusto):**
- **Tempo de resposta:** 100-500ms
- **Consultas simples:** 1-3 JOINs
- **Carga do banco:** 20-30% CPU
- **Disponibilidade:** 99.5%

#### **Métricas de Sucesso:**
- ✅ **Redução de 90%** no tempo de resposta
- ✅ **Redução de 80%** na carga do banco da operação
- ✅ **Aumento de 50%** na satisfação do usuário
- ✅ **Zero timeouts** em consultas de dashboard

---

## 🎯 BENEFÍCIOS ESTRATÉGICOS

### 💰 Benefícios Financeiros

#### **Redução de Custos:**
- **Infraestrutura:** Menor necessidade de recursos computacionais
- **Desenvolvimento:** Menos tempo gasto em otimizações
- **Manutenção:** Redução de bugs relacionados a performance
- **Suporte:** Menos tickets de lentidão do sistema

#### **Aumento de Receita:**
- **Experiência do usuário:** Interface mais rápida e responsiva
- **Novos recursos:** Facilidade para implementar funcionalidades
- **Escalabilidade:** Suporte a mais afiliados sem degradação
- **Analytics:** Relatórios mais precisos e rápidos

### 🚀 Benefícios Técnicos

#### **Performance:**
- Consultas 10-50x mais rápidas
- Redução drástica de timeouts
- Cache inteligente e eficiente
- Escalabilidade horizontal

#### **Manutenibilidade:**
- Código mais limpo e organizado
- Separação clara de responsabilidades
- Facilidade para debugging
- Testes mais eficientes

#### **Escalabilidade:**
- Crescimento independente de serviços
- Bancos especializados por domínio
- Load balancing eficiente
- Preparação para crescimento exponencial

### 👥 Benefícios para Equipe

#### **Desenvolvimento:**
- **Produtividade:** Menos tempo esperando consultas
- **Qualidade:** Foco em funcionalidades, não em performance
- **Inovação:** Facilidade para experimentar novas ideias
- **Satisfação:** Trabalhar com sistema responsivo

#### **Operações:**
- **Monitoramento:** Métricas claras por serviço
- **Debugging:** Isolamento de problemas
- **Deploy:** Atualizações independentes
- **Backup:** Estratégias específicas por domínio

---

## 🏁 CONCLUSÃO E PRÓXIMOS PASSOS

### 📋 Resumo da Estratégia

A implementação do **Banco de Dados Robusto para Afiliados** representa uma evolução fundamental na arquitetura do sistema Fature CPA. Esta estratégia combina:

1. **Otimização de Performance** através de dados pré-agregados
2. **Arquitetura de Microsserviços** escalável e manutenível
3. **Separação de Responsabilidades** clara entre componentes
4. **Preparação para Crescimento** exponencial do sistema

### 🎯 Objetivos Alcançados

- ✅ **Eliminação de processamento desnecessário**
- ✅ **Centralização de dados de afiliados**
- ✅ **Otimização da arquitetura de microsserviços**
- ✅ **Preparação para escalabilidade**
- ✅ **Melhoria da experiência do desenvolvedor**

### 🚀 Próximas Ações Recomendadas

#### **Imediato (Próximos 7 dias):**
1. **Aprovação da estratégia** pela equipe técnica
2. **Criação do banco PostgreSQL** para Affiliate Service
3. **Setup do ambiente** de desenvolvimento
4. **Definição da equipe** responsável

#### **Curto Prazo (Próximas 4 semanas):**
1. **Implementação do Affiliate Service**
2. **Desenvolvimento do ETL inicial**
3. **Fortalecimento do API Gateway**
4. **Testes de integração**

#### **Médio Prazo (Próximos 2 meses):**
1. **Deploy em produção**
2. **Migração gradual** dos dados
3. **Monitoramento e ajustes**
4. **Documentação completa**

### 📊 Indicadores de Sucesso

**Métricas Técnicas:**
- Tempo de resposta < 500ms
- Disponibilidade > 99.5%
- Redução de 90% na carga do banco da operação

**Métricas de Negócio:**
- Aumento na satisfação dos afiliados
- Redução de tickets de suporte
- Facilidade para implementar novas funcionalidades

**Métricas de Equipe:**
- Redução do tempo de desenvolvimento
- Melhoria na qualidade do código
- Aumento na produtividade

---

## 📚 ANEXOS

### 📄 Documentos Relacionados
- `fature_robust_database_structure.sql` - Estrutura completa do banco
- Documento de credenciais do sistema
- Especificações dos microsserviços CPA

### 🔗 Repositórios Relevantes
- `fature-affiliate-service` - Serviço principal de afiliados
- `fature-api-gateway` - Gateway a ser fortalecido
- `fature-config-service` - Configurações dinâmicas
- `fature-mlm-service-v2` - Processamento MLM
- `fature-commission-service` - Cálculo de comissões
- `fature-data-service-v2` - Analytics e ETL

### 🛠️ Ferramentas e Tecnologias
- **PostgreSQL** - Banco de dados principal
- **Redis** - Cache do API Gateway
- **Node.js + TypeScript** - Runtime e linguagem
- **Express.js** - Framework web
- **Prisma** - ORM
- **Railway** - Plataforma de deploy

---

**Documento preparado em:** 24 de junho de 2025  
**Versão:** 1.0  
**Status:** ✅ Pronto para Implementação

**Próxima revisão:** Após implementação da Fase 1

