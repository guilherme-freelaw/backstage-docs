# BACKSTAGE FREELAW — Super PRD v7

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

> **ATENÇÃO**: Todos os elementos marcados como "FUNCIONAL" neste PRD precisam ser verificados
> com testes em produção. Status funcional sem validação em produção é hipótese, não fato.
> Antes de fechar qualquer gate, o PO deve testar cada critério de aceite em produção.

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

A visão da Freelaw é ser uma **empresa powered by AI** — a CEO da Freelaw é uma IA. Humanos representam a IA por enquanto. Cada pessoa que abre o Backstage encontra um **AI Cockpit** personalizado para o seu papel: o que é mais importante para ela naquele dia, quais decisões tomar, quais compromissos cobrar. O **Management OS** é o sistema operacional da gestão em si — pulses, weekly reports, combinados, e a IA que orquestra tudo.

### 1.3 O problema: fragmentação operacional

Hoje a operação da Freelaw depende de:
- **HubSpot** para CRM e pipeline de vendas
- **Google Sheets** para business plan e planejamento financeiro
- **Conta Azul** para contabilidade
- **Notion** para documentação e knowledge base
- **Slack** para comunicação interna
- **Ferramentas externas** para automações (Zapier, Make, scripts avulsos)
- **Processos manuais** para acompanhamento de clientes
- **gestao.freelaw.ai** para gestão de RH e produtividade (nota: gestao.freelaw.ai **É** o Backstage — mesmo deploy Vercel, não é app separado)

Isso gera: dados duplicados, decisões baseadas em informação incompleta, dependência de ferramentas caras, e incapacidade de automatizar fluxos end-to-end.

### 1.4 Princípio operacional

> "Isso ajuda a fechar o gate da Fase 0?" — Se não, vai para o backlog.

A Fase 0 tem 7 gates. Cada gate representa uma capacidade operacional que o Backstage precisa entregar. Só depois que os 7 gates estiverem fechados é que avançamos para a visão expandida (cadências IA, omni-channel, flywheel).

### 1.5 Princípio do ciclo de vida completo

> **Todo cliente deve ser rastreável do ABSOLUTO início ao fim.**

O Backstage deve registrar cada touchpoint desde o primeiro contato (clique num ad, preenchimento de formulário, ligação fria) até o offboarding (churn, exclusão de dados). O ciclo completo:

**Aquisição → Qualificação → Deal → Onboarding → Ativo → Risco → Renovação/Churn → Offboarding**

Cada transição gera dados. Cada dado alimenta decisões. Nenhum touchpoint pode ser perdido. Este princípio atravessa todas as macro-áreas e é pré-requisito para cohortização, unit economics confiáveis e flywheel operacional.

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
- Contabilidade nativa (substituindo Conta Azul)

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
| Apps no monorepo | 7 (backstage, studio-offices, studio-providers, mobile x2, website, cron-service) |
| Packages compartilhados | 27 |
| Pages (Backstage) | 299 |
| Pages (total, todos os apps) | 448 |
| API Routes (total) | 629 |
| Database Schema Files | 100+ |
| Integrações externas | 14 (com plano de internalização — ver 3.7) |
| AI Agents (Mastra) | 3 backstage + monorepo AI separado |
| Cron Jobs (Vercel) | 24 |
| Test Files (Backstage) | 184 |
| UI Components (Design System) | 80+ |
| Design System Doc Pages | 70+ |

**Conclusão**: O Backstage já tem MUITA coisa construída. O problema não é "construir do zero" — é **terminar, conectar e estabilizar** o que existe.

### 3.2 Mapa de módulos

| Módulo | Rota | Páginas | Status | Gate |
|--------|------|---------|--------|------|
| **Financeiro** | /financeiro/* | 28 | Funcional | G1, G5 |
| **Vendas** | /vendas/* | 31 | Funcional | G3 |
| **CRM** | /crm/* | 12 | Funcional | G3 |
| **Aquisição** | /aquisicao/* | 6 | Parcial (stubs) | G3 |
| **CS** | /cs/* | 9 | Parcial | G2 |
| **Suporte** | /suporte/* | 13 | Parcial | G2, G7 |
| **WhatsApp** | /interno/whatsapp | 7 | Parcial | G7 |
| **Prestadores** | /prestadores/* | 14 | Funcional | — |
| **Produto** | /produto/* | 112 | Funcional | — |
| **RH** | /rh/* | 18 | Parcial | — |
| **Chat** | /chat | 4 | Funcional | — |
| **Wiki** | /wiki | 5 | Funcional | — |
| **Oitiva** | /oitiva/* | 8 | Em dev | — |
| **Marketing** | /marketing/* | 4 | Parcial | — |
| **Planejamento** | /planejamento | — | Parcial | — |
| **Dashboard** | /dashboard | — | Funcional | G5 |

### 3.3 Integrações externas (13)

| Integração | O que faz | Direção | Status | Gate | Plano de Internalização |
|------------|-----------|---------|--------|------|------------------------|
| **HubSpot** | CRM legado (16 entidades: contacts, companies, deals, calls, meetings, etc.) | Bidirecional → migrando para read-only | Ativo — em deprecação | G3 | **Substituir por CRM package (MA-CRM)** |
| **Stripe** | Billing para clientes novos. Webhook de eventos. | Inbound (webhooks) | Ativo — primário | G1 | Manter (gateway de pagamento) |
| **Iugu** | Billing legado. Sync via ETL (customers, subscriptions, invoices, financials). | Sync periódico | Ativo — sync estável | G1, G6 | Manter (gateway de pagamento) |
| **Meta Ads** | Marketing analytics. Dados de campanhas e CAPI. | Inbound | Ativo | — | Manter (fonte de dados externa) |
| **Google Ads** | Marketing analytics. Dados de campanhas. | Inbound | Ativo | — | Manter (fonte de dados externa) |
| **Conta Azul** | Contabilidade. Sync de despesas e categorias. | Sync periódico | Ativo | G1, G6 | **Substituir por Financeiro module (MA1)** |
| **Evolution** | WhatsApp Business API. Envio/recebimento de mensagens. | Bidirecional | Ativo | G7 | Manter (canal de comunicação) |
| **Blip** | Chatbot para atendimento automatizado. | Bidirecional | Ativo | G7 | Manter (canal de comunicação) |
| **Banco Inter** | Banking (mTLS + OAuth). Transações e reconciliação. | Inbound | Ativo (auto-submit 5min, sync 15min) | G6 | Manter (fonte de dados bancários) |
| **Notion** | Documentação. Sync de conteúdo. | Inbound | Ativo | — | **Substituir por Wiki/Knowledge Base (MA12)** |
| **Slack** | Comunicação interna. Notificações e alertas. | Outbound | Ativo | G2 | **Substituir por Chat interno (MA5)** |
| **Golden Record** | Entity resolution. Unificação de registros duplicados. | Bidirecional | Ativo | G3 | Manter (infra de dados) |
| **Clicksign** | Assinatura digital de contratos. | Bidirecional | Ativo | G3 | Manter (serviço especializado) |
| **Resend** | Email transacional. Webhooks de tracking (opens/clicks/replies). | Outbound + webhooks | Ativo | G7 | Manter (canal de comunicação) |
| **PostHog** | Product analytics. Person lookup, métricas de uso. | Inbound | Ativo | G5 | Manter (analytics externo) |
| **Vapi** | Voice/calls. Webhook handler para ligações. | Inbound | Ativo | — | Manter (canal de comunicação) |

### 3.4 AI Agents & Infraestrutura IA

**Backstage Agents (Mastra)**:

| Agent | Função | Status |
|-------|--------|--------|
| **Backstage Assistant** | Assistente geral do Backstage (API fetcher autenticado). | Ativo |
| **CEO IA** | Insights executivos sobre métricas da empresa. | Ativo |
| **Maria Insights** | Insights sobre prestadores (usado no domínio prestadores). | Ativo |
| **Oitiva Evaluation** | Avalia qualidade de ligações de vendas via transcrições. Scorecards com critérios ponderados. | Ativo (crons: sync-calls 10min, sync-evaluations 15min, process-transcriptions 5min) |

**Tools disponíveis**: `fetch-financial-data`, `fetch-sales-data`, `natural-query`

**Monorepo AI separado** (`ai/`):
- `@freelaw/ai` — Core AI infrastructure
- `ai/apps/ai-service` — Mastra-based AI service (legal prompts, petitions, CNJ validation, tribunal taxonomy)
- `ai/apps/studio-offices` — Mastra generate/iterate/chat para clientes
- `ai/apps/backstage`, `ai/apps/document-converter`, `ai/apps/marketing`
- SDKs: `@ai-sdk/anthropic`, `@ai-sdk/google`, `@ai-sdk/openai`

**API Routes IA**: `/api/ai/churn`, `/api/ai/edit`, `/api/ai/predictions`, `/api/ai/recommendations`, `/api/assistente/chat`
**Crons IA**: `openclaw-runtime` (10min), `churn-prediction` (weekly)

### 3.5 Domínios no backstage-core

| Domínio | Entidades principais |
|---------|---------------------|
| **Finance** | Subscriptions (Stripe+Iugu), MRR calculation, DRE, Budget, Cashflow, Unit Economics (CAC, LTV, payback) |
| **Leads** | Lead Hunter (prospecção B2B com IA), filtros por localidade, Google Places enrichment, Exa data enrichment |
| **Comunidade** | Provider funnel, tiers (S+ a D), journey phases, saturação, ranking |
| **Retention** | Churn risk signals, health score (5 componentes), risk levels, trends, retention offers |
| **Marketing** | Ad campaigns (Meta/Google), content analytics, channel performance, CAPI integration |

### 3.6 Vantagem do Monorepo: Packages Transversais

O monorepo da Freelaw permite que **packages compartilhados** sirvam múltiplas aplicações (backstage, studio, etc.). Esta é uma **vantagem estratégica** que o PRD deve explorar ao máximo.

**O que são packages transversais**:
- Código compartilhado entre apps: data models, types, validação, UI components
- Cada package encapsula um domínio e expõe APIs tipadas
- Qualquer app do monorepo pode importar e consumir

**27 packages existentes no monorepo** (auditoria 2026-03-09):

| Categoria | Packages | Reuso |
|-----------|----------|-------|
| **Core/Infra** | `@freelaw/core`, `@freelaw/config`, `@freelaw/auth`, `@freelaw/server`, `@freelaw/shared`, `@freelaw/lib`, `@freelaw/types`, `@freelaw/tsconfig` | Alto — todos os apps |
| **UI** | `@freelaw/ui` (80+ componentes), `@freelaw/ui-native` | Alto — todos os apps |
| **Domínio** | `@freelaw/backstage-core`, `@freelaw/hr-core`, `@freelaw/hr-sdk`, `@freelaw/payments-core`, `@freelaw/matching`, `@freelaw/workspace-core`, `@freelaw/workspace-ui` | Médio — apps específicos |
| **Infra pesada** | `@freelaw/infra` (100+ schemas, banking, analytics, observability, Slack, WhatsApp, Notion, Meta Ads, RAG) | Alto — backend |
| **Produto** | `@freelaw/api-contracts`, `@freelaw/feature-gates`, `@freelaw/workflows`, `@freelaw/editor`, `@freelaw/content-pipeline`, `@freelaw/ai` | Médio-Alto |
| **Provider** | `@freelaw/studio-providers-core`, `@freelaw/studio-providers-sdk` | Específico |

**Por que isso importa**:
- Evita duplicação de lógica entre 7 apps
- Garante consistência de tipos e validações
- Permite que mudanças num domínio reflitam em todas as apps que o consomem
- Habilita que novas apps (mobile, portal do cliente) reusem a mesma infraestrutura
- O `@freelaw/infra` é o package mais estratégico: contém 100+ schemas, todas as integrações, e a camada de dados inteira

> **Regra**: Sempre que uma funcionalidade serve mais de uma macro-área, ela deve ser um **package**, não código duplicado em cada área.

### 3.7 Estratégia de Internalização

A Freelaw depende de ferramentas externas que, ao longo do tempo, serão substituídas por módulos internos construídos no monorepo. Esta estratégia reduz custos, aumenta controle sobre dados e elimina fragmentação.

| Ferramenta externa | Substituto interno | Macro-Área | Timeline | Impacto |
|--------------------|--------------------|------------|----------|---------|
| **HubSpot** | CRM package nativo | MA-CRM | Fase 0 (G3) | Elimina ~R$X/mês + unifica dados de vendas |
| **Conta Azul** | Módulo Financeiro com contabilidade nativa | MA1 | Fase 0-1 | Elimina dependência + habilita General Ledger próprio |
| **Notion** | Wiki / Knowledge Base interno | MA12 | Fase 1 | Parcialmente existe; completar e migrar conteúdo |
| **Slack** | Chat interno do Backstage | MA5 | Fase 1+ | Parcialmente existe; falta feature parity para alertas críticos |
| **gestao.freelaw.ai** | RH & Produtividade | MA9 | Fase 0-1 | Migração completa de funcionalidades existentes |

**Princípios da internalização**:
1. **Só internalizar quando o módulo interno for MELHOR** que a ferramenta externa (ou equivalente + integrado)
2. **Migração gradual**: dual-write → leitura interna → desligar externo
3. **Dados históricos**: sempre migrar ou manter acessíveis (nunca perder)
4. **Contagem de integrações**: das 17 integrações atuais, 4 (HubSpot, Conta Azul, Notion, Slack) serão internalizadas. As 13 restantes continuam como integrações externas (gateways de pagamento, canais de comunicação, fontes de dados, serviços especializados).

---

## 4. ARQUITETURA DE DADOS — FONTE DA VERDADE

> **Esta seção é CENTRAL ao objetivo do Backstage.** O Backstage existe para ser a fonte da verdade
> operacional da Freelaw. Sem uma arquitetura de dados rigorosa, todo o resto desmorona.

### 4.1 Princípios fundamentais

1. **Toda transação financeira → lançamento contábil → razão geral (General Ledger)**
   - Integrações bancárias (Banco Inter, Iugu, Stripe) DEVEM fluir pelo plano de contas
   - Nenhum dado financeiro pode existir fora do GL
   - Reconciliação automática: gateway ↔ GL ↔ DRE

2. **Ciclo de vida completo do cliente**
   - Do primeiro touchpoint (ad click, form fill, ligação fria) até offboarding (churn + exclusão de dados)
   - Cada transição gera evento registrado e auditável
   - Fluxo: **Ad/Conteúdo → Lead → Qualificação → Deal → Contrato → Onboarding → Ativo → Health Monitoring → Renovação/Churn → Offboarding**

3. **Single source of truth por domínio**
   - Cada dado tem UM owner (uma macro-área responsável)
   - Outras áreas consomem via API ou views, nunca duplicam
   - Conflitos de ownership devem ser resolvidos explicitamente

4. **Auditabilidade**
   - Toda mudança de estado deve ter timestamp, autor e motivo
   - Dados financeiros devem ter trilha de auditoria completa
   - Exclusões lógicas (soft delete) para dados regulados

### 4.2 General Ledger (Plano de Contas)

O General Ledger é a espinha dorsal financeira do Backstage. Todo fluxo financeiro converge aqui.

```
ENTRADAS DE DADOS                    GENERAL LEDGER                    SAÍDAS
                                     (Plano de Contas)
Stripe webhooks ────────┐                                    ┌──→ DRE
Iugu sync ──────────────┤                                    ├──→ Balanço
Conta Azul sync ────────┤      ┌───────────────────┐         ├──→ DFC
Banco Inter sync ───────┼────→ │  Journal Entries   │────────→├──→ Unit Economics
Pagto prestadores ──────┤      │  (Lançamentos)     │         ├──→ Business Plan
Despesas manuais ───────┤      │       ↓            │         ├──→ Budget vs Real
Receita manual ─────────┘      │  General Ledger    │         └──→ Conciliação
                               │  (Razão)           │
                               │       ↓            │
                               │  Chart of Accounts │
                               │  (Plano de Contas) │
                               └───────────────────┘
```

**Estado atual (verificado no código 2026-03-09)**:

O General Ledger **já existe e é maduro**. Schema `v2_accounting` com:
- `chart_of_accounts`: **85 contas** hierárquicas, alinhadas com BP 2026
- `transactions`: Double-entry journal entries com workflow (pending → confirmed → reconciled)
- `transaction_allocations`: Débitos/créditos vinculados a contas e centros de custo
- `cost_centers`: Centros de custo alinhados à estrutura organizacional
- `vendors`, `bank_accounts`, `budgets`, `budget_lines`, `bank_reconciliations`
- `approval_requests`: Workflow de aprovação multi-nível
- `alerts`: Alertas financeiros (info/warning/critical)
- `audit_log`: Trilha de auditoria completa

**5 Posting Engines** (5.689 linhas, 10 arquivos):
1. `finance-posting.ts` (1.253 linhas) — v2_billing → GL (invoices, subscriptions)
2. `contaazul-posting.ts` (405 linhas) — Conta Azul ERP → GL
3. `inter-posting.ts` (316 linhas) — Banco Inter banking → GL
4. `payroll-posting.ts` (283 linhas) — Folha de pagamento → GL
5. `tax-posting-etl.ts` (455 linhas) — Cálculos tributários → GL

**Validação**: Toda posting engine valida balanço double-entry (SUM débitos = SUM créditos) atomicamente. Desbalanceamento causa rollback.

**Hierarquia do Plano de Contas** (85 contas):
- 1.x ATIVO: Caixa, Contas a Receber, PDD
- 2.x PASSIVO: Receita Diferida, A Pagar Prestadores, Impostos
- 3.x PATRIMÔNIO LÍQUIDO: Capital Social, Lucros Acumulados
- 4.x RECEITA: Faturamento (Contratantes + Prestadores), Não Recorrente, Serviços Avulsos, Deduções (PIS/COFINS, ISS, CBS, PCLD)
- 5.x DESPESA: Custos Receita, Vendas, Marketing, G&O, Administrativo, PDD, AI Inference, Resultado Financeiro
- 6.x Depreciação/Amortização
- 7.x Impostos sobre Lucro (IRPJ, CSLL)

### 4.3 Ownership de dados por macro-área

| Domínio de Dados | Owner (MA) | Tabelas principais | Consumidores |
|-------------------|-----------|---------------------|-------------|
| Leads, Contacts, Companies | MA-CRM | v2_sales.leads, .contacts, .companies | MA2, MA3, MA4, MA10, MA11 |
| Deals, Pipelines, Stages | MA-CRM | v2_sales.deals, .pipelines, .stages | MA2, MA1, MA8 |
| Activities, Sequences | MA-CRM | v2_sales.activities, .sequences | MA2, MA3 |
| Subscriptions, Invoices, MRR | MA1 | v2_billing.* | MA4, MA8 |
| Despesas, DRE, GL | MA1 | fin_*, budget*, vendor_category_mapping | MA8, MA11 |
| Health Score, NPS, Journey | MA4 | crm.customer_health_scores, .nps_responses, .customer_journey | MA1, MA2, MA8 |
| Renovações, Churn | MA4 | crm.renewal_tracking, churn_predictions | MA1, MA2, MA8 |
| Conversas, Mensagens | MA5 | v2_comms.conversations, .messages | MA2, MA4 |
| Templates de mensagem | MA5 | v2_comms.notification_templates | MA2, MA3, MA4, MA6 |
| Campanhas, Ads | MA10 | v2_marketing.campaigns, .ad_campaigns_v2 | MA3, MA8 |
| Prestadores, Tiers | MA11 | providers.* | MA1, MA-CRM |
| Workflows, Automações | MA6 | marketing_automations, lead_scoring_rules | MA2, MA3, MA4 |
| Sync externo | MA7 | sync.* | MA1, MA-CRM |

### 4.4 Convenções de schema

- **Naming**: snake_case para tabelas e colunas
- **IDs**: UUID v4 (gerado no banco)
- **Timestamps**: `created_at`, `updated_at` em todas as tabelas (timestamptz)
- **Soft delete**: `deleted_at` (nullable timestamptz) para dados regulados
- **Organization**: `organization_id` para multi-tenancy
- **Schemas nomeados**: `v2_billing`, `v2_sales`, `v2_marketing`, `v2_comms`, `crm`, `sync` — nunca `public`
- **RLS obrigatório**: todas as tabelas novas devem ter policies

### 4.5 Fluxos de dados críticos

#### Fluxo de Aquisição → Venda → Cliente → Lifecycle completo

```
PRIMEIRO TOUCHPOINT              QUALIFICAÇÃO                FECHAMENTO
(ad click, form fill,     →     BDR/SDR prospecta     →     Closer negocia
 ligação, evento, referral)          ↓                            ↓
    ↓                          v2_sales.leads              v2_sales.deals
v2_marketing.conversion_events (status: new→contacted      (status: open→won)
v2_sales.activities             →qualified→converted)           ↓
(first_touch registrado)             ↓                     NOVO CLIENTE
                               v2_sales.contacts           v2_billing.customers
                               (lifecycle: lead→mql        v2_billing.subscriptions
                                →sql→opportunity                ↓
                                →customer)              ONBOARDING (MA4)
                                                       crm.customer_journey
                                                       (stage: onboarding)
                                                              ↓
                                                        ATIVO + MONITORAMENTO
                                                        crm.customer_health_scores
                                                        (cron calcula scores)
                                                              ↓
                                                     ┌────────┴────────┐
                                                     ↓                 ↓
                                                  SAUDÁVEL         EM RISCO
                                                  Renovação        Playbook CS
                                                  Expansion        Retention offer
                                                     ↓                 ↓
                                                  RENEWED          CHURNED
                                                     ↓                 ↓
                                                  Novo ciclo      Offboarding
                                                                  (dados retidos
                                                                   para analytics)
```

#### Fluxo Financeiro → General Ledger

```
EVENTOS FINANCEIROS            JOURNAL ENTRIES              RELATÓRIOS
Stripe webhook      ───┐      ┌──────────────┐
(subscription.paid)     │      │              │         ┌─→ DRE mensal
                        ├────→ │  Lançamento  │─────────├─→ Balanço patrimonial
Iugu sync           ───┤      │  Contábil     │         ├─→ DFC
(invoices, receivables) │      │              │         ├─→ KPIs (MRR, churn)
                        ├────→ │      ↓       │─────────├─→ Unit Economics
Conta Azul sync     ───┤      │  Razão Geral │         ├─→ Conciliação
(despesas, categorias)  │      │ (General     │         └─→ Business Plan
                        ├────→ │  Ledger)     │
Banco Inter sync    ───┤      │              │
(transações)            │      │      ↓       │
                        ├────→ │  Plano de    │
Pagto prestadores   ───┘      │  Contas      │
                              └──────────────┘
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

### 4.6 RLS e Segurança

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

## 5. MACRO-ÁREAS DO BACKSTAGE

> Cada macro-área é um domínio funcional do Backstage com escopo, responsável e PRD próprio.
> O responsável pelo PRD de cada área detalha user stories, critérios de aceite e dependências.
> Esta seção é o **mapa de ownership** — quem é dono de quê.

### Arquitetura: CRM como Operating System

> **Decisão arquitetural**: O CRM **não é um módulo isolado**. Ele é uma camada de infraestrutura
> (package) que roda **por dentro** de múltiplas áreas. Marketing + Vendas = CRM, mas a mesma
> estrutura (contacts, companies, deals, activities, pipelines) é reutilizada para outros domínios:
> pipeline de gestão de prestadores, pipeline de CS, pipeline de investidores, etc.
>
> O **Management OS** segue a mesma lógica transversal: AI Cockpit + CEO AI + Pulses + Weekly Reports
> + Combinados rodam **por dentro** de todas as áreas. Cada pessoa abre o Backstage e encontra uma
> home personalizada pelo seu papel. Como o CRM, o Management OS é infraestrutura consumida por
> todas as macro-áreas — não é um módulo isolado.
>
> Essa lógica de **packages transversais** se aplica a outras conexões entre áreas:
> - **Financeiro ↔ Prestadores**: pagamento a prestadores alimenta custos e DRE
> - **Financeiro ↔ CS**: recebimentos ligados a health score e inadimplência
> - **Financeiro ↔ Billing**: faturamento → receita → MRR → business plan
> - **Vendas ↔ CS**: deal fechado → onboarding → health score → renovação
> - **Aquisição ↔ Vendas**: lead qualificado → deal no pipeline
> - **Mensageria ↔ Todos**: WhatsApp/email como canal transversal
> - **Management OS ↔ Todos**: pulses, reports e combinados alimentam e são alimentados por todas as MAs
>
> **Status**: Mapeamento completo de conexões cross-área **PENDENTE** (ver D14).

### Alerta: Desalinhamento Domínios vs Macro-Áreas

> **Auditoria do código (2026-03-09)** revelou que existem **3 taxonomias independentes** que evoluíram separadamente:
>
> 1. **29 route domains** no filesystem (cresceram organicamente)
> 2. **11 sidebar sections** na UI (curadoria manual)
> 3. **10 ColaboradorAreas** no DB de RH (org chart)
>
> **Problemas identificados:**
> - **10 rotas órfãs** sem entrada no sidebar: `chat`, `wiki`, `crm`, `assistente`, `workflows`, `design-system`, `features`, `projetos`, `meu-espaco`, e todo o grupo `(aquisicao)`
> - 3 sidebar sections (`oitiva`, `whatsapp`, `suporte`) sem ColaboradorArea correspondente no area-mapping
> - Relações many-to-one: "Pessoas" agrega 8 route domains (`rh`, `colaboradores`, `payroll`, `performance`, `ausencias`, `desenvolvimento`, `vagas`, `people-analytics`)
> - "Operacoes" no sidebar mapeia para `/prestadores` nas rotas
>
> **Decisão necessária (D21)**: Unificar as 3 taxonomias sob as macro-áreas do PRD.
> Arquivos-chave: `app-sidebar.tsx`, `area-mapping.ts`, `permissions.ts`

### Mapa de Atribuição

| # | Macro-Área | Tipo | Módulos | Páginas | Gates | Responsável PRD | Status PRD |
|---|------------|------|---------|---------|-------|-----------------|------------|
| **PACKAGES (infra transversal)** ||||||
| MA-CRM | CRM (Operating System) | Package | Transversal | — | G3 | _a definir_ | Não iniciado |
| MA-MGMT | Management OS (Gestão & Reports) | Package | Transversal | — | — | _a definir_ | Não iniciado |
| MA7 | Integrações | Package | Transversal (13→9 sistemas, com internalização) | — | G6 | _a definir_ | Não iniciado |
| MA6 | Automações & Workflows | Package | /produto/workflows, crons, edge functions | — | G4 | _a definir_ | Não iniciado |
| MA5 | Mensageria & Comunicação | Package | /interno/whatsapp, /suporte/inbox, /chat | 31 | G7 | _a definir_ | Não iniciado |
| **ÁREAS DE NEGÓCIO (consomem os packages)** ||||||
| MA1 | Financeiro & Billing | Área | /financeiro/* | 28 | G1, G5 | _a definir_ | Não iniciado |
| MA2 | Vendas & Pipeline | Área | /vendas/*, /oitiva/* | 39 | G3 | _a definir_ | Não iniciado |
| MA3 | Aquisição (Multicanal) | Área | /aquisicao/*, /vendas/outbound | 6 | G3 | _a definir_ | Não iniciado |
| MA4 | Customer Success | Área | /cs/*, /suporte/cliente/* | 24 | G2 | _a definir_ | Não iniciado |
| MA11 | Prestadores & Comunidade | Área | /prestadores/* | 12 | — | _a definir_ | Não iniciado |
| MA10 | Marketing | Área | /marketing/* | 4 | — | _a definir_ | Não iniciado |
| MA9 | RH & Produtividade | Área | /rh/* | 18 | — | _a definir_ | Não iniciado |
| MA13 | IA & Produção (LIA) | Área | /ia/*, /oitiva/* | — | — | _a definir_ | Não iniciado |
| **SUPORTE** ||||||
| MA8 | Dashboard & Analytics Executivo | Suporte | /dashboard, /executive | — | G5 | _a definir_ | Não iniciado |
| MA12 | Produto & Plataforma | Suporte | /produto/*, /wiki, /chat | 121 | — | _a definir_ | Não iniciado |

### Conexões Cross-Área (a mapear)

> **ATENÇÃO**: As conexões abaixo são o mapeamento inicial. Cada conexão precisa ser
> detalhada com: dados que fluem, direção, triggers, e quem é owner do fluxo.
> Este mapeamento é pré-requisito para construir os PRDs das macro-áreas.

```
    ┌───────────────────────────────────────────────────────────────────┐
    │                  Management OS (MA-MGMT)                         │
    │  AI Cockpit · CEO AI · Pulses · Weekly Reports · Combinados      │
    │  (transversal — personalizado por role, permeia todas as áreas)  │
    └──────────┬──────────┬──────────┬──────────┬──────────┬───────────┘
               │          │          │          │          │
    ┌──────────▼──────────▼──────────▼──────────▼──────────▼───────────┐
    │                CRM (Operating System)                            │
    │  contacts · companies · deals · pipelines                        │
    │  activities · sequences · scoring                                │
    └──────┬──────────┬──────────┬──────────┬──────────┬───────────────┘
           │          │          │          │          │
           ▼          ▼          ▼          ▼          ▼
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
      │  General Ledger · plano de contas             │
      └────────────────────┬──────────────────────────┘
                           │
                           ▼
      ┌───────────────────────────────────────────────┐
      │            IA & Produção (MA13)               │
      │  LIA · AI agents · Oitiva · insights          │
      └───────────────────────────────────────────────┘
```

| Conexão | De → Para | Dados que fluem | Status Mapeamento |
|---------|-----------|-----------------|-------------------|
| Venda → Onboarding | MA2 → MA4 | Deal fechado dispara onboarding CS | A mapear |
| CS → Renovação | MA4 → MA2 | Health score alimenta pipeline renovação | A mapear |
| CS → Financeiro | MA4 → MA1 | Inadimplência, churn → impacto MRR | A mapear |
| Vendas → Financeiro | MA2 → MA1 | Deal fechado → faturamento → receita → GL | A mapear |
| Prestadores → Financeiro | MA11 → MA1 | Pagamento a prestadores → custos/DRE/GL | A mapear |
| Aquisição → Vendas | MA3 → MA2 | Lead qualificado → deal no pipeline CRM | A mapear |
| Marketing → Aquisição | MA10 → MA3 | Campanhas (qualquer canal) → leads | A mapear |
| Financeiro → Dashboard | MA1 → MA8 | MRR, receita, despesas, unit economics | A mapear |
| CS → Dashboard | MA4 → MA8 | Health score médio, NPS, clientes em risco | A mapear |
| Mensageria → Todos | MA5 → * | WhatsApp/email como canal transversal | A mapear |
| CS → Suporte | MA4 ↔ MA5 | Tickets de suporte alimentam health score | A mapear |
| CS → Produto | MA4 ← Studio | Dados de uso alimentam health score | A mapear |
| Aquisição → CS | MA3 → MA4 | Onboarding inicia imediatamente após deal close | A mapear |
| Management OS → Todas | MA-MGMT ↔ * | Pulses, reports e combinados alimentam e são alimentados por todas as MAs. Cockpit consome dados de todas as áreas. | A mapear |

---

### MA1: Financeiro & Billing

**Escopo**: Toda gestão financeira — receita, custos, MRR, DRE, billing, conciliação, unit economics, business plan. Inclui contabilidade nativa (substituindo Conta Azul) e General Ledger.

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
- General Ledger / Plano de Contas: estruturar para receber todos os fluxos financeiros
- Contabilidade nativa: definir escopo mínimo para começar a substituir Conta Azul

**Gates**: G1 (95%), G5 (parcial — dados financeiros alimentam dashboard)

**Escopo Fase 0 (2 meses)**:
- [ ] Business Plan 100% editável na UI (eliminar dependência Google Sheets)
- [ ] General Ledger: tabela de journal_entries com plano de contas configurável
- [ ] Todas as transações Iugu e Stripe → journal entries automáticos
- [ ] Despesas Conta Azul → journal entries (sync mantido, mas GL como fonte de verdade)
- [ ] DRE gerada a partir do GL (não de queries avulsas)
- [ ] Recebimentos: tela funcional com dados reais do Iugu/Stripe
- [ ] Estruturas organizacionais: CRUD mínimo funcional
- [ ] Conciliação automática: GL ↔ gateway (Iugu + Stripe) com alerta de divergências
- [ ] ERP (Freelaw One): decisão tomada e documentada (D9)
- [ ] Unit Economics com dados atualizados (< 24h defasagem)
- [ ] Relatório diário automático funcionando
- [ ] Banco Inter: decisão sobre ativação (D12)

---

### MA-CRM: CRM (Operating System)

**Escopo**: Camada de infraestrutura que fornece contacts, companies, deals, pipelines, activities, sequences e scoring para todas as áreas que precisam gerenciar relacionamentos. Inclui pipeline de investidores para relações com investidores.

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
- Configuração de pipelines por domínio (vendas, prestadores, CS, investidores)
- Documentar API de package para áreas consumidoras

**Pipeline de Investidores (Fase 1+)**:
- A arquitetura de pipelines configuráveis DEVE suportar extensão para relações com investidores
- Funcionalidades futuras: investor pipeline, fundraising tracking, data room management
- A decisão de implementar é Fase 1+, mas o design dos pipelines na Fase 0 não pode bloquear isso
- Mesma infraestrutura: contacts (tipo: investor), companies (tipo: fund), deals (pipeline: fundraising)

**Gates**: G3 (70%) — **GATE MAIS CRÍTICO** (mudança de processo + código)

**Escopo Fase 0 (2 meses)**:
- [ ] HubSpot sync desligado (cron desabilitado, fonte de verdade = v2_sales)
- [ ] Migração completa de dados históricos HubSpot → v2_sales (contacts, companies, deals)
- [ ] Dedup nativo funcionando (consulta v2_sales.companies.domain, sem HubSpot)
- [ ] Motor de sequences: cron que processa enrollments e envia emails via Resend
- [ ] 3 pipelines configurados: vendas (MA2), prestadores (MA11), CS (MA4)
- [ ] API de package documentada (como outras áreas consomem o CRM)
- [ ] Golden Record integrado ao fluxo de criação (dedup automática)
- [ ] Tracking de opens/clicks/replies via webhooks Resend
- [ ] Contacts com lifecycle tracking completo (lead → customer → churned)
- [ ] Activities registrando TODOS os touchpoints (calls, emails, meetings, demos, tasks)

---

### MA-MGMT: Management OS (Gestão, Reports, AI Cockpit)

**Escopo**: Sistema de gestão transversal — AI Cockpit personalizado por role, CEO AI que orquestra prioridades/cobranças/aprendizado, Pulses (captura qualitativa diária/semanal), Weekly Reports automáticos, Combinados (commitments rastreáveis com evidência), Knowledge Schema para contexto da IA, Ritmos de gestão (diário/semanal/mensal/trimestral).

**Visão**: "A IA não é um relatório: ela é o sistema operacional da gestão." Cada pessoa abre o Backstage e encontra o que é mais importante para ela naquele dia. A IA observa dados, pergunta o que falta, documenta decisões, cobra combinados, propõe plano, e mede resultado.

**Módulos**: Transversal — roda por dentro de todas as áreas (como o CRM package)

**Componentes**:

1. **AI Cockpit** (home personalizada por role): Top priorities, metas, alertas, chat com IA, combinados pendentes
2. **Pulses** (captura qualitativa): Daily pulse (30-60s), Weekly pulse (2-4min), Incident pulse (ad hoc), suporte a áudio com transcrição automática
3. **Weekly Report** (CEO AI gera automaticamente): Resumo executivo, riscos/oportunidades, decisões sugeridas, plano de ação, perguntas pendentes
4. **Combinados** (commitments rastreáveis): título, owner, data, KPI relacionado, evidência esperada, status, logs automáticos
5. **CEO AI behaviors**: Sensemaking (conecta dados e narrativa), Prioritização (rankeia por impacto), Orquestração (distribui ações), Cobrança (follow-ups automáticos), Documentação (registra decisões e outcomes)
6. **CEO AI modes**: Autopilot (sugere, humano aprova), Copilot (humano pede, IA executa), Ops Mode (anomalia detectada → playbook automático)

**Knowledge Schema**:
- `knowledge_events` — eventos relevantes (marco, incidente, mudança)
- `knowledge_pulses` — respostas de daily/weekly pulses
- `knowledge_decisions` — decisões documentadas com contexto e rationale
- `knowledge_outcomes` — resultados de ações e experimentos
- `kpi_definitions` — definição única de cada KPI (fórmula, fonte, owner, frequência)
- `kpi_values` — valores históricos de KPIs (snapshots)
- `management_actions` — ações de gestão (cobranças, follow-ups, escalações)

**Cockpits por role**:
- **Vendas**: pipeline ativo + forecast de receita + top 5 accounts + combinados de vendas
- **Marketing**: budget utilizado + CAC por canal + experimentos ativos + leads da semana
- **CS**: health score médio + clientes em risco + playbooks ativos + renovações próximas
- **Financeiro**: caixa projetado + MRR trend + desvios do BP + alertas financeiros
- **Executivo**: north star metric + top 3 riscos + decisões pendentes + combinados da liderança

**O que existe**:
- Mastra AI agents (backstage-assistant, CEO IA, Maria Insights, Oitiva Evaluation)
- Dashboard hub (/dashboard)
- Alguns KPIs espalhados por módulos
- Nenhum sistema de pulses
- Nenhum sistema de combinados

**O que falta para Fase 0** (MVP):
- Home personalizada por role (4 roles mínimos)
- Daily pulse (texto) + Weekly pulse (texto + áudio)
- Weekly report automático (1 executivo + por área)
- Combinados com owner/prazo
- Alertas simples (thresholds) em 5 KPIs vitais

**Fases**:
- **Fase 1**: "Gestão que se escreve sozinha" — Pulses capturam contexto → IA sintetiza → weekly report → ações sugeridas
- **Fase 2**: "Gestão que executa" — IA cria drafts de emails, tasks, tickets, updates automaticamente
- **Fase 3**: "Gestão que aprende" — Loop completo: ações → evidências → outcomes → playbooks. A IA aprende o que funciona e refina continuamente

**Gates**: Transversal — conecta com G2 (CS cockpit), G3 (Vendas cockpit), G5 (Dashboard executivo)

**Escopo Fase 0 (2 meses)**:
- [ ] Home/Cockpit personalizada para 4 roles (Vendas, CS, Marketing, Financeiro)
- [ ] Daily pulse com template por área (texto, 30-60s para preencher)
- [ ] Weekly pulse com suporte a áudio + transcrição automática
- [ ] CEO AI gera weekly report automático (1 executivo + 4 por área)
- [ ] Weekly report inclui: resumo narrativo, 3-7 decisões sugeridas, plano de ação com owners
- [ ] Combinados CRUD: criar, atribuir, acompanhar, cobrar, fechar com evidência
- [ ] 5 KPIs vitais com alertas de threshold (MRR, churn, pipeline, caixa, NPS)
- [ ] Chat com IA na home (gerar email, resumo, plano, checklist)
- [ ] Knowledge schema básico implementado (events, pulses, decisions, kpi_definitions)
- [ ] Cockpit testável em produção pelo PO para cada role

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
- **Conexão com CS**: deal fechado → trigger automático de onboarding

**Gates**: G3 (70%) — **GATE MAIS CRÍTICO** (mudança de processo + código)

**Escopo Fase 0 (2 meses)**:
- [ ] Pipeline de deals 100% no Backstage (zero operação no HubSpot)
- [ ] Propostas: fluxo completo draft → sent → viewed → accepted com notificação
- [ ] Comissão: forecast validado com time de vendas (BDR 1%, Closer 5%)
- [ ] Oitiva: avaliação de ligações integrada ao scorecard do vendedor
- [ ] Analytics de vendas: ciclo de vendas, win rate, ticket médio, conversão por estágio
- [ ] Metas vs realizado: dashboard com progresso em tempo real
- [ ] Deal fechado → trigger automático: criar subscription + iniciar onboarding CS
- [ ] Timeline de interações por deal (todas as activities linkadas)

---

### MA3: Aquisição (Multicanal)

**Escopo**: Toda a estratégia de aquisição de clientes, independente do canal. A macro-área responde à pergunta "como trazemos clientes?" e deve suportar qualquer canal de aquisição presente ou futuro.

**Módulos**: `/aquisicao/*` (6 páginas), `/vendas/outbound`, `/vendas/leads`

**Canais suportados** (a estrutura deve ser agnóstica ao canal):
- **Outbound**: prospecção BDR (Lead Hunter, telefone, email, LinkedIn)
- **Inbound**: site, blog, landing pages, formulários
- **Paid**: Meta Ads, Google Ads, outras plataformas
- **Organic**: SEO, conteúdo, redes sociais
- **Referral**: indicação de clientes existentes, parceiros
- **Events**: eventos, webinars, conferências
- **Partnerships**: parceiros estratégicos, revendedores
- **Community**: comunidade jurídica, fóruns

**O que existe**:
- Lead Hunter: prospecção B2B com Google Places + Exa enrichment → FUNCIONAL
- Lista de leads com filtros por localidade e segmento → FUNCIONAL
- KPIs diários de aquisição → PARCIAL (tela existe mas dados vazios)
- Leads com scoring schema (lead_scoring_rules) → SCHEMA PRONTO
- Meta/Google Ads com analytics e CAPI → FUNCIONAL
- UTM tracking e conversion events → SCHEMA PRONTO

**O que falta para Fase 0**:
- KPIs diários com data binding real (hoje mostra zeros)
- Lead scoring processor: cron que calcula scores baseado nas regras — **agnóstico ao canal**
- Cadências outbound funcionais (depende do motor de sequences de MA2)
- Dedup nativo sem HubSpot
- **Atribuição de canal**: rastrear de qual canal cada lead veio (first-touch + last-touch)
- **Conversão por canal**: qual canal gera mais leads, mais MQLs, mais deals fechados
- **Referral tracking**: mecanismo para registrar indicações de clientes existentes

**Gates**: G3 (parcial — depende de MA2 para sequences e dedup)

**Escopo Fase 0 (2 meses)**:
- [ ] KPIs diários com dados reais (leads trabalhados, demos agendadas, conversão)
- [ ] Lead scoring processor ativo (cron que calcula score por regras configuráveis)
- [ ] Score agnóstico ao canal: mesma fórmula aplica a lead outbound, inbound, referral, etc.
- [ ] Atribuição de canal: every lead tem `acquisition_channel` e `first_touch_source`
- [ ] Cadências outbound: BDR consegue executar sequence de email sem HubSpot
- [ ] Dedup nativo: verificação por domain + email + telefone
- [ ] Dashboard de aquisição: leads por canal, conversão por estágio, CAC por canal
- [ ] UTM tracking funcional: leads inbound com source/medium/campaign registrados
- [ ] Referral: campo para registrar "indicado por" (link com contact existente)

---

### MA4: Customer Success

**Escopo**: Gestão completa do ciclo de vida do cliente pós-venda. Foco em **confiabilidade operacional** — toda ação de CS deve gerar dados confiáveis e auditáveis.

**Módulos**: `/cs/*` (9 páginas), `/suporte/cliente/[orgId]` (15 páginas)

**Áreas operacionais principais**:
1. **Onboarding**: receber o cliente do pipeline de vendas e guiá-lo até o primeiro valor
2. **Atendimento (Suporte)**: resolução de problemas e tickets
3. **Monitoramento proativo**: health score, risk signals, alertas automáticos
4. **Renovações e expansão**: pipeline de renovações, upsell, cross-sell
5. **Gestão de churn**: retention offers, playbooks de recuperação

**Conexões profundas com outras áreas**:
- **Aquisição/Vendas → CS**: onboarding começa IMEDIATAMENTE após deal close. Não pode haver gap.
- **CS → Financeiro**: health score impacta decisões de billing (retention offers, descontos). Inadimplência alimenta risk signals.
- **CS → Suporte (Mensageria)**: resolução de tickets alimenta support score do health score. Volume e tempo de resolução são inputs diretos.
- **CS → Produto**: dados de uso do Studio alimentam product usage score. Feature adoption, login frequency, feature depth.

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
- **Onboarding automático**: trigger de deal close → tarefas de onboarding criadas
- **Support score wiring**: tickets resolvidos → atualização automática do health score

**Gates**: G2 (73%)

**Escopo Fase 0 (2 meses)**:
- [ ] 3 playbooks CS operando: onboarding (4-6 tarefas automáticas), cliente em risco (alerta + ações), NPS detractor (follow-up imediato)
- [ ] Health score calculado para 100% dos clientes ativos (cron diário)
- [ ] Pesos do health score validados com time de CS (D3 resolvido)
- [ ] Retention offers conectadas à UI: CS Manager calcula offer em 2 cliques
- [ ] Timeline 100% real (zero dados mock)
- [ ] Alerta automático Slack/Chat quando score < 40
- [ ] Customer 360 carrega em < 3 segundos para qualquer cliente
- [ ] Onboarding automático: deal fechado → playbook de onboarding dispara
- [ ] Renovações: pipeline com alertas 60/30/15 dias antes do vencimento
- [ ] Integração suporte → health score: tickets de suporte atualizam support score
- [ ] NPS: survey automático a cada 90 dias para clientes ativos
- [ ] Dados de uso do Studio integrados (se time Migração entregar)

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

**Escopo Fase 0 (2 meses)**:
- [ ] Histórico consolidado: dado um orgId, retornar TODAS as interações (WhatsApp + email + suporte) ordenadas
- [ ] Webhook de respostas WhatsApp processando e aparecendo no histórico
- [ ] Templates: UI de CRUD funcional, validada com CS e Vendas
- [ ] Chat interno: notificações de alertas do sistema (health score, integrações, etc.) — substituição gradual do Slack
- [ ] AI Classification: accuracy > 85% medida em amostra real
- [ ] Inbox unificado: assignedTo funcionando com roteamento por department
- [ ] Integração Customer 360 ↔ v2_comms.conversations (link por orgId)
- [ ] Email tracking: opens/clicks registrados e visíveis na timeline do deal
- [ ] Decisão sobre SMS (entra Fase 0 ou não)

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

**Escopo Fase 0 (2 meses)**:
- [ ] Marketing automations: TODAS as 8 ações funcionais (incluir create_deal, webhook, internal_notification)
- [ ] Lead scoring processor: cron ativo que calcula scores e marca leads para follow-up
- [ ] Monitoramento de crons: dashboard com status, última execução, falhas
- [ ] Inventário completo de automações externas (Zapier, Make) com plano de migração
- [ ] Workflow builder: pelo menos 3 workflows simples executando (ex: deal stage change → task, health score < 40 → alert, lead score > 80 → notify)
- [ ] Logs de execução acessíveis na UI para debugging
- [ ] Edge functions: inventário com status e documentação mínima
- [ ] Alerta automático quando cron falha (Slack/Chat/Sentry)

---

### MA7: Integrações

**Escopo**: Integrações externas (evoluindo de 13 para 9 com internalização), sync engine, monitoramento de saúde, documentação técnica.

**Módulos**: Transversal — afeta todos os módulos

**O que existe**:
- **Iugu**: sync estável (customers, subscriptions, invoices, financials, receivables, plans)
- **Stripe**: webhooks funcionando (subscription + invoice events)
- **Conta Azul**: sync estável (despesas, categorias) — **em processo de internalização (MA1)**
- **Banco Inter**: mTLS + OAuth implementado → pronto mas INATIVO (D12)
- **HubSpot**: sync ativo → **em deprecação, substituído por CRM package (MA-CRM)**
- **Evolution/Blip**: WhatsApp operando
- **Meta/Google Ads**: analytics funcionando, CAPI integrado
- **Golden Record**: entity resolution para dedup
- **Clicksign**: assinatura digital de contratos
- **Slack**: notificações outbound — **sendo gradualmente substituído por chat interno (MA5)**
- **Notion**: sync de documentação — **sendo substituído por wiki interna (MA12)**
- Sync engine com dual-write para v2_billing

**O que falta para Fase 0**:
- Monitoramento centralizado com alertas (Sentry/Slack)
- Documentação formal de cada integração (o que sincroniza, frequência, credenciais, fallback)
- SLA definido por sync
- Decisão sobre Banco Inter (D12)
- Plano de desligamento para integrações sendo internalizadas (HubSpot, Conta Azul)

**Gates**: G6 (85%)

**Escopo Fase 0 (2 meses)**:
- [ ] Dashboard de integrações: status, última sync, erros, latência
- [ ] Alerta automático (Sentry + Slack/Chat) quando integração falha
- [ ] SLA definido: cada sync tem frequência esperada e threshold de alerta
- [ ] Zero falhas críticas em 7 dias consecutivos
- [ ] Documentação: cada integração com ficha técnica (credenciais, frequência, tabelas, fallback)
- [ ] HubSpot: plano de desligamento com data-alvo e checklist de migração
- [ ] Conta Azul: plano de transição para contabilidade nativa (MA1)
- [ ] Banco Inter: decisão tomada (D12) — ativar ou postergar com justificativa
- [ ] Sync engine: dual-write validado sem perda de dados
- [ ] Inventário de credenciais: todas em vault seguro, nenhuma hardcoded

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

**Escopo Fase 0 (2 meses)**:
- [ ] /executive page: MRR, churn rate, novos clientes, receita, despesas, health score médio, pipeline — tudo numa tela
- [ ] Filtros transversais: período (mês, trimestre, ano), plano, ICP
- [ ] MetricaSnapshot: cron diário que snapshota métricas-chave para histórico
- [ ] Comparativo mês a mês: MRR atual vs anterior, churn trend, pipeline trend
- [ ] KPIs de aquisição com dados reais (binding funcional)
- [ ] Métricas de CS: health score médio, # clientes em risco, NPS score
- [ ] Drill-down: clicar em qualquer métrica leva ao módulo correspondente
- [ ] Aprovação formal por Ju e VDC (PO sign-off)
- [ ] Cohort básico: filtrar métricas por mês de aquisição do cliente

---

### MA9: RH & Produtividade

**Escopo**: Gestão de pessoas, org chart, dashboards de produtividade do time de desenvolvimento, performance tracking. Inclui a **migração do gestao.freelaw.ai** para dentro do Backstage.

**Módulos**: `/rh/*` (18 páginas)

**O que existe**:
- 18 páginas de RH parcialmente implementadas
- Org chart → PARCIAL
- Perfis de usuário (user_profiles) → FUNCIONAL
- gestao.freelaw.ai → É O BACKSTAGE (mesmo deploy Vercel — não é app separado, apenas domínio/rota diferente)

**O que falta para Fase 0**:
- **Migração do gestao.freelaw.ai**: inventário de funcionalidades, plano de migração, execução (D18)
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

**Escopo Fase 0 (2 meses)**:
- [ ] Inventário completo do gestao.freelaw.ai: funcionalidades, dados, usuários
- [ ] Plano de migração gestao.freelaw.ai → Backstage (D18 resolvido)
- [ ] Org chart funcional: hierarquia, reportes, departamentos
- [ ] Perfis de usuário completos: cargo, departamento, data de entrada, gestor
- [ ] Dashboard de produtividade dev: commits, PRs, cycle time (integração GitHub API)
- [ ] Visão de alocação: quem está em qual projeto/sprint
- [ ] 1:1s: template + registro de notas por período
- [ ] Goals: CRUD de metas por pessoa com tracking de progresso

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

**Escopo Fase 0 (2 meses)**:
- [ ] Dashboard de marketing: investimento, leads gerados, CPL, CAC por canal
- [ ] Campanhas Meta/Google: métricas atualizadas com < 24h defasagem
- [ ] CAPI: validar eventos de conversão (lead, deal, subscription) fluindo corretamente
- [ ] UTM tracking: leads inbound com source/medium/campaign registrados e visíveis no CRM
- [ ] Email marketing: campanha simples (selecionar lista, template, enviar, trackear)
- [ ] Newsletter: subscriber management + envio + métricas de abertura
- [ ] Analytics de canal consolidado: qual canal traz mais leads qualificados

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

**Escopo Fase 0 (2 meses)**:
- [ ] Onboarding de prestadores 100% no Backstage (zero dependência HubSpot)
- [ ] Pipeline de prestadores via CRM package (pipeline configurável tipo: provider_acquisition)
- [ ] Funnel com métricas: candidatos → triagem → aprovados → ativos por tier
- [ ] Saturação: dashboard por região e especialidade com alertas
- [ ] Ranking: algoritmo funcionando com inputs reais (qualidade, prazo, NPS do cliente)
- [ ] Pagamento a prestadores integrado com Financeiro (MA1) → journal entry no GL
- [ ] Perfil do prestador: histórico de trabalhos, tier atual, avaliações

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

**Design System — Trabalho de alta prioridade**:

O Design System é um pilar transversal que afeta TODAS as outras macro-áreas. Todo componente de UI do Backstage deve consumir tokens e componentes do design system. Este é um trabalho de escala significativa.

**Escopo do Design System**:
- **Design Tokens**: cores, tipografia, espaçamento, sombras, border-radius — definidos centralmente
- **Component Library**: botões, forms, modais, tabelas, cards, charts — todos consumindo tokens
- **Patterns**: layout patterns, navigation patterns, data display patterns
- **Designer's Bible**: documento de referência com guidelines visuais, do's and don'ts, exemplos
- **Aplicação**: TODAS as macro-áreas devem usar o design system (não é opcional)

> **Decisão D19**: Definir escopo do Design System para Fase 0. Considerar se deve ser
> elevado a gate próprio (G8), dado o impacto transversal em todas as áreas.

**Gates**: Nenhum — plataforma de suporte transversal (candidato a G8 para Design System)

**Escopo Fase 0 (2 meses)**:
- [ ] Design tokens documentados e implementados (Tailwind config centralizada)
- [ ] Componentes core: Button, Input, Select, Modal, Table, Card, Badge, Alert — todos com variants
- [ ] Designer's Bible v1: documento com guidelines visuais, paleta, tipografia, espaçamento (D19)
- [ ] Auditoria: listar todas as páginas que NÃO usam o design system
- [ ] Wiki: migração de conteúdo prioritário do Notion → wiki interna
- [ ] Audit logs: query por usuário, ação, recurso, período
- [ ] Feature flags: UI de gerenciamento funcional
- [ ] AI Agents: documentar capabilities e limitações de cada agente

---

### MA13: IA & Produção (LIA)

**Escopo**: IA centralizada da Freelaw — inclui produção de peças jurídicas para clientes (LIA), AI agents internos (Mastra), avaliação de ligações (Oitiva), e infraestrutura compartilhada de IA.

**Módulos**: `/ia/*` (a criar), `/oitiva/*` (8 páginas existentes)

**O que é LIA**:
LIA é a IA centralizada para clientes da Freelaw — produção de peças jurídicas, análise de documentos, pesquisa jurisprudencial assistida. A mesma infraestrutura pode (e deve ser avaliada se deve) servir necessidades internas: agents internos, insights automáticos, coaching de vendas.

**O que existe**:
- Oitiva Evaluation: avaliação de ligações com scorecards → EM DEV
- 4 AI Agents internos (Mastra): CEO IA, Maria Insights, Oitiva, Backstage Assistant → FUNCIONAL
- Infraestrutura de AI credits e ledger: v2_billing.ai_credits, ai_credits_ledger → FUNCIONAL

**Questão arquitetural (D16)**:
- **Opção A**: Infraestrutura compartilhada — mesma base de código, configurações e modelos servem tanto clientes (LIA) quanto uso interno (agents, insights). Vantagem: eficiência, consistência. Risco: complexidade, blast radius.
- **Opção B**: Infraestrutura separada com máximo reuso de código — LIA como serviço independente, agents internos como outro serviço, mas compartilhando packages de utils, types, prompt engineering. Vantagem: isolamento, deploy independente. Risco: duplicação.

**Gates**: Nenhum gate vinculado na Fase 0. Candidato a gate futuro.

**Escopo Fase 0 (2 meses)**:
- [ ] Decisão arquitetural (D16): compartilhar ou separar infraestrutura IA
- [ ] Oitiva: avaliação de ligações funcional com scorecards reais (integrado a MA2)
- [ ] AI Agents: documentação de capabilities, custos, limitações
- [ ] AI Credits: billing de uso de IA rastreável por cliente e por agent
- [ ] Inventário de necessidades de IA: quais áreas precisam de IA e para quê
- [ ] PoC LIA: produção de 1 tipo de peça jurídica com avaliação de qualidade
- [ ] Infraestrutura: definição do stack (modelos, hosting, rate limiting, custo por request)
- [ ] Coaching de vendas (via Oitiva): comparativo humano vs IA em avaliações

---

## 6. OS 7 GATES — ESPECIFICAÇÃO DE PRODUTO

> Cada gate é um marco de capacidade que cruza uma ou mais macro-áreas (seção 5).
> Use a tabela abaixo para navegar entre gates e macro-áreas.

| Gate | Descrição | Macro-Áreas | Completude |
|------|-----------|-------------|------------|
| G1 | Gestão financeira centralizada | MA1 (Financeiro) | 95% |
| G2 | Acompanhar Cliente v1 | MA4 (Customer Success) + MA-MGMT (CS cockpit) | 73% |
| G3 | Pipeline e CRM unificados | MA2 (Vendas) + MA3 (Aquisição) + MA11 (Prestadores) + MA-MGMT (Vendas cockpit) | 70% |
| G4 | Automações internas | MA6 (Automações) | 60% |
| G5 | Dashboard unificado | MA8 (Dashboard) + MA1 (Financeiro) + MA4 (CS) + MA-MGMT (Executivo cockpit) | 90% |
| G6 | Integrações estabilizadas | MA7 (Integrações) | 85% |
| G7 | Mensageria no monorepo | MA5 (Mensageria) | 85% |

### 6.1 G1: Gestão financeira centralizada

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
- General Ledger: precisa ser formalizado como hub central de todas as transações

**Critérios de aceite**:
- [ ] Toda operação financeira consultável no Backstage (nenhum dado só em planilha)
- [ ] Business Plan editável 100% na UI (import/export CSV como fallback)
- [ ] Sync Iugu + Conta Azul estável (7 dias sem falha)
- [ ] DRE, KPIs, Unit Economics com dados reais e atualizados
- [ ] Zero dependência de Google Sheets para dados financeiros
- [ ] General Ledger recebendo transações de todos os gateways

**Dependências técnicas**:
- Tabelas: v2_billing.*, sync.iugu_*, budgetCenarios, budgetDespesas, budgetMetas, mrrMovimentos, unitEconomicsSnapshots, vendor_category_mapping, fin_estruturas, fin_centros_custo
- APIs: /api/financeiro/* (30+ routes)
- Externas: Iugu (sync), Stripe (webhooks), Conta Azul (sync)

**Riscos**:
- Google Sheets elimination pode afetar workflow do CFO — validar antes de remover

---

### 6.2 G2: Acompanhar Cliente v1

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

### 6.3 G3: Pipeline e CRM unificados (matar HubSpot)

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

### 6.4 G4: Automações internas

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

### 6.5 G5: Dashboard unificado de métricas

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

### 6.6 G6: Integrações estabilizadas

**Declaração**: Todas as integrações externas estão monitoradas, documentadas, e operando sem falhas críticas. Integrações em processo de internalização (HubSpot, Conta Azul) têm plano de transição claro.

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
- [ ] Auditoria completa das integrações (status, última sync, erros)
- [ ] Monitoramento ativo com alertas Sentry/Slack para falhas
- [ ] Zero falhas críticas em 7 dias consecutivos
- [ ] Documentação de cada integração (o que sincroniza, frequência, credenciais, fallback)

**Dependências técnicas**:
- Tabelas: sync.* (todas as tabelas de sync externo)
- Infra: Sentry (monitoramento), Slack (alertas)

**Riscos**:
- Baixo — integrações já funcionam, falta formalizar monitoramento e documentação

---

### 6.7 G7: Mensageria no monorepo

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

## 7. ARQUITETURA DE DADOS — SCHEMAS

### 7.1 Mapa de schemas

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

### 7.2 Fluxo de Sequences (Cadências de Email)

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

### 7.3 Entidades que faltam para a visão do Gab

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

---

## 8. VISÃO PÓS-FASE 0 (Ju)

> Esta seção documenta a visão estratégica da CEO (Ju) para o Backstage APÓS a Fase 0.
> Nenhum item desta seção entra no escopo atual. São pré-requisitos futuros que
> a Fase 0 deve habilitar.

### 8.1 Dados unificados com cohortização

**Visão**: Qualquer métrica da empresa pode ser segmentada por cohort (mês de aquisição), plano, ICP, canal de aquisição. Exemplo: "MRR dos clientes que entraram em janeiro no plano Professional via inbound."

**O que existe**: Churn cohort em /vendas/analytics. Filtros em vários módulos mas não transversais.

**O que a Fase 0 habilita**: Customer 360 (G2) + dashboard unificado (G5) + CRM nativo (G3) criam a base de dados necessária. Cohortização transversal é Fase 1.

### 8.2 Cadências automáticas omni-channel

**Visão**: Cadências de vendas e CS que combinam email + WhatsApp + ligação, operadas por humanos OU por IA, com comparativos de performance.

**O que existe**: Schema de sequences com suporte multi-canal. Evolution API para WhatsApp. Resend para email.

**O que a Fase 0 habilita**: G3 (sequences com motor de execução) + G7 (mensageria consolidada) criam a infraestrutura. IA para cadências é Fase 1+.

### 8.3 IA por área

**Visão**: Cada área da empresa tem IA aplicada: coaching de SDRs/Closers com comparativo humano vs IA, pipeline de conteúdo marketing com IA, health score inteligente com tratativa automatizada.

**O que existe**: 4 AI agents (Mastra), Oitiva para avaliação de ligações, ChurnRiskSignal.

**O que a Fase 0 habilita**: G2 (health score funcionando) + G4 (automações) + Oitiva (avaliações) criam os dados necessários. IA coaching e comparativos são Fase 1+.

### 8.4 Flywheel operacional

**Visão**: O Backstage como flywheel — dados de uso alimentam health score, health score aciona CS, CS melhora retenção, retenção aumenta LTV, LTV justifica mais investimento em aquisição.

**Pré-requisitos da Fase 0**: Todos os 7 gates fechados. Dados fluindo end-to-end. Sem fragmentação.

---

## 9. DÉBITO TÉCNICO

### 9.1 Inventário

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

### 9.2 Estratégia

> **NÃO** tratar débito técnico de forma horizontal na Fase 0.
> Apenas corrigir nos módulos que estamos tocando para fechar os gates.
> Débito horizontal fica para Fase 1.

Exceção: se uma route sem auth for pública e expor dados sensíveis, corrigir imediatamente.

### 9.3 Routes sem auth — priorização

A priorização deve ser feita pelo time na Sprint 1 (decisão D6). Critério sugerido:
1. Routes públicas que expõem dados de clientes → P0
2. Routes que escrevem dados (POST/PUT/DELETE) sem auth → P1
3. Routes internas de leitura → P2 (pode esperar)

---

## 10. DECISÕES PENDENTES

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
| D15 | CRM package: quais pipelines configuráveis por domínio (vendas, prestadores, CS, investidores)? | VDC + Time | Sprint 1 | Pendente |
| D16 | LIA: compartilhar infraestrutura IA entre cliente e interno, ou separar? | VDC + Produto | Sprint 1 | Pendente |
| D17 | Documentação completa da arquitetura de dados (schemas, ownership, fluxos) | VDC + Time | Sprint 1 | Pendente |
| D18 | Plano de migração gestao.freelaw.ai → Backstage MA9 | VDC + RH | Sprint 1 | Pendente |
| D19 | Design System: escopo Fase 0 e aplicação do Designer's Bible | VDC + Design | Sprint 1 | Pendente |
| D20 | Validação em produção: plano de testes para todos os módulos "FUNCIONAL" | VDC + QA | Sprint 1 | Pendente |
| D21 | Unificar 3 taxonomias (route domains, sidebar sections, ColaboradorAreas) sob macro-áreas | VDC + Produto | Sprint 1 | Pendente |
| D22 | Management OS: escopo MVP e roles iniciais para AI Cockpit | VDC + Produto | Sprint 1 | Pendente |
| D23 | CEO AI: quais KPIs vitais v1 por área + definições únicas | VDC + Lideranças | Sprint 1 | Pendente |
| D24 | Pulses: templates por área + cadência (diário vs semanal vs amostragem) | VDC + Time | Sprint 1 | Pendente |
| D25 | LIA vs CEO AI: compartilhar infraestrutura ou separar? | VDC + Produto | Sprint 1 | Pendente |

---

## 11. MÉTRICAS DE SUCESSO

### 11.1 Fase 0 (quantitativas)

- [ ] 7/7 gates fechados e aprovados por VDC + Ju
- [ ] Zero dependência de CRM externo como fonte de verdade
- [ ] 100% dos dados de clientes consultáveis no Backstage
- [ ] Health score calculado para 100% dos clientes ativos
- [ ] Dashboard executivo operando (aprovado pela liderança)
- [ ] HubSpot sync desligado
- [ ] Zero falhas críticas de integração em 7 dias
- [ ] 3 playbooks CS operando
- [ ] Todos os módulos marcados "FUNCIONAL" validados em produção (D20)
- [ ] Ciclo de vida completo do cliente rastreável (primeiro touchpoint → status atual)

### 11.2 Norte pós-Fase 0 (qualitativas)

- Cadências automáticas (email + WhatsApp + ligação) operando
- Sistema omni-channel funcional com timeline unificada
- IA para coaching de SDRs/Closers com comparativo humano vs IA
- Pipeline de conteúdo marketing com IA
- Health score inteligente com tratativa automatizada
- Cohortização transversal de qualquer métrica
- Flywheel operacional rodando
- LIA (IA para clientes) integrada à plataforma
- Contabilidade 100% nativa (Conta Azul desligado)
- Comunicação interna 100% no Backstage (Slack desligado)

---

## 12. GOVERNANÇA

### Regra de ouro

> Se alguém do time quer fazer algo que não está neste documento, a resposta é:
> "Adiciona no PRD, aprova com o owner, e aí a gente faz."

### Validação em produção

> **ATENÇÃO**: Todos os elementos marcados como "FUNCIONAL" neste PRD precisam ser verificados
> com testes em produção. Status funcional sem validação em produção é hipótese, não fato.
> O PO deve testar cada critério de aceite em ambiente de produção antes de considerar um gate fechado.
> Ver D20 para plano de testes.

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
| 2026-03-09 | v6 | Rewrite + auditoria de código: internalização (HubSpot/Conta Azul/Notion/Slack), MA13 LIA, Arquitetura de Dados com GL real (85 contas, 5 posting engines), ciclo de vida do cliente, CRM investidores, 27 packages documentados, aquisição multicanal, CS operacional, escopo 2 meses todas MAs, validação produção, desalinhamento domínios (3 taxonomias), D16-D21 | Guilherme + Claude |
| 2026-03-09 | v7 | Management OS (MA-MGMT): package transversal com AI Cockpit personalizado por role, CEO AI (sensemaking/orquestração/cobrança), Pulses (daily/weekly qualitativo + áudio), Weekly Reports automáticos, Combinados rastreáveis, Knowledge Schema, ritmos de gestão. Visão "empresa powered by AI" no contexto estratégico. Gates G2/G3/G5 conectados ao MA-MGMT. D22-D25 | Guilherme + Claude |
