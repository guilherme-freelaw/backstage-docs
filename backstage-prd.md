# BACKSTAGE FREELAW — Super PRD v2

> Documento bounding para o time de Backstage.
> Toda decisão, construção e priorização deve estar ancorada aqui.
> Se não está neste documento, não existe.

**Última atualização**: 2026-03-09
**Owner**: VDC | **Líder de execução**: Guilherme
**Time**: Alex, Biel, Clarinha, Davi
**Prazo hard**: Fim de Abril 2026
**URL produção**: backstage.freelaw.ai

---

## 1. CONTEXTO E POSICIONAMENTO

### O que é o Backstage
Plataforma interna da Freelaw. Enquanto o Studio Offices atende o contratante e o Studio Providers atende o prestador, o **Backstage conecta todas as pontas** — centraliza gestão operacional, financeira, comercial e de pessoas.

### Estado real do código (mapeado 2026-03-09)

| Métrica | Valor |
|---------|-------|
| Pages (Next.js) | 299 |
| API Routes | 167+ |
| TSX Files | 431 |
| Database Tables | 40+ (Backstage) + 50+ (Infra DB) |
| Feature Modules | 23 |
| Integrações externas | 14 |
| AI Agents | 4 |

**Conclusão**: O Backstage já tem MUITA coisa construída. O problema não é "construir do zero" — é **terminar, conectar e estabilizar** o que existe.

### Princípio operacional
> "Isso ajuda a fechar o gate da Fase 0?" — Se não, vai para o backlog.

---

## 2. O QUE JÁ EXISTE (INVENTÁRIO REAL)

### 2.1 Módulos do Backstage (UI)

| Módulo | Rota | Status | Notas |
|--------|------|--------|-------|
| **Vendas** | /vendas/* | Funcional | Deals, pipeline, leads, performance, analytics, comissões |
| **Aquisição** | /aquisicao/* | Funcional | BDR/SDR, KPIs diários, lead hunter, metas |
| **Financeiro** | /financeiro/* | Funcional | Receita, custos, DRE, MRR, LTV, churn, forecast, budget |
| **CRM** | /crm/* | Funcional | Contatos, empresas, negócios, atividades, sequences |
| **CS** | /cs/* | Parcial | Health score, churn, NPS, renewals, journey |
| **Suporte** | /suporte/* | Parcial | Inbox, WhatsApp, chatbot, analytics, knowledge base |
| **Prestadores** | /prestadores/* | Funcional | Capacidade, ranking, KPIs, base de conhecimento |
| **RH** | /rh/* | Parcial | Org chart, recrutamento |
| **Chat interno** | /chat | Funcional | Canais, DMs, threads |
| **Wiki** | /wiki | Funcional | Knowledge base interna |
| **Oitiva** | /oitiva/* | Em dev | Avaliações, scorecards, transcrições |
| **Produto** | /produto/* | Funcional | Design system, academy, audit logs, workflows, agents |
| **Planejamento** | /planejamento | Parcial | OKRs, forecasts |
| **Marketing** | /marketing/* | Parcial | Ferramentas de marketing |
| **Dashboard** | /dashboard | Funcional | Dashboard principal |

### 2.2 Database Schemas (Drizzle ORM)

**Tabelas backstage-específicas** (src/db/schema/):
- users, sales_people, funnels, deals, goals, activities
- financeiro (vendor_category_mapping, fin_estruturas, fin_centros_custo)
- hubspot_* (DEPRECATED - migrando para v3.4)
- chat, chatbots, clicksign, org_chart
- planejamento, reports, wiki, support, imports, integrations

**Tabelas infra (re-exported de @freelaw/infra-db)**:
- v2_sales.* — Dados de vendas
- v2_sync.* — Sync financeiro (Iugu)
- v2_billing.* — 24 tabelas de billing
- v2_customers.* — Dados de clientes
- v2_subscriptions.* — Subscrições
- v2_invoices.* — Faturas

### 2.3 Integrações externas (14)

| Integração | Função | Status |
|------------|--------|--------|
| HubSpot | CRM (16 entidades) | Ativo - migrando p/ interno |
| Stripe | Billing (clientes novos) | Ativo - primário |
| Iugu | Billing (legado) | Ativo - sync via ETL |
| Omie | ERP | Ativo |
| Meta Ads | Marketing | Ativo |
| Google Ads | Marketing | Ativo |
| Conta Azul | Contabilidade | Ativo |
| Evolution | WhatsApp | Ativo |
| Blip | Chatbot | Ativo |
| Banco Inter | Banking (mTLS+OAuth) | Ativo |
| Notion | Docs | Ativo |
| Slack | Comunicação | Ativo |
| Golden Record | Entity resolution | Ativo |
| Clicksign | Assinatura digital | Ativo |

### 2.4 AI Agents (Mastra)

| Agent | Função |
|-------|--------|
| CEO IA | Insights executivos, decisões estratégicas |
| Maria Insights | Análise financeira |
| Oitiva Evaluation | Análise de performance |
| Backstage Assistant | Assistente geral |

### 2.5 Domínios no backstage-core (packages/backstage-core/)

| Domínio | O que tem |
|---------|-----------|
| Finance | Subscriptions (Stripe+Iugu), MRR, DRE, Budget, Cashflow, Unit Economics |
| Leads | Lead Hunter (prospecção B2B com IA), filtros, Google Places |
| Comunidade | Provider funnel, tiers (S+ a D), journey phases |
| Retention | Churn risk signals, health score, risk levels, trends |
| Marketing | Ad campaigns, content analytics, channel performance |

---

## 3. GATES DA FASE 0 — MAPEADOS CONTRA O CÓDIGO

### G1: Gestão operacional e financeira centralizada

**O que já existe:**
- Módulo financeiro completo (/financeiro/*) com DRE, MRR, custos, receita
- 30+ API routes de financeiro
- Sync com Iugu e Stripe funcionando
- Budget e forecast parcialmente implementados

**O que falta para fechar o gate:**
- [ ] Validar que TODA operação financeira está consultável no Backstage
- [ ] Eliminar dependência de planilhas externas
- [ ] Garantir que dados de Omie/Conta Azul estão sincronizados
- [ ] Dashboard financeiro unificado (um lugar só para ver tudo)

**Estimativa**: 2-3 semanas | **Complexidade**: Média

---

### G2: Acompanhar Cliente v1

**O que já existe:**
- Rota /cs/* com health score, churn, NPS, renewals, journey
- CustomerHealthScore no backstage-core (overall + component scores)
- ChurnRiskSignal com scoring e ações recomendadas
- Dados de uso do Studio via analytics

**O que falta para fechar o gate:**
- [ ] Dashboard por cliente funcional (uso, saúde, interações em uma tela)
- [ ] Health score calculado e visível para todos os clientes ativos
- [ ] Histórico de interações consolidado (WhatsApp + email + ligações + suporte)
- [ ] Alertas automáticos para clientes em risco

**Estimativa**: 2-3 semanas | **Complexidade**: Média-Alta

---

### G3: Pipeline e gestão de clientes unificados

**O que já existe:**
- CRM completo (/crm/*) com contatos, empresas, negócios, atividades, sequences
- Sales funnel management (BDR/SDR/Closer)
- Deal tracking + pipeline (/vendas/*)
- HubSpot sync ativo (mas em deprecação)

**O que falta para fechar o gate:**
- [ ] Migrar 100% dos dados do HubSpot para o Backstage
- [ ] Desligar dependência do HubSpot como fonte de verdade
- [ ] Garantir que todo o funil de vendas opera dentro do Backstage
- [ ] Sequences (cadências de email) funcionando end-to-end

**Estimativa**: 3-4 semanas | **Complexidade**: Alta (migração de dados + mudança de processo)

---

### G4: Automações internas

**O que já existe:**
- Workflow builder (/produto/workflows/*)
- 26+ cron jobs definidos
- Sequences (cadências) parcialmente implementados
- 58 Edge Functions (Deno)

**O que falta para fechar o gate:**
- [ ] Inventariar TODAS as automações que rodam fora do Backstage
- [ ] Migrar automações críticas para dentro (workflows ou crons)
- [ ] Eliminar ferramentas de automação terceirizadas
- [ ] Monitoramento de automações (falhas, execuções)

**Estimativa**: 2-3 semanas | **Complexidade**: Média

---

### G5: Dashboard unificado de métricas

**O que já existe:**
- /dashboard principal
- /financeiro/analise/* (churn, analytics, unit economics)
- /vendas/analytics/* (CRM, churn cohort, form analysis, plan mix)
- /aquisicao/kpis-diarios
- PostHog integrado

**O que falta para fechar o gate:**
- [ ] UM dashboard que mostra: MRR, churn, aquisição, operação, saúde de clientes
- [ ] Métricas em tempo real (não batch diário)
- [ ] Filtros por período, cohort, plano, ICP
- [ ] Acesso rápido para liderança (Gab, VDC, Didico)

**Estimativa**: 2 semanas | **Complexidade**: Média

---

### G6: Integrações estabilizadas

**O que já existe:**
- 14 integrações ativas
- Sync engine operando
- Dual-write para v2_billing

**O que falta para fechar o gate:**
- [ ] Auditoria de TODAS as integrações (quais falham? quais estão atrasadas?)
- [ ] Monitoramento ativo com alertas
- [ ] Resolver pendências críticas identificadas na auditoria
- [ ] Documentar cada integração (o que sincroniza, frequência, fallback)

**Estimativa**: 1-2 semanas | **Complexidade**: Média

---

### G7: Mensageria no monorepo

**O que já existe:**
- WhatsApp via Evolution + Blip
- /suporte/whatsapp
- Email routes (6+ endpoints)
- Chat interno funcional

**O que falta para fechar o gate:**
- [ ] WhatsApp business integrado e operando dentro do monorepo
- [ ] Email transacional e marketing operando internamente
- [ ] Histórico de todas as mensagens consolidado por cliente/lead
- [ ] Templates de mensagem gerenciáveis pelo time

**Estimativa**: 2-3 semanas | **Complexidade**: Média-Alta

---

## 4. VISÃO DO GAB — MAPEADA CONTRA O QUE EXISTE

### 4.1 Estrutura de dados unificada com cohortização

| Requisito | Status | Onde está |
|-----------|--------|-----------|
| Dados unificados | Parcial | v2_customers + v2_billing + deals. Falta consolidar. |
| Cohortização | Parcial | /vendas/analytics/churn-cohort existe. Falta generalizar. |
| Segmentação | Parcial | Filtros existem em vários módulos. Falta ser transversal. |

**Gap principal**: Não existe uma "visão 360 do cliente" única. Dados estão em deals, customers, subscriptions, support tickets, separados.

### 4.2 Cadências automáticas/semi-automáticas

| Canal | Humano | IA | Status |
|-------|--------|-----|--------|
| Ligação | /crm/atividades | Não existe | Tarefa agendada existe, IA não |
| WhatsApp | /suporte/whatsapp | Chatbot básico | Parcial |
| Email | /crm/sequences | Não existe | Sequences parcial, IA não |

**Gap principal**: Cadências existem como conceito (/crm/sequences) mas não estão conectadas end-to-end. IA para cadências não existe.

### 4.3 Sistema omni-channel

| Canal | Status | Onde |
|-------|--------|------|
| WhatsApp | Parcial | /suporte/whatsapp + Evolution/Blip |
| Email | Parcial | API routes existem, UI parcial |
| Ligação | Não existe | Apenas registro manual |
| Chat interno | Funcional | /chat |

**Gap principal**: Canais existem separados. Não há visão unificada de "todas as interações com este cliente/lead".

### 4.4 IA por área

| Área | O que existe | O que falta |
|------|-------------|-------------|
| Vendas | Maria Insights (financeiro), Lead Hunter | IA coaching SDR/Closer, comparativo humano vs IA |
| Marketing | Nada específico | Pipeline conteúdo, SEO, mídia paga com IA |
| CS | ChurnRiskSignal, HealthScore | Tratativa inteligente, onboarding com IA |

**Gap principal**: IA existe como infraestrutura (Mastra, 4 agents) mas não está aplicada para coaching, automação de cadências, ou comparativos.

---

## 5. PROBLEMAS TÉCNICOS CRÍTICOS (DÉBITO)

### Segurança (P0)
- **~167 API routes sem autenticação** — CRÍTICO
- Vulnerabilidades CSRF em OAuth flows
- Sem rate limiting
- 70 arquivos com @ts-nocheck

### Qualidade de código
- 92 instâncias de z.any() ou `as any`
- 83 console.log (deveria usar logger)
- 30 páginas sem error handling
- 27 páginas sem loading states
- 13 páginas sem empty states

### Decisão: Tratar débito técnico na Fase 0?
> **Recomendação**: NÃO tratar débito técnico de forma horizontal. Apenas corrigir nos módulos que estamos tocando para fechar os gates. Débito horizontal fica para Fase 1.

---

## 6. ARQUITETURA DE DADOS — REAL vs ALVO

### 6.1 Estrutura real (código)

```
BACKSTAGE DB SCHEMAS (apps/backstage/src/db/schema/):
├── deals.ts          → Deal tracking, pipeline, pricing
├── sales-people.ts   → BDR, SDR, Closer, canais
├── goals.ts          → Metas mensais/trimestrais
├── activities.ts     → Métricas diárias de vendas
├── funnels.ts        → Definição de funis
├── financeiro.ts     → Vendor mapping, cost centers
├── hubspot.ts        → DEPRECATED - sync legado
├── chat.ts           → Chat interno
├── chatbots.ts       → Configuração chatbots
├── clicksign.ts      → Assinatura digital
├── org-chart.ts      → Hierarquia organizacional
├── planejamento.ts   → Planejamento e forecast
├── reports.ts        → Relatórios
├── support.ts        → Tickets de suporte
├── user-profiles.ts  → Perfis de usuário
├── users.ts          → Autenticação
├── wiki.ts           → Knowledge base
└── imports.ts        → Histórico de imports

INFRA DB (re-exportado de @freelaw/infra-db):
├── v2_sales.*          → Dados de vendas
├── v2_sync.*           → Sync financeiro (Iugu)
├── v2_billing.*        → 24 tabelas (subscriptions, invoices, credits, usage...)
├── v2_customers.*      → Dados de clientes
├── v2_subscriptions.*  → Subscrições
└── v2_invoices.*       → Faturas
```

### 6.2 Entidades que FALTAM para a visão do Gab

```
PRECISAM SER CRIADAS OU EXPANDIDAS:

Cadência (cadences)
├── tipo (ligação/email/whatsapp)
├── modo (humano/IA)
├── destinatário_type (lead/cliente)
├── destinatário_id
├── sequência de steps
├── status (draft/active/paused/completed)
├── métricas (abertura, resposta, conversão)
└── created_by, assigned_to

InteraçãoUnificada (unified_interactions)
├── canal (whatsapp/email/ligação/chat/suporte)
├── direção (inbound/outbound)
├── entidade_type (lead/cliente/prestador)
├── entidade_id
├── participantes
├── conteúdo/resumo
├── sentimento (IA)
├── cadência_id (se veio de cadência)
└── tags/classificação

ClienteVisão360 (VIEW, não tabela)
├── dados cadastrais (de v2_customers)
├── plano/pricing (de v2_subscriptions)
├── health score (calculado)
├── última interação (de unified_interactions)
├── métricas de uso (de analytics)
├── financeiro (de v2_billing)
├── cohort/segmentação
└── risco de churn (de retention engine)

MetricaSnapshot (metric_snapshots)
├── tipo (MRR, churn, aquisição, NPS, health)
├── período (dia/semana/mês)
├── valor
├── segmentação (cohort, plano, ICP)
└── comparativo (meta vs real)
```

---

## 7. ESTRATÉGIA DE EXECUÇÃO

### 7.1 Princípios

1. **Gate-driven**: Tudo fecha um gate. Se não fecha, não fazemos.
2. **Terminar > Começar**: Priorizar terminar módulos existentes sobre criar novos.
3. **Data-first**: Estrutura de dados antes de UI.
4. **Vertical**: Módulo completo end-to-end antes de expandir.
5. **Demo semanal**: Toda sexta, algo novo funciona.

### 7.2 Cronograma (8 semanas: 10/mar — 30/abr)

```
SPRINT 1 (10-21 mar): FUNDAÇÃO + G6
├── Semana 1: Auditoria completa
│   ├── Mapear estado real de cada módulo
│   ├── Inventariar ferramentas externas
│   ├── Auditar integrações (quais falham?)
│   ├── Alinhar dependências com Migração (Juju)
│   └── VALIDAR este PRD com VDC e Gab
│
├── Semana 2: Estabilizar integrações (G6) + iniciar G1
│   ├── Resolver falhas de integração críticas
│   ├── Monitoramento ativo com alertas
│   ├── Documentar cada integração
│   └── Iniciar consolidação financeira
│
└── DEMO sexta 21/mar: Integrações estabilizadas + plano detalhado

SPRINT 2 (24 mar - 4 abr): G1 + G5
├── Semana 3: Gestão financeira centralizada (G1)
│   ├── Validar que toda operação financeira está no Backstage
│   ├── Eliminar planilhas externas
│   ├── Sync Omie/Conta Azul estabilizado
│   └── G1 FECHADO
│
├── Semana 4: Dashboard unificado (G5)
│   ├── Dashboard executivo: MRR, churn, aquisição, operação
│   ├── Filtros por período, cohort, plano
│   ├── Acesso rápido para liderança
│   └── G5 FECHADO
│
└── DEMO sexta 4/abr: Financeiro centralizado + dashboard

SPRINT 3 (7-18 abr): G2 + G3
├── Semana 5: Acompanhar Cliente v1 (G2)
│   ├── Dashboard por cliente (uso, saúde, interações)
│   ├── Health score calculado para todos
│   ├── Histórico de interações consolidado
│   └── G2 FECHADO
│
├── Semana 6: Pipeline unificado (G3)
│   ├── Migrar dados do HubSpot
│   ├── Funil de vendas 100% interno
│   ├── Sequences funcionando
│   └── G3 FECHADO
│
└── DEMO sexta 18/abr: Visão do cliente + pipeline migrado

SPRINT 4 (21-30 abr): G4 + G7 + POLISH
├── Semana 7: Automações (G4) + Mensageria (G7)
│   ├── Migrar automações críticas
│   ├── WhatsApp + Email no monorepo
│   ├── Templates de mensagem
│   └── G4 e G7 FECHADOS
│
├── Semana 8: Buffer + polish + validação final
│   ├── Testes end-to-end de todos os gates
│   ├── UX review com stakeholders
│   ├── Correções finais
│   └── TODOS OS 7 GATES FECHADOS
│
└── DEMO sexta 25/abr: Backstage completo — gate review

```

### 7.3 Atribuição por pessoa (sugestão inicial)

| Pessoa | Foco principal | Gates |
|--------|---------------|-------|
| Guilherme | Liderança, arquitetura, data layer, PRD | Todos (horizontal) |
| Alex | Financeiro, integrações, dashboard | G1, G5, G6 |
| Biel | CRM, pipeline, vendas, sequences | G3, G4 |
| Clarinha | CS, health score, cliente 360 | G2 |
| Davi | Mensageria, WhatsApp, automações | G7, G4 |

### 7.4 Dependências críticas com outros times

| Dependência | Time | Gate impactado | Quando precisa |
|-------------|------|----------------|----------------|
| Monorepo estável | Migração (Juju) | Todos | Semana 1 |
| Dados do Studio sincronizando | Migração | G2, G5 | Semana 3 |
| Definição de planos/pricing | GTM (Didico) | G1, G3 | Semana 2 |
| Definição de ICP | GTM | G2, G3 | Semana 2 |
| Métricas de uso do Studio | Migração | G2, G5 | Semana 4 |

---

## 8. RITUAIS DO TIME

| Ritual | Quando | Duração | Output |
|--------|--------|---------|--------|
| Daily standup | Diário, 9h | 15min | Bloqueios + foco do dia |
| Sprint planning | Segunda | 45min | Tarefas da semana atribuídas |
| Comitê Produto & Backstage | Quinta | 60min | Decisões sobre dependências |
| Demo & Report | Sexta (empresa toda) | 30min | O que entregamos esta semana |
| PRD review | Bi-semanal (segunda) | 30min | Atualizar este documento |

---

## 9. DECISÕES PENDENTES

| # | Decisão | Responsável | Deadline | Status |
|---|---------|-------------|----------|--------|
| D1 | Quais ferramentas externas eliminar (lista completa) | Time | Sem 1 | Pendente |
| D2 | Prioridade dos gates (ordem proposta acima OK?) | VDC + Gab | Sem 1 | Pendente |
| D3 | Health score: quais métricas compõem? | CS + Produto | Sem 3 | Pendente |
| D4 | HubSpot: timeline de desligamento | Vendas + Backstage | Sem 2 | Pendente |
| D5 | Stack de mensageria final (Evolution vs outro) | Time | Sem 2 | Pendente |
| D6 | Débito técnico (167 routes sem auth): tratar agora? | Gab + VDC | Sem 1 | Pendente |
| D7 | Stakeholders para aprovação de UX | Gab | Sem 1 | Pendente |
| D8 | Atribuição final do time (quem faz o quê) | Guilherme | Sem 1 | Pendente |

---

## 10. RISCOS

| Risco | Prob. | Impacto | Mitigação |
|-------|-------|---------|-----------|
| Migração atrasa e bloqueia Backstage | Alta | Crítico | Comitê quinta. Trabalhar em paralelo onde possível. |
| Escopo cresce além dos 7 gates | Média | Alto | Este doc é bounding. Se não está aqui, não entra. |
| HubSpot migration mais complexa que esperado | Alta | Alto | Começar mapeamento na semana 1. Plano B: dual-run. |
| 167 API routes sem auth exploradas | Média | Crítico | Avaliar risco real. Priorizar routes públicas críticas. |
| Time sobrecarregado com suporte ao legado | Média | Alto | Zero construção no legado. |

---

## 11. MÉTRICAS DE SUCESSO

### Fase 0 (até fim de abril)
- [ ] 7/7 gates fechados
- [ ] Zero dependência de CRM externo como fonte de verdade
- [ ] 100% dos dados de clientes consultáveis no Backstage
- [ ] Dashboard executivo operando em tempo real
- [ ] Stakeholders aprovaram UX dos módulos principais

### Norte pós-Fase 0 (visão Gab)
- Cadências automáticas (ligação, email, WhatsApp) operando
- Sistema omni-channel funcional
- IA para coaching de SDRs/Closers
- Comparativo humano vs IA em uso
- Pipeline de conteúdo marketing com IA
- Health score inteligente com tratativa automatizada

---

## CHANGELOG

| Data | Alteração | Autor |
|------|-----------|-------|
| 2026-03-09 | v1 — Documento criado | Guilherme + Claude |
| 2026-03-09 | v2 — Atualizado com mapeamento real do código (inventário completo) | Guilherme + Claude |

---

> **Regra de ouro**: Se alguém do time quer fazer algo que não está neste documento, a resposta é: "Adiciona no PRD, aprova com o owner, e aí a gente faz."
