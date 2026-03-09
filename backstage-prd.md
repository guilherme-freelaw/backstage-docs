# BACKSTAGE FREELAW — Super PRD v3

> O QUE construir e POR QUÊ.
> Toda decisão, construção e priorização deve estar ancorada aqui.
> Se não está neste documento, não existe.
>
> Para COMO e QUANDO construir, consulte o **Plano de Trabalho**.
> Para o estado ideal de cada miniapp, consulte o **Discovery**.

**Última atualização**: 2026-03-09
**Owner**: Guilherme (VDC), Diretor Financeiro
**Time**: Alex, Biel, Clarinha, Davi
**Prazo hard**: Fim de Abril 2026
**URL produção**: backstage.freelaw.ai

---

## 1. CONTEXTO ESTRATÉGICO

### 1.1 A tese da Freelaw

A Freelaw é uma legaltech B2B SaaS para escritórios de advocacia brasileiros (2-15 advogados). O produto principal — o **Studio** — concentra 25+ funcionalidades jurídicas numa plataforma única: publicações, prazos, tarefas, documentos, processos, cálculos, consultas, CRM, IA.

A **Onda Zero** (2026) é uma refundação da empresa. A tese: o mercado jurídico de PMEs no Brasil é enorme, fragmentado, e mal atendido por ferramentas genéricas. A Freelaw quer ser o sistema operacional do escritório de advocacia — e para isso precisa de uma operação interna que funcione tão bem quanto o produto que vende.

### 1.2 O que é o Backstage

O Backstage é a **plataforma interna da Freelaw**. Enquanto o Studio Offices atende o contratante e o Studio Providers atende o prestador, o Backstage conecta todas as pontas internas:

```
STUDIO OFFICES (cliente)     BACKSTAGE (operação)      STUDIO PROVIDERS (prestador)
       ↕                           ↕                           ↕
  Usa o produto    ←→    Vende, monitora,     ←→    Executa serviços
                         cobra, suporta,
                         retém, expande
```

O Backstage centraliza **gestão comercial** (vendas, CRM, pipeline), **gestão financeira** (receita, custos, DRE, billing), **gestão de clientes** (CS, health score, NPS, churn), **mensageria** (WhatsApp, email), **automações** (workflows, cadências) e **inteligência** (dashboards, IA, analytics).

### 1.3 O problema: fragmentação operacional

Hoje a operação da Freelaw depende de:
- **HubSpot** para CRM e pipeline de vendas
- **Google Sheets** para business plan e planejamento financeiro
- **Ferramentas externas** para automações (Zapier, Make, scripts avulsos)
- **Processos manuais** para acompanhamento de clientes

Isso gera: dados duplicados, decisões baseadas em informação incompleta, dependência de ferramentas caras, e incapacidade de automatizar fluxos end-to-end.

### 1.4 Princípio operacional

> "Isso ajuda a fechar o gate da Fase 0?" — Se não, vai para o backlog.

A Fase 0 tem 7 gates. Cada gate representa uma capacidade operacional que o Backstage precisa entregar. Só depois que os 7 gates estiverem fechados é que avançamos para a visão expandida (cadências IA, omni-channel, flywheel).

---

## 2. PERSONAS E JOBS-TO-BE-DONE

### 2.1 BDR / SDR (Aquisição de Clientes)

**Quem**: Time de prospecção outbound (BDR) e qualificação inbound (SDR).

**O que faz hoje**:
- BDR usa Lead Hunter para encontrar escritórios, prospecta por telefone/email/WhatsApp
- SDR recebe leads inbound (site, ads) e qualifica
- Ambos agendam demos para os Closers

**O que deveria fazer no Backstage**:
- Ver KPIs diários: leads trabalhados, demos agendadas, conversão
- Gerenciar lista de leads com score e priorização
- Executar cadências de email/WhatsApp (sequences) sem sair do sistema
- Ver metas vs realizado em tempo real

**Módulos**: `/aquisicao`, `/vendas/outbound`, `/crm/sequences`, `/vendas/leads`

**Dores atuais**: Outbound ainda depende do HubSpot para dedup e escrita. Sequences não têm motor de execução. KPIs diários estão vazios (falta binding de dados).

---

### 2.2 Closer (Vendas)

**Quem**: Vendedores seniores que fecham deals.

**O que faz hoje**:
- Recebe leads qualificados do SDR
- Conduz demo do produto
- Negocia contrato e fecha venda
- Acompanha comissão

**O que deveria fazer no Backstage**:
- Ver pipeline de deals por estágio (visualização kanban)
- Gerenciar deals: valor, plano, desconto, contrato assinado, pós-venda
- Acompanhar comissão (BDR 1%, Closer 5%) e forecast
- Ver analytics de performance: ciclo de vendas, win rate, ticket médio

**Módulos**: `/vendas/deals`, `/vendas/pipeline`, `/vendas/commission`, `/crm`

**Dores atuais**: Pipeline funciona mas métricas de Oitiva (qualidade de ligações) não estão integradas. Forecast de comissão precisa de validação.

---

### 2.3 CS Manager (Customer Success)

**Quem**: Gestores de sucesso do cliente. Responsáveis por retenção e expansão.

**O que faz hoje**:
- Monitora saúde dos clientes (health score)
- Identifica clientes em risco de churn
- Gerencia pipeline de renovações
- Conduz NPS e atua sobre detractors
- Faz onboarding de novos clientes

**O que deveria fazer no Backstage**:
- Ver dashboard com todos os clientes: score, trend, risk signals
- Receber alertas automáticos quando cliente fica critical
- Abrir ficha 360 de qualquer cliente (uso, saúde, interações, financeiro)
- Executar playbooks pré-definidos (onboarding, risco, recovery)
- Calcular e oferecer retention offers quando cliente quer cancelar

**Módulos**: `/cs`, `/suporte/cliente/[orgId]`, `/cs/health`, `/cs/renewals`, `/cs/playbooks`

**Dores atuais**: Health score existe com 5 componentes mas pesos não foram validados pelo CS. Playbooks estão vazios (0% implementado). Retention offers têm schema mas não estão conectadas. Timeline de interações mistura dados reais e mock.

---

### 2.4 Finance (Financeiro)

**Quem**: CFO, time financeiro. Responsáveis por receita, custos, planejamento.

**O que faz hoje**:
- Acompanha MRR, churn, inadimplência
- Gera DRE, balanço, DFC
- Monitora unit economics (CAC, LTV, payback)
- Planeja budget e business plan
- Reconcilia pagamentos (Iugu, Stripe, Conta Azul)

**O que deveria fazer no Backstage**:
- Ver tudo numa tela: receita, custos, MRR, churn, inadimplência
- Gerar relatórios sem planilhas externas
- Business plan editável na UI (sem Google Sheets)
- Conciliação automática entre gateways e contabilidade

**Módulos**: `/financeiro/*` (28 páginas, 30+ API routes)

**Dores atuais**: Módulo é o mais completo (95% do G1). Única dependência externa: Google Sheets para Business Plan. ERP (Freelaw One) é placeholder. Recebimentos e estruturas organizacionais são stubs.

---

### 2.5 Operações (Automações e Integrações)

**Quem**: Time técnico/operacional que mantém os sistemas rodando.

**O que faz hoje**:
- Configura e monitora integrações (14 sistemas externos)
- Cria e mantém automações (workflows, crons)
- Debugga falhas de sync
- Conecta novos sistemas

**O que deveria fazer no Backstage**:
- Ver dashboard de status de todas as integrações
- Monitorar automações: quais rodaram, quais falharam, logs
- Criar workflows via UI (trigger → condição → ação)
- Gerenciar templates de mensagem (WhatsApp, email)

**Módulos**: `/produto/workflows`, `/interno/whatsapp`, integrações

**Dores atuais**: Workflow builder tem tipos e executor definidos, mas execução é STUB. Ações de automação como SMS, webhook, create_deal estão vazias. Lead scoring tem schema mas sem processador. 25 cron jobs definidos, maioria funcional, mas sem monitoramento centralizado.

---

### 2.6 Liderança (Ju, VDC, Carol, Didico)

**Quem**: CEO (Ju), Diretor Financeiro (VDC/Guilherme), Gerente de CS (Carol), Gerente de Aquisição (Didico).

**O que faz hoje**:
- Toma decisões estratégicas baseadas em métricas
- Acompanha performance da empresa

**O que deveria fazer no Backstage**:
- Abrir UMA tela e ver: MRR, churn rate, novos clientes, receita, despesas, health score médio, pipeline de vendas
- Filtrar por período, plano, cohort, ICP
- Drill-down em qualquer métrica

**Módulos**: `/dashboard`, `/executive`

**Dores atuais**: Dashboard existe mas não unifica todas as métricas. Analytics estão espalhados por módulos diferentes (financeiro, vendas, CS). Falta visão executiva consolidada.

---

### 2.7 Produto / Dev

**Quem**: Time de produto e engenharia.

**O que faz hoje**:
- Gerencia design system, feature flags, audit logs
- Usa wiki e chat interno
- Configura AI agents

**O que deveria fazer no Backstage**:
- Gerenciar design system atualizado
- Consultar audit logs para debugging
- Configurar e monitorar AI agents
- Usar wiki como base de conhecimento interna

**Módulos**: `/produto/*` (112 páginas — maior módulo), `/wiki`, `/chat`

**Dores atuais**: Módulo funcional mas não é prioridade para Fase 0. Não bloqueia nenhum gate.

---

## 3. INVENTÁRIO DO QUE EXISTE

### 3.1 Números reais do código (mapeado 2026-03-09)

| Métrica | Valor |
|---------|-------|
| Pages (Next.js) | 299 |
| API Routes | 167+ |
| TSX Files | 431 |
| Database Tables (Backstage) | 40+ |
| Database Tables (Infra DB) | 80+ (em 80 schema files) |
| Feature Modules | 23 |
| Integrações externas | 13 |
| AI Agents | 4 |
| Cron Jobs | 25 |
| Edge Functions (Deno) | 58 |

**Conclusão**: O Backstage já tem MUITA coisa construída. O problema não é "construir do zero" — é **terminar, conectar e estabilizar** o que existe.

### 3.2 Mapa de módulos

| Módulo | Rota | Páginas | Status | Gate |
|--------|------|---------|--------|------|
| **Financeiro** | /financeiro/* | 28 | Funcional | G1, G5 |
| **Vendas** | /vendas/* | 31 | Funcional | G3 |
| **CRM** | /crm/* | 12 | Funcional | G3 |
| **Aquisição** | /aquisicao/* | 6 | Parcial (stubs) | G3 |
| **CS** | /cs/* | 9 | Parcial | G2 |
| **Suporte** | /suporte/* | 15 | Parcial | G2, G7 |
| **WhatsApp** | /interno/whatsapp | 12 | Parcial | G7 |
| **Prestadores** | /prestadores/* | 12 | Funcional | — |
| **Produto** | /produto/* | 112 | Funcional | — |
| **RH** | /rh/* | 18 | Parcial | — |
| **Chat** | /chat | 4 | Funcional | — |
| **Wiki** | /wiki | 5 | Funcional | — |
| **Oitiva** | /oitiva/* | 8 | Em dev | — |
| **Marketing** | /marketing/* | 4 | Parcial | — |
| **Planejamento** | /planejamento | — | Parcial | — |
| **Dashboard** | /dashboard | — | Funcional | G5 |

### 3.3 Integrações externas (13)

| Integração | O que faz | Direção | Status | Gate |
|------------|-----------|---------|--------|------|
| **HubSpot** | CRM legado (16 entidades: contacts, companies, deals, calls, meetings, etc.) | Bidirecional → migrando para read-only | Ativo — em deprecação | G3 |
| **Stripe** | Billing para clientes novos. Webhook de eventos. | Inbound (webhooks) | Ativo — primário | G1 |
| **Iugu** | Billing legado. Sync via ETL (customers, subscriptions, invoices, financials). | Sync periódico | Ativo — sync estável | G1, G6 |
| **Meta Ads** | Marketing analytics. Dados de campanhas e CAPI. | Inbound | Ativo | — |
| **Google Ads** | Marketing analytics. Dados de campanhas. | Inbound | Ativo | — |
| **Conta Azul** | Contabilidade. Sync de despesas e categorias. | Sync periódico | Ativo | G1, G6 |
| **Evolution** | WhatsApp Business API. Envio/recebimento de mensagens. | Bidirecional | Ativo | G7 |
| **Blip** | Chatbot para atendimento automatizado. | Bidirecional | Ativo | G7 |
| **Banco Inter** | Banking (mTLS + OAuth). Transações e reconciliação. | Inbound | Pronto, inativo | G6 |
| **Notion** | Documentação. Sync de conteúdo. | Inbound | Ativo | — |
| **Slack** | Comunicação interna. Notificações e alertas. | Outbound | Ativo | G2 |
| **Golden Record** | Entity resolution. Unificação de registros duplicados. | Bidirecional | Ativo | G3 |
| **Clicksign** | Assinatura digital de contratos. | Bidirecional | Ativo | G3 |

### 3.4 AI Agents (Mastra)

| Agent | Função | Status |
|-------|--------|--------|
| **CEO IA** | Insights executivos sobre métricas da empresa. Responde perguntas estratégicas. | Ativo |
| **Maria Insights** | Análise financeira. Interpreta DRE, MRR, churn. | Ativo |
| **Oitiva Evaluation** | Avalia qualidade de ligações de vendas via transcrições. Scorecards com critérios ponderados. | Em desenvolvimento |
| **Backstage Assistant** | Assistente geral do Backstage. Ajuda a navegar e encontrar informação. | Ativo |

### 3.5 Domínios no backstage-core

| Domínio | Entidades principais |
|---------|---------------------|
| **Finance** | Subscriptions (Stripe+Iugu), MRR calculation, DRE, Budget, Cashflow, Unit Economics (CAC, LTV, payback) |
| **Leads** | Lead Hunter (prospecção B2B com IA), filtros por localidade, Google Places enrichment, Exa data enrichment |
| **Comunidade** | Provider funnel, tiers (S+ a D), journey phases, saturação, ranking |
| **Retention** | Churn risk signals, health score (5 componentes), risk levels, trends, retention offers |
| **Marketing** | Ad campaigns (Meta/Google), content analytics, channel performance, CAPI integration |

---

## 4. MACRO-ÁREAS DO BACKSTAGE

> Cada macro-área é um domínio funcional do Backstage com escopo, responsável e PRD próprio.
> O responsável pelo PRD de cada área detalha user stories, critérios de aceite e dependências.
> Esta seção é o **mapa de ownership** — quem é dono de quê.

### Arquitetura: CRM como Operating System

> **Decisão arquitetural**: O CRM **não é um módulo isolado**. Ele é uma camada de infraestrutura
> (package) que roda **por dentro** de múltiplas áreas. Marketing + Vendas = CRM, mas a mesma
> estrutura (contacts, companies, deals, activities, pipelines) é reutilizada para outros domínios:
> pipeline de gestão de prestadores, pipeline de CS, etc.
>
> Essa lógica de **packages transversais** se aplica a outras conexões entre áreas:
> - **Financeiro ↔ Prestadores**: pagamento a prestadores alimenta custos e DRE
> - **Financeiro ↔ CS**: recebimentos ligados a health score e inadimplência
> - **Financeiro ↔ Billing**: faturamento → receita → MRR → business plan
> - **Vendas ↔ CS**: deal fechado → onboarding → health score → renovação
> - **Aquisição ↔ Vendas**: lead qualificado → deal no pipeline
> - **Mensageria ↔ Todos**: WhatsApp/email como canal transversal
>
> **Status**: Mapeamento completo de conexões cross-área **PENDENTE** (ver D14).

### Mapa de Atribuição

| # | Macro-Área | Tipo | Módulos | Páginas | Gates | Responsável PRD | Status PRD |
|---|------------|------|---------|---------|-------|-----------------|------------|
| **PACKAGES (infra transversal)** ||||||
| MA-CRM | CRM (Operating System) | Package | Transversal | — | G3 | _a definir_ | Não iniciado |
| MA7 | Integrações | Package | Transversal (13 sistemas) | — | G6 | _a definir_ | Não iniciado |
| MA6 | Automações & Workflows | Package | /produto/workflows, crons, edge functions | — | G4 | _a definir_ | Não iniciado |
| MA5 | Mensageria & Comunicação | Package | /interno/whatsapp, /suporte/inbox, /chat | 31 | G7 | _a definir_ | Não iniciado |
| **ÁREAS DE NEGÓCIO (consomem os packages)** ||||||
| MA1 | Financeiro & Billing | Área | /financeiro/* | 28 | G1, G5 | _a definir_ | Não iniciado |
| MA2 | Vendas & Pipeline | Área | /vendas/*, /oitiva/* | 39 | G3 | _a definir_ | Não iniciado |
| MA3 | Aquisição & Outbound | Área | /aquisicao/*, /vendas/outbound | 6 | G3 | _a definir_ | Não iniciado |
| MA4 | Customer Success | Área | /cs/*, /suporte/cliente/* | 24 | G2 | _a definir_ | Não iniciado |
| MA11 | Prestadores & Comunidade | Área | /prestadores/* | 12 | — | _a definir_ | Não iniciado |
| MA10 | Marketing | Área | /marketing/* | 4 | — | _a definir_ | Não iniciado |
| MA9 | RH & Produtividade | Área | /rh/* | 18 | — | _a definir_ | Não iniciado |
| **SUPORTE** ||||||
| MA8 | Dashboard & Analytics Executivo | Suporte | /dashboard, /executive | — | G5 | _a definir_ | Não iniciado |
| MA12 | Produto & Plataforma | Suporte | /produto/*, /wiki, /chat | 121 | — | _a definir_ | Não iniciado |

### Conexões Cross-Área (a mapear)

> **ATENÇÃO**: As conexões abaixo são o mapeamento inicial. Cada conexão precisa ser
> detalhada com: dados que fluem, direção, triggers, e quem é owner do fluxo.
> Este mapeamento é pré-requisito para construir os PRDs das macro-áreas.

```
                    ┌─────────────────────────────────────────────┐
                    │         CRM (Operating System)              │
                    │  contacts · companies · deals · pipelines   │
                    │  activities · sequences · scoring           │
                    └──────┬──────────┬──────────┬────────────────┘
                           │          │          │
              ┌────────────┤          │          ├────────────┐
              ▼            ▼          ▼          ▼            ▼
         ┌─────────┐ ┌─────────┐ ┌────────┐ ┌──────────┐ ┌──────────┐
         │ Vendas  │ │Marketing│ │  CS    │ │Prestador.│ │Aquisição │
         │  (MA2)  │ │ (MA10)  │ │ (MA4)  │ │  (MA11)  │ │  (MA3)   │
         └────┬────┘ └─────────┘ └───┬────┘ └─────┬────┘ └──────────┘
              │                      │             │
              ▼                      ▼             ▼
         ┌───────────────────────────────────────────────┐
         │              Financeiro (MA1)                 │
         │  receita · custos · billing · recebimentos    │
         │  pagto prestadores · DRE · business plan      │
         └───────────────────────────────────────────────┘
```

| Conexão | De → Para | Dados que fluem | Status Mapeamento |
|---------|-----------|-----------------|-------------------|
| Venda → Onboarding | MA2 → MA4 | Deal fechado dispara onboarding CS | A mapear |
| CS → Renovação | MA4 → MA2 | Health score alimenta pipeline renovação | A mapear |
| CS → Financeiro | MA4 → MA1 | Inadimplência, churn → impacto MRR | A mapear |
| Vendas → Financeiro | MA2 → MA1 | Deal fechado → faturamento → receita | A mapear |
| Prestadores → Financeiro | MA11 → MA1 | Pagamento a prestadores → custos/DRE | A mapear |
| Aquisição → Vendas | MA3 → MA2 | Lead qualificado → deal no pipeline CRM | A mapear |
| Marketing → Aquisição | MA10 → MA3 | Campanhas → leads inbound | A mapear |
| Financeiro → Dashboard | MA1 → MA8 | MRR, receita, despesas, unit economics | A mapear |
| CS → Dashboard | MA4 → MA8 | Health score médio, NPS, clientes em risco | A mapear |
| Mensageria → Todos | MA5 → * | WhatsApp/email como canal transversal | A mapear |

---

### MA1: Financeiro & Billing

**Escopo**: Toda gestão financeira — receita, custos, MRR, DRE, billing, conciliação, unit economics, business plan.

**Módulos**: `/financeiro/*` (28 páginas, 30+ API routes)

**O que existe**:
- Receita: overview, assinaturas, faturamento, billing, novo-MRR → FUNCIONAL
- Custos: despesas (sync Conta Azul), prestadores, nova-sede → FUNCIONAL
- Relatórios: DRE, balanço, DFC, KPIs, auditoria, relatório diário → FUNCIONAL
- Análise: churn, inadimplência, unit economics (CAC/LTV/payback), analytics → FUNCIONAL
- Planejamento: business plan com import/export Google Sheets → FUNCIONAL
- Schemas: v2_billing.* (24+ tabelas), sync.iugu_*, fin_estruturas, fin_centros_custo

**O que falta para Fase 0**:
- ERP (Freelaw One): placeholder — decisão pendente (D9)
- Recebimentos: STUB
- Estruturas organizacionais: STUB
- Eliminar Google Sheets como dependência para Business Plan (D10)

**Gates**: G1 (95%), G5 (parcial — dados financeiros alimentam dashboard)

---

### MA-CRM: CRM (Operating System)

**Escopo**: Camada de infraestrutura que fornece contacts, companies, deals, pipelines, activities, sequences e scoring para todas as áreas que precisam gerenciar relacionamentos.

**Módulos**: `/crm/*` (12 páginas) — mas a lógica roda como package consumido por MA2, MA3, MA4, MA10, MA11

**O que existe**:
- Entities: contacts, companies, deals, activities → FUNCIONAL
- Pipeline kanban com estágios configuráveis → FUNCIONAL
- Sequences: schema pronto, motor de execução stub
- Golden Record: entity resolution para dedup → FUNCIONAL
- Analytics: CRM analytics, churn cohort, form analysis → FUNCIONAL

**O que falta para Fase 0**:
- Desligar HubSpot como fonte de verdade (D4)
- Motor de execução de sequences funcional
- Dedup nativo sem HubSpot
- Configuração de pipelines por domínio (vendas, prestadores, CS)
- Documentar API de package para áreas consumidoras

**Gates**: G3 (70%) — **GATE MAIS CRÍTICO** (mudança de processo + código)

---

### MA2: Vendas & Pipeline

**Escopo**: Pipeline de deals, comissões, propostas, metas de vendas, oitiva (avaliação de ligações). **Consome o CRM package (MA-CRM)** para contacts, companies, deals.

**Módulos**: `/vendas/*` (31 páginas), `/oitiva/*` (8 páginas)

**O que existe**:
- Pipeline kanban com deals por estágio (via CRM package) → FUNCIONAL
- Propostas: draft → sent → viewed → accepted/rejected → FUNCIONAL
- Metas de vendas (sales_targets) por pessoa e período → FUNCIONAL
- Oitiva: scorecards com critérios ponderados, transcrições, avaliações → EM DEV
- Analytics: CRM analytics, churn cohort, form analysis, plan mix → FUNCIONAL
- Comissão: BDR 1%, Closer 5% → PARCIAL (forecast não validado)

**O que falta para Fase 0**:
- Desligar HubSpot como fonte de verdade (D4)
- Motor de execução de sequences (schema pronto, sem cron que processa e envia)
- Dedup nativo sem HubSpot (consultar v2_sales.companies.domain)
- Integrar métricas de Oitiva ao pipeline e scorecards
- Validar forecast de comissão com vendas
- Tracking de opens/clicks/replies (webhooks Resend)

**Gates**: G3 (70%) — **GATE MAIS CRÍTICO** (mudança de processo + código)

---

### MA3: Aquisição & Outbound

**Escopo**: Prospecção outbound (BDR), qualificação inbound (SDR), Lead Hunter, lead scoring, KPIs de aquisição.

**Módulos**: `/aquisicao/*` (6 páginas), `/vendas/outbound`, `/vendas/leads`

**O que existe**:
- Lead Hunter: prospecção B2B com Google Places + Exa enrichment → FUNCIONAL
- Lista de leads com filtros por localidade e segmento → FUNCIONAL
- KPIs diários de aquisição → PARCIAL (tela existe mas dados vazios)
- Leads com scoring schema (lead_scoring_rules) → SCHEMA PRONTO

**O que falta para Fase 0**:
- KPIs diários com data binding real (hoje mostra zeros)
- Lead scoring processor: cron que calcula scores baseado nas regras
- Cadências outbound funcionais (depende do motor de sequences de MA2)
- Dedup nativo sem HubSpot

**Gates**: G3 (parcial — depende de MA2 para sequences e dedup)

---

### MA4: Customer Success

**Escopo**: Health score, NPS, renovações, playbooks, retention offers, customer 360, journey, churn prediction.

**Módulos**: `/cs/*` (9 páginas), `/suporte/cliente/[orgId]` (15 páginas)

**O que existe**:
- Health Score: 5 componentes (usage, engagement, support, financial, advocacy) com thresholds → FUNCIONAL
- Customer 360 API: agrega 9 fontes (CRM, billing, health, support, WhatsApp, AI, onboarding, churn, NPS) → FUNCIONAL
- NPS: surveys + respostas + score (promoter/passive/detractor) → FUNCIONAL
- Renovações: pipeline (upcoming → in_negotiation → renewed/churned/downgraded/upgraded) → FUNCIONAL
- Journey: 7 estágios (onboarding → advocacy → churned) → FUNCIONAL
- Churn Predictions: risk score + fatores + ações recomendadas → FUNCIONAL
- Retention Offers: schema completo (discount_percentage, discount_fixed, upgrade_discount, free_months, custom) → NÃO CONECTADO

**O que falta para Fase 0**:
- **Playbooks CS: 0% implementado** — precisa de 3 mínimos (onboarding, cliente em risco, NPS detractor)
- Wiring de retention offers à UI (engine existe, falta conectar)
- Validar pesos do health score com time de CS (D3)
- Timeline de interações com dados 100% reais (hoje mistura real + mock)
- Dados de uso do Studio: depende time Migração (risco externo)

**Gates**: G2 (73%)

---

### MA5: Mensageria & Comunicação

**Escopo**: WhatsApp, email transacional, inbox unificado, templates, chat interno, classificação IA de mensagens.

**Módulos**: `/interno/whatsapp` (12 páginas), `/suporte/inbox`, `/chat` (4 páginas)

**O que existe**:
- WhatsApp via Evolution API: envio e recebimento bidireccional → FUNCIONAL
- Email via Resend: transacional com tracking completo → FUNCIONAL
- Inbox unificado em /suporte/inbox → FUNCIONAL (complexo, com assignedTo e departments)
- Chat interno com canais, DMs, threads → FUNCIONAL
- Chatbot via Blip para atendimento automatizado → FUNCIONAL
- AI Classification: classifica mensagens por intent (sales/support/cs/marketing) com confidence → FUNCIONAL
- Templates: notification_templates por canal (email, push, sms, in_app, whatsapp, slack) → PARCIAL

**O que falta para Fase 0**:
- Histórico consolidado por cliente cross-channel (conversas em v2_comms não linkadas ao Customer 360)
- Completar webhook de respostas WhatsApp
- UI de gerenciamento de templates validada com o time
- SMS: stub sem provedor (decisão: entra na Fase 0?)

**Gates**: G7 (85%)

---

### MA6: Automações & Workflows

**Escopo**: Workflow builder, marketing automations, lead scoring engine, cron jobs, edge functions, automações de processo.

**Módulos**: `/produto/workflows`, crons, 58 edge functions (Deno)

**O que existe**:
- Workflow Builder: tipos (trigger → condição → ação) e executor definidos → STUB (simula, não executa)
- Marketing Automations: CRUD + cron 5min + enrollment engine → PARCIAL
  - Funcionais: send_email (Resend), send_whatsapp (Evolution), set_contact_property, create_task
  - Stub: send_sms, create_deal, webhook, internal_notification
- 25 Cron Jobs: maioria funcional, mas sem monitoramento centralizado
- 58 Edge Functions (Deno): separadas do monorepo, rodando no Supabase

**O que falta para Fase 0**:
- Workflow execution real (substituir stub por execução de ações)
- Lead scoring processor: cron que calcula scores e dispara ações
- Completar 4 ações de automação (SMS, deal, webhook, notification)
- UI de monitoramento de automações (logs, falhas, re-runs)
- Inventário de automações externas (Zapier, Make) para migrar ou eliminar (D1)
- Monitoramento centralizado dos 25 crons (health dashboard)

**Gates**: G4 (60%)

---

### MA7: Integrações

**Escopo**: 13 integrações externas, sync engine, monitoramento de saúde, documentação técnica.

**Módulos**: Transversal — afeta todos os módulos

**O que existe**:
- **Iugu**: sync estável (customers, subscriptions, invoices, financials, receivables, plans)
- **Stripe**: webhooks funcionando (subscription + invoice events)
- **Conta Azul**: sync estável (despesas, categorias)
- **Banco Inter**: mTLS + OAuth implementado → pronto mas INATIVO (D12)
- **HubSpot**: sync ativo → em deprecação, migrando para G3
- **Evolution/Blip**: WhatsApp operando
- **Meta/Google Ads**: analytics funcionando, CAPI integrado
- **Golden Record**: entity resolution para dedup
- **Clicksign**: assinatura digital de contratos
- **Slack**: notificações outbound
- **Notion**: sync de documentação
- Sync engine com dual-write para v2_billing

**O que falta para Fase 0**:
- Monitoramento centralizado com alertas (Sentry/Slack)
- Documentação formal de cada integração (o que sincroniza, frequência, credenciais, fallback)
- SLA definido por sync
- Decisão sobre Banco Inter (D12)

**Gates**: G6 (85%)

---

### MA8: Dashboard & Analytics Executivo

**Escopo**: Dashboard unificado para liderança, métricas consolidadas, filtros transversais, visão executiva.

**Módulos**: `/dashboard` (existente), `/executive` (a criar)

**O que existe**:
- /dashboard: hub principal com links para todos os módulos → FUNCIONAL
- Analytics espalhados por módulo: financeiro (DRE, MRR, churn), vendas (pipeline, conversão), CS (health) → FUNCIONAL
- PostHog integrado para analytics de produto → FUNCIONAL
- Churn cohort, unit economics, form analysis → FUNCIONAL

**O que falta para Fase 0**:
- **UMA página /executive** que consolide: MRR, churn rate, novos clientes, receita, despesas, health score médio, pipeline
- Filtros transversais: período, plano, cohort, ICP
- Binding de dados em /aquisicao/kpis-diarios (hoje mostra zeros)
- Métricas de CS no dashboard (health score médio, clientes em risco, NPS)
- MetricaSnapshot: tabela para snapshots históricos + cron diário (para comparativos mês a mês)

**Gates**: G5 (90%)

---

### MA9: RH & Produtividade

**Escopo**: Gestão de pessoas, org chart, dashboards de produtividade do time de desenvolvimento, performance tracking.

**Módulos**: `/rh/*` (18 páginas)

**O que existe**:
- 18 páginas de RH parcialmente implementadas
- Org chart → PARCIAL
- Perfis de usuário (user_profiles) → FUNCIONAL

**O que falta para Fase 0**:
- **Dashboard de produtividade dos devs**:
  - Métricas de código: commits, PRs abertos/merged, code review time, lines changed
  - Velocity do time: story points, cycle time (PR aberto → merged → deployed)
  - Alocação por projeto/sprint: quem está trabalhando em quê
  - Integração com GitHub API para puxar métricas automaticamente
- Performance tracking por pessoa (reviews, goals, 1:1s)
- Org chart funcional com hierarquia e reportes
- Visão de capacidade do time (quem está disponível, quem está alocado)

**Gates**: Nenhum gate vinculado. **Decisão necessária**: elevar a gate ou manter como Fase 1?

> **Nota**: Este módulo tem 18 páginas construídas mas não foi incluído nos 7 gates originais.
> Se a liderança considerar os dashboards de produtividade críticos para Fase 0,
> deve ser elevado a G8 ou integrado ao G5 (Dashboard Executivo).

---

### MA10: Marketing

**Escopo**: Campanhas de ads, analytics de marketing, email marketing, newsletters, landing pages, CAPI.

**Módulos**: `/marketing/*` (4 páginas)

**O que existe**:
- Campanhas Meta/Google Ads com analytics → FUNCIONAL
- CAPI (Conversions API) para Meta e Google → FUNCIONAL
- Schema completo: campaigns, email_campaigns, landing_pages, utm_parameters, conversion_events, blog_posts, newsletters
- Newsletter subscribers e events → SCHEMA PRONTO, UI parcial

**O que falta para Fase 0**:
- UI de marketing pouco desenvolvida (apenas 4 páginas para schema extenso)
- Email marketing campaigns: schema existe mas sem UI de criação e envio
- Landing pages: schema sem builder visual
- Analytics de canal de aquisição consolidado

**Gates**: Nenhum gate vinculado. Candidato a Fase 1 (exceto analytics de aquisição que alimenta G5).

---

### MA11: Prestadores & Comunidade

**Escopo**: Gestão de prestadores de serviço jurídico, funnel de aquisição de prestadores, tiers de qualidade, ranking, saturação. **Consome o CRM package (MA-CRM)** para pipeline de gestão de prestadores.

**Módulos**: `/prestadores/*` (12 páginas)

**O que existe**:
- Provider funnel com tiers (S+ a D) e ranking → FUNCIONAL
- Journey phases de prestadores → FUNCIONAL
- Saturação por região/especialidade → FUNCIONAL
- Onboarding de prestadores → PARCIAL (depende HubSpot para parte do fluxo)

**O que falta para Fase 0**:
- Desvincular onboarding de prestadores do HubSpot (atrelado ao G3)
- Migrar pipeline de prestadores para CRM package (pipelines configuráveis por domínio)
- Validar se módulo opera de forma autônoma sem HubSpot

**Gates**: Parcialmente vinculado ao G3 (dependência HubSpot no onboarding + CRM package)

---

### MA12: Produto & Plataforma

**Escopo**: Design system, wiki/knowledge base, audit logs, feature flags, AI agents (Mastra), chatbots, infraestrutura técnica.

**Módulos**: `/produto/*` (112 páginas), `/wiki` (5 páginas)

**O que existe**:
- Design system completo → FUNCIONAL
- Wiki / knowledge base → FUNCIONAL
- AI Agents (Mastra): CEO IA, Maria Insights, Oitiva Evaluation, Backstage Assistant → FUNCIONAL
- Audit logs → FUNCIONAL
- Feature flags → FUNCIONAL
- Chatbots configuráveis → FUNCIONAL

**O que falta para Fase 0**:
- Nenhum bloqueio crítico — módulo funcional e mantido
- Melhorias são Fase 1+

**Gates**: Nenhum — plataforma de suporte transversal

---

## 5. OS 7 GATES — ESPECIFICAÇÃO DE PRODUTO

> Cada gate é um marco de capacidade que cruza uma ou mais macro-áreas (seção 4).
> Use a tabela abaixo para navegar entre gates e macro-áreas.

| Gate | Descrição | Macro-Áreas | Completude |
|------|-----------|-------------|------------|
| G1 | Gestão financeira centralizada | MA1 (Financeiro) | 95% |
| G2 | Acompanhar Cliente v1 | MA4 (Customer Success) | 73% |
| G3 | Pipeline e CRM unificados | MA2 (Vendas) + MA3 (Aquisição) + MA11 (Prestadores) | 70% |
| G4 | Automações internas | MA6 (Automações) | 60% |
| G5 | Dashboard unificado | MA8 (Dashboard) + MA1 (Financeiro) + MA4 (CS) | 90% |
| G6 | Integrações estabilizadas | MA7 (Integrações) | 85% |
| G7 | Mensageria no monorepo | MA5 (Mensageria) | 85% |

### 5.1 G1: Gestão financeira centralizada

**Declaração**: Toda operação financeira da Freelaw é consultável, analisável e planejável dentro do Backstage, sem dependência de planilhas externas.

**Completude estimada**: 95%

**User stories**:
- Como CFO, quero ver MRR atual, churn rate e inadimplência numa única tela, para saber a saúde financeira da empresa.
- Como Finance, quero gerar DRE mensal sem exportar para planilha, para fechar o mês mais rápido.
- Como Finance, quero editar o Business Plan dentro do Backstage, sem precisar do Google Sheets.
- Como Finance, quero que a conciliação Iugu vs contabilidade funcione automaticamente, para identificar divergências.

**Estado atual (evidência do código)**:
- **Receita**: overview, assinaturas, faturamento, billing, novo-MRR → FUNCIONAL
- **Custos**: despesas (sync Conta Azul), prestadores, nova-sede → FUNCIONAL
- **Relatórios**: DRE, balanço, DFC, KPIs, auditoria, relatório diário → FUNCIONAL
- **Análise**: churn, inadimplência, unit economics (CAC/LTV/payback), analytics → FUNCIONAL
- **Planejamento**: business plan com import/export Google Sheets, estratégico → FUNCIONAL
- **Configurações**: categorias (vendor_category_mapping), centros de custo (fin_centros_custo), estruturas (fin_estruturas) → FUNCIONAL
- **Conciliação**: Iugu vs GL → FUNCIONAL
- **Schemas de billing**: v2_billing com 24+ tabelas (customers, plans, subscriptions, invoices, invoice_items, ai_credits, subscription_events, retention_offer_configs, promotional_coupons, cancellation_tracking)
- **Sync financeiro**: sync.iugu_customers, sync.iugu_subscriptions, sync.iugu_invoices, sync.iugu_financial_transactions, sync.iugu_receivables

**Gaps**:
- ERP (Freelaw One) → PLACEHOLDER ("em construção") — decisão: entra na Fase 0?
- Recebimentos → STUB
- Estruturas organizacionais → STUB ("em breve")
- Google Sheets como dependência externa para Business Plan

**Critérios de aceite**:
- [ ] Toda operação financeira consultável no Backstage (nenhum dado só em planilha)
- [ ] Business Plan editável 100% na UI (import/export CSV como fallback)
- [ ] Sync Iugu + Conta Azul estável (7 dias sem falha)
- [ ] DRE, KPIs, Unit Economics com dados reais e atualizados
- [ ] Zero dependência de Google Sheets para dados financeiros

**Dependências técnicas**:
- Tabelas: v2_billing.*, sync.iugu_*, budgetCenarios, budgetDespesas, budgetMetas, mrrMovimentos, unitEconomicsSnapshots, vendor_category_mapping, fin_estruturas, fin_centros_custo
- APIs: /api/financeiro/* (30+ routes)
- Externas: Iugu (sync), Stripe (webhooks), Conta Azul (sync)

**Riscos**:
- Google Sheets elimination pode afetar workflow do CFO — validar antes de remover

---

### 5.2 G2: Acompanhar Cliente v1

**Declaração**: O time de CS consegue acompanhar a saúde de qualquer cliente ativo numa única tela, receber alertas quando algo piora, e agir com playbooks pré-definidos.

**Completude estimada**: 73%

**User stories**:
- Como CS Manager, quero ver o health score de todos os clientes numa lista ordenável, para priorizar quem precisa de atenção.
- Como CS Manager, quero receber alerta no Slack quando um cliente cai para "critical" (score < 40), para agir imediatamente.
- Como CSM, quero ver todas as interações (WhatsApp, email, suporte, NPS) de um cliente numa timeline unificada, para ter contexto antes de ligar.
- Como CS Manager, quero ativar um playbook de "cliente em risco" que cria tarefas automáticas para o CSM responsável.
- Como CSM, quero ver o pipeline de renovações com data, valor e probabilidade, para planejar meu mês.
- Como CS Manager, quero calcular retention offers automaticamente quando um cliente quer cancelar.

**Estado atual (evidência do código)**:
- **Health Score**: tabela crm.customer_health_scores com 5 componentes:
  - productUsageScore (0-100) — inputs: DAU, WAU, MAU, featureAdoption, lastLoginAt
  - engagementScore (0-100) — inputs: emailOpenRate, meetingsAttended, trainingCompleted
  - supportScore (0-100) — inputs: ticketsLast30Days, avgResolutionTime, csatScore
  - financialScore (0-100) — inputs: mrr, paymentStatus, invoicesOverdue
  - advocacyScore (0-100) — inputs: npsScore, referralsGiven, caseStudyParticipant
  - Status: healthy (≥70), at_risk (40-69), critical (<40)
  - Trend: improving | stable | declining
  - Risk signals: array de {type, severity (low/medium/high), description, detectedAt}
  - Opportunities: array de {type (upsell/cross_sell/expansion), product, estimatedValue, confidence}
- **Customer 360 API**: agrega 9 fontes — CRM, billing, health, support, WhatsApp, AI, onboarding, churn predictions, NPS
- **NPS**: surveys + respostas + score (promoter 9-10, passive 7-8, detractor 0-6)
- **Renovações**: pipeline com status (upcoming, in_negotiation, renewed, churned, downgraded, upgraded)
- **Journey**: 7 estágios (onboarding → adoption → value_realization → expansion → advocacy → at_risk → churned)
- **Churn Predictions**: predições com risk score, fatores, ações recomendadas → FUNCIONAL
- **Playbooks**: → VAZIO (0% implementado)
- **Retention Offers**: schema existe (retention_offer_configs com tipos: discount_percentage, discount_fixed, upgrade_discount, free_months, custom) mas NÃO conectado à UI

**Gaps**:
- Playbooks: 0% — precisa de pelo menos 3 para a Fase 0
- Retention offers: engine construída mas não wired
- Timeline de interações: mistura dados reais e mock
- Health score: pesos dos componentes não validados pelo CS
- Dados de uso do Studio: dependem do time de Migração

**Critérios de aceite**:
- [ ] /suporte/cliente/[orgId] carrega para qualquer cliente ativo com dados reais
- [ ] Health score calculado para 100% dos clientes ativos (cron rodando)
- [ ] Alerta automático no Slack quando score < 40
- [ ] Timeline de interações unificada (WhatsApp + email + suporte + NPS) com dados reais
- [ ] 3 playbooks CS operando (onboarding, cliente em risco, NPS detractor)
- [ ] Retention offers calculáveis via API

**Dependências técnicas**:
- Tabelas: crm.customer_health_scores, crm.nps_responses, crm.renewal_tracking, crm.customer_journey
- APIs: /api/customer-360/[id], /api/customer-success/health, /api/customer-success/retention-offers
- Externas: dados de uso do Studio (depende time Migração), Slack (alertas)

**Riscos**:
- Dados de uso do Studio podem não estar disponíveis a tempo (dep. Juju)
- Pesos do health score precisam ser validados com o CS antes de produzir
- Sem playbooks, o CS não tem ação padronizada para clientes em risco

---

### 5.3 G3: Pipeline e CRM unificados (matar HubSpot)

**Declaração**: Todo o funil de vendas — da prospecção ao fechamento — opera dentro do Backstage, com CRM nativo e zero dependência do HubSpot como fonte de verdade.

**Completude estimada**: 70%

**User stories**:
- Como Closer, quero gerenciar meu pipeline de deals no Backstage, sem precisar abrir o HubSpot.
- Como BDR, quero prospectar leads no outbound sem que o sistema precise do HubSpot para dedup.
- Como SDR, quero criar e executar cadências de email (sequences) nativamente no Backstage.
- Como VP Comercial, quero ver métricas de conversão por estágio do pipeline em tempo real.
- Como vendedor, quero ver o histórico completo de interações com um lead/empresa numa timeline.

**Estado atual (evidência do código)**:
- **CRM nativo (v2_sales)**: Schema completo com:
  - leads (status: new → contacted → qualified → unqualified → nurturing → converted → lost)
  - contacts (lifecycle: subscriber → lead → mql → sql → opportunity → customer → evangelist)
  - companies (type: prospect, customer, partner, competitor; enrichment via Google Places + Exa)
  - deals (status: open, won, lost; com pipeline_id e stage_id)
  - pipelines (types: sales, support, onboarding, provider_acquisition)
  - stages (com probability, daysInStage, isWon, isClosed)
  - activities (types: call, email, meeting, demo, task, note, follow_up)
  - proposals (status: draft → sent → viewed → accepted/rejected/expired)
  - sales_targets (metas por pessoa, time, período)
- **Sequences (v2_sales)**: Schema COMPLETO mas sem motor de execução:
  - sequences (status: draft, active, paused, archived; com enrollment limits)
  - sequence_steps (types: email, call, linkedin_connect, linkedin_message, whatsapp, sms, manual_task; com delay days/hours)
  - sequence_enrollments (status: active, paused, completed, replied, bounced, unsubscribed, opted_out, meeting_booked)
  - step_executions (status: pending → scheduled → sent → delivered → opened → clicked → replied → bounced → failed → skipped)
- **HubSpot sync**: Ainda ativo para:
  - Sync de dados entrando (contacts, companies, deals)
  - Outbound dedup (dedupHubspot() consulta HubSpot antes de criar)
  - Escrita de novos leads (hubspotWrite() envia para HubSpot)
  - Provider onboarding
  - hubspot_sync_log ainda em uso

**Gaps**:
- Motor de execução de sequences: schema completo mas **não existe cron que processa enrollments e envia emails**
- Outbound pipeline: dedup ainda usa HubSpot API
- Provider onboarding: depende de HubSpot
- Dados históricos do HubSpot: decisão pendente sobre migrar ou perder
- Tracking de opens/clicks/replies: schema existe (step_executions) mas sem webhook processing

**Critérios de aceite**:
- [ ] HubSpot sync desligado (cron desabilitado, HUBSPOT_ACCESS_TOKEN removido)
- [ ] Contacts, companies, deals criados nativamente em v2_sales
- [ ] Outbound pipeline funciona sem HubSpot (dedup consulta v2_sales.companies por domain)
- [ ] Provider onboarding funciona sem HubSpot
- [ ] Sequences: criar, enrollar, enviar email (via Resend), trackear opens/clicks/replies
- [ ] Pipeline de vendas 100% no Backstage (zero operação no HubSpot)

**Dependências técnicas**:
- Tabelas: v2_sales.leads, v2_sales.contacts, v2_sales.companies, v2_sales.deals, v2_sales.pipelines, v2_sales.stages, v2_sales.activities, v2_sales.sequences, v2_sales.sequence_steps, v2_sales.sequence_enrollments, v2_sales.step_executions
- APIs: /api/crm/*, /api/vendas/*, /api/crm/sequences
- Externas: Resend (envio de emails), Clicksign (assinatura de contratos), Golden Record (dedup)

**Riscos**:
- **GATE MAIS CRÍTICO**. Migração HubSpot é mudança de processo, não só de código.
- Dados históricos do HubSpot: se perder, perde contexto de vendas passadas
- Sequence engine precisa ser construída do zero (schema pronto, execução não)
- Time de vendas precisa ser treinado na nova ferramenta

---

### 5.4 G4: Automações internas

**Declaração**: Todas as automações críticas da operação rodam dentro do Backstage, com monitoramento, logs, e UI para gerenciamento.

**Completude estimada**: 60%

**User stories**:
- Como Operações, quero ver todas as automações rodando no Backstage com status e logs.
- Como Operações, quero criar automações simples (trigger → condição → ação) sem escrever código.
- Como Marketing, quero que leads que atingem certo score entrem automaticamente numa cadência.
- Como vendedor, quero que tarefas sejam criadas automaticamente quando um deal muda de estágio.

**Estado atual (evidência do código)**:
- **Workflow Builder**: tipos e executor definidos, mas execução é STUB (simula trabalho)
- **Marketing Automations**: CRUD + cron rodando a cada 5min + enrollment engine → PARCIAL
  - Ações funcionais: send_email (Resend), send_whatsapp (Evolution), set_contact_property, create_task
  - Ações STUB: send_sms, create_deal, webhook, internal_notification
- **Lead Scoring**: schema de regras (lead_scoring_rules) existe → SEM PROCESSADOR (nenhum cron calcula scores)
- **Cron Jobs**: 25 definidos → maioria funcional mas sem monitoramento centralizado
- **Edge Functions**: 58 Deno functions → separadas do monorepo

**Gaps**:
- Workflow execution é stub — não executa ações reais
- Lead scoring processor não existe
- 4 ações de automação são stub (SMS, deal, webhook, notification)
- Sem UI para monitorar automações (logs, falhas, re-runs)
- Inventário de automações externas (Zapier, Make) não feito
- Sem monitoramento centralizado dos 25 crons

**Critérios de aceite**:
- [ ] Marketing automations rodando (cron 5min, TODAS as ações funcionais ou com fallback logado)
- [ ] Lead scoring processor ativo (cron calcula scores baseado em regras)
- [ ] UI para listar automações, ativar/desativar, ver enrollments, ver logs
- [ ] Ferramentas externas de automação eliminadas (ou justificadas com plano de migração)
- [ ] Logs de execução acessíveis para debugging

**Dependências técnicas**:
- Tabelas: v2_sales.sequences (reusa para cadências), marketing_automations, lead_scoring_rules
- APIs: /api/automations/*, /api/cron/*
- Externas: Resend (email), Evolution (WhatsApp)

**Riscos**:
- Sem inventário de automações externas, podemos esquecer algo crítico
- Workflow builder visual pode ser escopo demais para Fase 0 — priorizar execução sobre UI de criação

---

### 5.5 G5: Dashboard unificado de métricas

**Declaração**: A liderança da Freelaw abre UMA tela no Backstage e vê todas as métricas-chave da empresa em tempo real.

**Completude estimada**: 90%

**User stories**:
- Como CEO, quero abrir o Backstage e ver MRR, churn, novos clientes, NPS, health score médio — tudo numa tela.
- Como Gerente de Aquisição, quero filtrar métricas por período, plano e cohort para entender tendências.
- Como Diretor Financeiro, quero ver receita vs despesas e unit economics atualizados.

**Estado atual (evidência do código)**:
- /dashboard: hub principal com links para módulos → FUNCIONAL
- /financeiro/analise/*: churn analytics, unit economics → FUNCIONAL
- /vendas/analytics/*: CRM analytics, churn cohort, form analysis, plan mix → FUNCIONAL
- /aquisicao/kpis-diarios: KPIs diários de aquisição → PARCIAL (dados vazios)
- PostHog integrado para analytics de produto

**Gaps**:
- Não existe UM dashboard executivo que consolide tudo
- Métricas estão espalhadas por módulos diferentes
- Aquisição KPIs mostrando zeros (falta binding de dados)
- Falta métricas de CS (health score médio, clientes em risco) no dashboard

**Critérios de aceite**:
- [ ] UMA página /executive com: MRR, churn rate, novos clientes, receita, despesas, health score médio, pipeline de vendas
- [ ] Filtros: período, plano, cohort, ICP
- [ ] Dados atualizados (real-time ou < 24h)
- [ ] Aprovado por Ju e VDC

**Dependências técnicas**:
- Tabelas: v2_billing.subscriptions (MRR), crm.customer_health_scores (health), v2_sales.deals (pipeline), v2_sales.leads (aquisição)
- APIs: /api/dashboard/*, /api/financeiro/*, /api/customer-success/*
- Depende: G1 (dados financeiros) e G2 (dados de CS) parcialmente prontos

**Riscos**:
- Baixo — maioria dos dados já existe, é questão de consolidar numa view

---

### 5.6 G6: Integrações estabilizadas

**Declaração**: Todas as 13 integrações externas estão monitoradas, documentadas, e operando sem falhas críticas.

**Completude estimada**: 85%

**User stories**:
- Como Operações, quero ver o status de cada integração (ok, falha, atrasada) num dashboard.
- Como Operações, quero receber alerta no Slack quando uma integração falha.
- Como Dev, quero documentação de cada integração (o que sincroniza, frequência, credenciais, fallback).

**Estado atual (evidência do código)**:
- **Iugu**: sync estável (customers, subscriptions, invoices, financial_transactions, receivables, plans)
- **Conta Azul**: sync estável (despesas, categorias)
- **Stripe**: webhooks funcionando (subscription events, invoice events)
- **Banco Inter**: mTLS + OAuth implementado, pronto mas INATIVO
- **HubSpot**: sync ativo mas em deprecação (migra para G3)
- **Evolution/Blip**: WhatsApp operando
- **Meta/Google Ads**: ads analytics funcionando
- Sync engine com dual-write para v2_billing

**Gaps**:
- Sem monitoramento centralizado com alertas
- Banco Inter inativo (decisão pendente)
- Sem documentação formal de cada integração
- Sem SLA definido para cada sync

**Critérios de aceite**:
- [ ] Auditoria completa das 13 integrações (status, última sync, erros)
- [ ] Monitoramento ativo com alertas Sentry/Slack para falhas
- [ ] Zero falhas críticas em 7 dias consecutivos
- [ ] Documentação de cada integração (o que sincroniza, frequência, credenciais, fallback)

**Dependências técnicas**:
- Tabelas: sync.* (todas as tabelas de sync externo)
- Infra: Sentry (monitoramento), Slack (alertas)

**Riscos**:
- Baixo — integrações já funcionam, falta formalizar monitoramento e documentação

---

### 5.7 G7: Mensageria no monorepo

**Declaração**: Toda comunicação com clientes e leads (WhatsApp, email) opera dentro do monorepo, com histórico consolidado por cliente e templates gerenciáveis.

**Completude estimada**: 85%

**User stories**:
- Como CSM, quero enviar WhatsApp para um cliente direto do Backstage, sem trocar de ferramenta.
- Como vendedor, quero que emails de cadências sejam enviados automaticamente pelo Backstage.
- Como CS Manager, quero ver TODAS as mensagens (WhatsApp + email + suporte) de um cliente numa timeline.
- Como Operações, quero gerenciar templates de mensagem (WhatsApp e email) pelo Backstage.

**Estado atual (evidência do código)**:
- **WhatsApp**: Evolution API para envio/recebimento → FUNCIONAL
  - v2_comms.conversations (com department, channel, status, assignedTo)
  - v2_comms.messages (com direction, senderType, content, attachments, readAt, deliveredAt)
  - v2_comms.message_identities (mapeia contatos cross-channel: phone_hash, email, whatsapp_jid)
- **Email**: Resend para envio transacional → FUNCIONAL (6+ endpoints)
  - v2_comms.email_logs (tracking completo com status history)
  - v2_comms.notification_templates (templates por canal: email, push, sms, in_app, whatsapp, slack)
- **Chat interno**: /chat com canais, DMs, threads → FUNCIONAL
- **Blip**: Chatbot para atendimento automatizado → FUNCIONAL
- **Inbox unificado**: /suporte/inbox → FUNCIONAL (complexo)
- **AI Classification**: v2_comms.ai_message_classification (classifica mensagens como sales/support/cs/marketing com confidence score)

**Gaps**:
- SMS: stub (sem provedor configurado)
- Histórico consolidado por cliente: conversas existem em v2_comms.conversations mas não estão linkadas ao Customer 360 de forma unificada
- Templates avançados: schema existe (notification_templates) mas UI de gerenciamento não validada
- Webhook de respostas WhatsApp: parcialmente implementado

**Critérios de aceite**:
- [ ] WhatsApp enviando e recebendo dentro do monorepo (Evolution API)
- [ ] Email transacional funcionando (Resend)
- [ ] Histórico consolidado por cliente: dado um cliente, listar TODAS as interações (WhatsApp + email + suporte) ordenadas por data
- [ ] Templates de mensagem gerenciáveis pelo time via UI
- [ ] Webhook de respostas (WhatsApp) processando e aparecendo no histórico

**Dependências técnicas**:
- Tabelas: v2_comms.conversations, v2_comms.messages, v2_comms.message_identities, v2_comms.notification_templates, v2_comms.email_logs, v2_comms.ai_message_classification
- APIs: /api/whatsapp/*, /api/email/*, /api/suporte/inbox
- Externas: Evolution (WhatsApp), Resend (email), Blip (chatbot)

**Riscos**:
- Evolution API: estabilidade e custo a longo prazo — decisão pendente sobre alternativas
- Unificação de histórico pode ser complexa (dados em tabelas diferentes com schemas diferentes)

---

## 6. ARQUITETURA DE DADOS

### 6.1 Mapa de schemas

O Backstage usa PostgreSQL via Drizzle ORM com estratégia multi-schema:

```
SCHEMAS BACKSTAGE (apps/backstage/src/db/schema/):
├── deals.ts           → daily_funnel_activity, daily_sales_person_activity, deals
├── sales-people.ts    → sales_people (BDR, SDR, Closer, canais)
├── goals.ts           → goals (metas mensais/trimestrais)
├── activities.ts      → métricas diárias de vendas
├── funnels.ts         → funnels (definição de funis)
├── financeiro.ts      → vendor_category_mapping, fin_estruturas, fin_centros_custo
│                        + re-exports: iugu*, v2Plans, v2Subscriptions, v2Invoices,
│                          budget*, despesas, mrrMovimentos, unitEconomicsSnapshots
├── hubspot.ts         → DEPRECATED (hubspot_owners, contacts, companies, deals, calls, meetings, sync_log)
├── chat.ts            → chat interno
├── chatbots.ts        → configuração chatbots
├── clicksign.ts       → assinatura digital
├── org-chart.ts       → hierarquia organizacional
├── planejamento.ts    → planejamento e forecast
├── reports.ts         → relatórios
├── support.ts         → tickets de suporte
├── user-profiles.ts   → perfis de usuário
├── users.ts           → autenticação
├── wiki.ts            → knowledge base
└── imports.ts         → histórico de imports

SCHEMAS INFRA-DB (packages/infra/db/src/schemas/v2/):
├── billing/           → v2_billing.* (24+ tabelas)
│   ├── customers, plans, subscriptions, invoices, invoice_items
│   ├── ai_credits, ai_credits_ledger
│   ├── subscription_events
│   ├── retention_offer_configs, promotional_coupons
│   ├── cancellation_tracking
│   └── ... (dispute, refund, collection, usage metrics)
│
├── sales/             → v2_sales.* (20+ tabelas)
│   ├── leads, contacts, companies, deals
│   ├── pipelines, stages
│   ├── activities, proposals, sales_targets
│   ├── sequences, sequence_steps, sequence_enrollments, step_executions
│   ├── scorecards, scorecard_criteria (Oitiva)
│   ├── transcriptions, evaluations, evaluation_scores, evaluation_aggregates
│   └── ...
│
├── marketing/         → v2_marketing.* (15+ tabelas)
│   ├── campaigns, email_campaigns, landing_pages
│   ├── utm_parameters, conversion_events, conversions_analytics
│   ├── capi_events, capi_platforms (Meta/Google CAPI)
│   ├── ad_accounts, ad_campaigns_v2, ad_creative, audience_syncs
│   └── blog_posts, newsletters, newsletter_subscribers, newsletter_events
│
├── comms/             → v2_comms.* (10+ tabelas)
│   ├── notification_templates, notifications
│   ├── email_logs
│   ├── conversations, messages, message_identities
│   ├── contacts (multi-channel)
│   └── ai_message_classification
│
├── customer-success/  → crm.* (5+ tabelas)
│   ├── customer_health_scores
│   ├── nps_responses
│   ├── renewal_tracking
│   ├── customer_journey
│   └── sales_sequences, sequence_steps, sequence_enrollments, sequence_step_executions
│
└── sync/              → sync.* (cópias de dados externos)
    ├── iugu_customers, iugu_subscriptions, iugu_invoices
    ├── iugu_financial_transactions, iugu_receivables, iugu_plans
    └── ...
```

### 6.2 Fluxos de dados entre sistemas

#### Fluxo de Aquisição → Venda → Cliente

```
LEAD HUNTER                    OUTBOUND/INBOUND              CLOSER
Google Places + Exa     →     BDR prospecta          →     Demo + Contrato
    ↓                              ↓                            ↓
v2_sales.companies            v2_sales.leads               v2_sales.deals
(enrichment data)             (status: new→contacted       (status: open→won)
                               →qualified→converted)            ↓
                                   ↓                      NOVO CLIENTE
                              v2_sales.contacts            v2_billing.customers
                              (lifecycle: lead→mql         v2_billing.subscriptions
                               →sql→opportunity            v2_billing.invoices
                               →customer)                       ↓
                                                          sync.iugu_*
                                                          (sync financeiro)
```

#### Fluxo de Health Score e Retenção

```
DADOS DE USO              DADOS FINANCEIROS           DADOS DE SUPORTE
(Studio - via Migração)   (v2_billing)                (v2_comms)
       ↓                        ↓                          ↓
productUsageScore          financialScore              supportScore
       ↓                        ↓                          ↓
       └────────────→ HEALTH SCORE (0-100) ←──────────────┘
                      crm.customer_health_scores
                              ↓
                     ┌────────┴────────┐
                     ↓                 ↓
              Score ≥ 70          Score < 40
              HEALTHY             CRITICAL
                                     ↓
                              Alerta Slack
                              Playbook acionado
                              Retention offer calculada
```

#### Fluxo HubSpot → Nativo (Migração G3)

```
HOJE (dependente):                    ALVO (nativo):
HubSpot API                           v2_sales.*
    ↓                                     ↑
hubspot_sync_log                     Criação direta
    ↓                                     ↑
hubspot_contacts → MIGRAR →         v2_sales.contacts
hubspot_companies → MIGRAR →        v2_sales.companies
hubspot_deals → MIGRAR →            v2_sales.deals
dedupHubspot() → SUBSTITUIR →       dedup por v2_sales.companies.domain
hubspotWrite() → SUBSTITUIR →       escrita direta em v2_sales
```

#### Fluxo de Sequences (Cadências de Email)

```
CRIAÇÃO:                    EXECUÇÃO:                   TRACKING:
POST /api/crm/sequences    Cron job (a construir)      Resend webhooks
    ↓                           ↓                           ↓
v2_sales.sequences         Busca enrollments com       step_executions
v2_sales.sequence_steps    nextStepAt <= now            status: sent→
    ↓                           ↓                       delivered→opened→
Enrollar contacts          Envia email via Resend       clicked→replied
    ↓                           ↓                           ↓
sequence_enrollments       Atualiza step_execution     Exit conditions:
(status: active)           (status: sent)               reply, meeting,
                                                        unsubscribe
```

### 6.3 Entidades que faltam para a visão do Gab

#### Cadência Unificada (expandir v2_sales.sequences)
O schema de sequences já suporta multi-canal (stepType: email, call, linkedin, whatsapp, sms, manual_task). O que falta é o **motor de execução** e a **integração com WhatsApp/SMS** além de email.

#### Interação Unificada
A tabela v2_comms.conversations + messages já serve como base. Falta:
- Linkagem consistente com v2_sales.contacts e v2_sales.companies
- Consolidação cross-channel via v2_comms.message_identities
- API unificada: GET /api/customer-360/{id}/interactions

#### Cliente Visão 360 (VIEW)
O endpoint /api/customer-360 já agrega 9 fontes. Falta:
- Performance (latência de agregação com muitas fontes)
- Dados de uso do Studio (depende Migração)
- Cohortização (segmentar por plano, período de aquisição, ICP)

#### MetricaSnapshot
Não existe tabela dedicada para snapshots de métricas históricas. Hoje cada módulo calcula em real-time. Para dashboard executivo com comparativos, pode ser necessário:
- Tabela metric_snapshots com tipo, período, valor, segmentação
- Cron job diário que snapshota MRR, churn, health score médio, etc.

### 6.4 RLS e Segurança

**Padrão**: Row Level Security no Supabase por organization_id.

```sql
-- Padrão para tabelas com organizationId
pgPolicy("table_select", {
  using: sql`(organization_id = user_organization_id())`
})

-- Para tabelas filho (herda do pai)
pgPolicy("child_select", {
  using: sql`EXISTS (SELECT 1 FROM parent WHERE parent.id = child.parent_id AND parent.org = user_org())`
})

-- Para dados financeiros (restrito a finance team)
pgPolicy("finance_only", {
  using: sql`(sync.is_finance_team(current_user_id()))`
})
```

**Problema**: ~167 API routes sem autenticação. Estratégia: corrigir nas routes que tocamos para fechar os gates.

---

## 7. VISÃO PÓS-FASE 0 (Ju)

> Esta seção documenta a visão estratégica da CEO (Ju) para o Backstage APÓS a Fase 0.
> Nenhum item desta seção entra no escopo atual. São pré-requisitos futuros que
> a Fase 0 deve habilitar.

### 7.1 Dados unificados com cohortização

**Visão**: Qualquer métrica da empresa pode ser segmentada por cohort (mês de aquisição), plano, ICP, canal de aquisição. Exemplo: "MRR dos clientes que entraram em janeiro no plano Professional via inbound."

**O que existe**: Churn cohort em /vendas/analytics. Filtros em vários módulos mas não transversais.

**O que a Fase 0 habilita**: Customer 360 (G2) + dashboard unificado (G5) + CRM nativo (G3) criam a base de dados necessária. Cohortização transversal é Fase 1.

### 7.2 Cadências automáticas omni-channel

**Visão**: Cadências de vendas e CS que combinam email + WhatsApp + ligação, operadas por humanos OU por IA, com comparativos de performance.

**O que existe**: Schema de sequences com suporte multi-canal. Evolution API para WhatsApp. Resend para email.

**O que a Fase 0 habilita**: G3 (sequences com motor de execução) + G7 (mensageria consolidada) criam a infraestrutura. IA para cadências é Fase 1+.

### 7.3 IA por área

**Visão**: Cada área da empresa tem IA aplicada: coaching de SDRs/Closers com comparativo humano vs IA, pipeline de conteúdo marketing com IA, health score inteligente com tratativa automatizada.

**O que existe**: 4 AI agents (Mastra), Oitiva para avaliação de ligações, ChurnRiskSignal.

**O que a Fase 0 habilita**: G2 (health score funcionando) + G4 (automações) + Oitiva (avaliações) criam os dados necessários. IA coaching e comparativos são Fase 1+.

### 7.4 Flywheel operacional

**Visão**: O Backstage como flywheel — dados de uso alimentam health score, health score aciona CS, CS melhora retenção, retenção aumenta LTV, LTV justifica mais investimento em aquisição.

**Pré-requisitos da Fase 0**: Todos os 7 gates fechados. Dados fluindo end-to-end. Sem fragmentação.

---

## 8. DÉBITO TÉCNICO

### 8.1 Inventário

**Segurança (P0)**:
- ~167 API routes sem autenticação — CRÍTICO
- Vulnerabilidades CSRF em OAuth flows
- Sem rate limiting
- 70 arquivos com @ts-nocheck

**Qualidade de código**:
- 92 instâncias de z.any() ou `as any`
- 83 console.log (deveria usar logger estruturado)
- 30 páginas sem error handling
- 27 páginas sem loading states
- 13 páginas sem empty states

**UX**:
- Aquisição KPIs mostrando zeros (falta data binding)
- Módulos com dados mock misturados com dados reais
- Playbooks CS vazio

### 8.2 Estratégia

> **NÃO** tratar débito técnico de forma horizontal na Fase 0.
> Apenas corrigir nos módulos que estamos tocando para fechar os gates.
> Débito horizontal fica para Fase 1.

Exceção: se uma route sem auth for pública e expor dados sensíveis, corrigir imediatamente.

### 8.3 Routes sem auth — priorização

A priorização deve ser feita pelo time na Sprint 1 (decisão D6). Critério sugerido:
1. Routes públicas que expõem dados de clientes → P0
2. Routes que escrevem dados (POST/PUT/DELETE) sem auth → P1
3. Routes internas de leitura → P2 (pode esperar)

---

## 9. DECISÕES PENDENTES

| # | Decisão | Responsável | Deadline | Status |
|---|---------|-------------|----------|--------|
| D1 | Quais ferramentas externas eliminar (lista completa) | Time | Sprint 1 | Pendente |
| D2 | Prioridade dos gates (ordem proposta OK?) | VDC + Ju | Sprint 1 | Pendente |
| D3 | Health score: quais métricas e pesos? CS valida? | CS + Produto | Sprint 2 | Pendente |
| D4 | HubSpot: data-alvo de desligamento | Vendas + Backstage | Sprint 1 | Pendente |
| D5 | Stack de mensageria final (Evolution API é definitivo?) | Time | Sprint 1 | Pendente |
| D6 | 167 routes sem auth: quais priorizar? | Guilherme + Time | Sprint 1 | Pendente |
| D7 | Stakeholders para aprovação de UX de cada gate | Ju | Sprint 1 | Pendente |
| D8 | Atribuição final do time (quem faz o quê) | Guilherme | Sprint 1 | Pendente |
| D9 | ERP (Freelaw One): entra na Fase 0? | VDC | Sprint 1 | Pendente |
| D10 | Google Sheets: eliminar ou conviver na Fase 0? | Finance + Guilherme | Sprint 1 | Pendente |
| D11 | Dados históricos do HubSpot: migrar ou perder? | Vendas + Biel | Sprint 1 | Pendente |
| D12 | Banco Inter sync: ativar na Fase 0? | Finance | Sprint 2 | Pendente |
| D13 | Workflow builder visual: Fase 0 ou Fase 1? | VDC | Sprint 1 | Pendente |
| D14 | Mapeamento completo de conexões cross-área (dados, direção, triggers, owners) | VDC + Time | Sprint 1 | Pendente |
| D15 | CRM package: quais pipelines configuráveis por domínio (vendas, prestadores, CS)? | VDC + Time | Sprint 1 | Pendente |

---

## 10. MÉTRICAS DE SUCESSO

### 10.1 Fase 0 (quantitativas)

- [ ] 7/7 gates fechados e aprovados por VDC + Ju
- [ ] Zero dependência de CRM externo como fonte de verdade
- [ ] 100% dos dados de clientes consultáveis no Backstage
- [ ] Health score calculado para 100% dos clientes ativos
- [ ] Dashboard executivo operando (aprovado pela liderança)
- [ ] HubSpot sync desligado
- [ ] Zero falhas críticas de integração em 7 dias
- [ ] 3 playbooks CS operando

### 10.2 Norte pós-Fase 0 (qualitativas)

- Cadências automáticas (email + WhatsApp + ligação) operando
- Sistema omni-channel funcional com timeline unificada
- IA para coaching de SDRs/Closers com comparativo humano vs IA
- Pipeline de conteúdo marketing com IA
- Health score inteligente com tratativa automatizada
- Cohortização transversal de qualquer métrica
- Flywheel operacional rodando

---

## 11. GOVERNANÇA

### Regra de ouro

> Se alguém do time quer fazer algo que não está neste documento, a resposta é:
> "Adiciona no PRD, aprova com o owner, e aí a gente faz."

### Regras de desenvolvimento (do AGENTS.md)

- Workflow obrigatório: PLAN → CLARIFY → BUILD → VALIDATE → PRESENT → SHIP
- PRs ≤ 500 linhas
- Screenshots obrigatórios para mudanças de UI
- Bun como runtime (nunca npm/yarn/pnpm)
- Sem @ts-nocheck ou `as any` novo
- Auth (RLS) obrigatório em novas rotas
- Conventional commits
- Zero construção no legado — toda energia no monorepo

---

## CHANGELOG

| Data | Versão | Alteração | Autor |
|------|--------|-----------|-------|
| 2026-03-09 | v1 | Documento criado | Guilherme + Claude |
| 2026-03-09 | v2 | Atualizado com mapeamento real do código | Guilherme + Claude |
| 2026-03-09 | v3 | Reescrita completa: personas, user stories, arquitetura profunda, separação PRD/Plano | Guilherme + Claude |
| 2026-03-09 | v4 | Macro-áreas com ownership, RH & Produtividade, gates orientados, remoção Omie, correção papel VDC | Guilherme + Claude |
| 2026-03-09 | v5 | CRM como OS (package transversal), liderança corrigida (Ju CEO, Carol CS, Didico Aquisição), conexões cross-área, D14/D15 | Guilherme + Claude |
