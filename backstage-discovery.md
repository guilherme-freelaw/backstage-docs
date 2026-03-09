# BACKSTAGE — DISCOVERY: Estado Ideal de Cada Miniapp

> **Objetivo**: Definir o "estado legal" de cada miniapp do Backstage.
> Cada pessoa do time investiga suas miniapps e preenche: o que está pronto,
> o que falta, e como se parece quando está ENTREGUE.
>
> Este documento é o INPUT para o plano de execução.
> Sem discovery completa, não tem kickoff.

**Status**: EM DISCOVERY
**Deadline de preenchimento**: [DEFINIR NO KICKOFF]
**Responsável geral**: Guilherme

---

## COMO USAR ESTE DOCUMENTO

Para cada miniapp:

1. **Abra o Backstage em staging/produção** e navegue pela miniapp
2. **Abra o código** e leia o que existe vs o que é stub
3. **Preencha as 4 seções**:
   - ESTADO ATUAL: o que funciona hoje de verdade
   - ESTADO IDEAL: como essa miniapp se parece quando está "pronta" para a Fase 0
   - GAP: a diferença entre atual e ideal
   - DECISÕES: o que precisa ser decidido antes de construir

4. **Classifique o esforço**: P (pequeno, <3 dias), M (médio, 3-10 dias), G (grande, >10 dias)

---

## MINIAPPS CORE (Gates da Fase 0)

Estas miniapps são diretamente ligadas aos 7 gates. São PRIORIDADE.

---

### 1. FINANCEIRO `/financeiro`
**Gate**: G1 (Gestão financeira centralizada) + G5 (Dashboard)
**Investigador**: _______________
**28 páginas | 30+ API routes**

**ESTADO ATUAL** (pré-preenchido pelo mapeamento de código):
- Receita: overview, assinaturas, faturamento, billing, novo-MRR → FUNCIONAL
- Custos: despesas (Conta Azul), prestadores, nova-sede → FUNCIONAL
- Relatórios: DRE, balanço, DFC, KPIs, auditoria, relatório diário → FUNCIONAL
- Análise: churn, inadimplência, unit economics, analytics → FUNCIONAL
- Planejamento: business plan (com import/export Google Sheets), estratégico → FUNCIONAL
- Configurações: categorias, centros de custo → FUNCIONAL
- Conciliação: Iugu vs GL → FUNCIONAL
- ERP (Freelaw One) → PLACEHOLDER ("em construção")
- Recebimentos → STUB
- Estruturas organizacionais → STUB ("em breve")

**ESTADO IDEAL** (preencher):
```
[ ] O que precisa funcionar para considerar "entregue"?
[ ] Quem vai usar essa miniapp no dia a dia?
[ ] Quais decisões de negócio dependem dos dados daqui?
[ ] Google Sheets pode continuar como ferramenta auxiliar ou precisa ser eliminado?
[ ] ERP (Freelaw One) entra na Fase 0 ou fica para depois?
```

**GAP** (preencher após investigar):
```
[ ] Lista de funcionalidades faltantes
[ ] Lista de bugs/problemas encontrados
[ ] Dependências externas que precisam ser resolvidas
```

**DECISÕES PENDENTES**:
- [ ] ERP entra na Fase 0? (afeta escopo significativamente)
- [ ] Google Sheets: eliminar ou conviver?
- [ ] Recebimentos: precisa estar pronto?

**Esforço estimado**: ___ (P/M/G)

---

### 2. CRM `/crm`
**Gate**: G3 (Pipeline e CRM unificados)
**Investigador**: _______________
**12 páginas**

**ESTADO ATUAL**:
- Negócios (Deals): CRUD funcional, leitura de v2_sales → FUNCIONAL
- Empresas (Companies): CRUD funcional → FUNCIONAL
- Contatos (Contacts): CRUD funcional → FUNCIONAL
- Atividades (Activities): timeline unificada → FUNCIONAL
- Sequences: UI existe, dados vêm do HubSpot sync → PARCIAL (dependente HubSpot)
- Providers: pipeline de prestadores → DEPENDENTE HubSpot
- Dashboard: métricas de CRM → FUNCIONAL
- Configurações: settings do CRM → PARCIAL

**ESTADO IDEAL**:
```
[ ] CRM 100% independente do HubSpot?
[ ] Sequences precisam ser criadas e executadas nativamente?
[ ] Qual o fluxo completo de um lead: da descoberta ao fechamento?
[ ] Provider onboarding: como funciona sem HubSpot?
[ ] Quais campos/dados do HubSpot NÃO existem no v2_sales?
[ ] Importação de dados históricos do HubSpot: necessária?
```

**GAP**:
```
[ ]
```

**DECISÕES PENDENTES**:
- [ ] Timeline de desligamento do HubSpot
- [ ] Sequences: construir motor de execução agora ou Fase 1?
- [ ] Dados históricos do HubSpot: migrar ou perder?

**Esforço estimado**: ___ (P/M/G)

---

### 3. VENDAS `/vendas`
**Gate**: G3 (Pipeline e CRM unificados)
**Investigador**: _______________
**31 páginas**

**ESTADO ATUAL**:
- Pipeline: visualização de deals por estágio → FUNCIONAL
- Deals: gestão de negócios → FUNCIONAL
- Clientes: lista de clientes → FUNCIONAL
- Sales People: gestão do time de vendas → FUNCIONAL
- Commission: cálculo de comissões (BDR 1%, Closer 5%) → FUNCIONAL
- Commission Forecast: previsão → FUNCIONAL
- Goals: metas de vendas → FUNCIONAL
- Analytics: dashboards (CRM, churn cohort, form analysis, plan mix) → FUNCIONAL
- Outbound: prospecção → DEPENDENTE HubSpot (dedup + write)
- Performance: métricas do time → FUNCIONAL
- Closer: seção específica → PARCIAL

**ESTADO IDEAL**:
```
[ ] Outbound pipeline: como funciona sem HubSpot?
[ ] Qual o fluxo ideal: BDR descobre lead → SDR qualifica → Closer fecha?
[ ] Commission: o modelo atual está correto?
[ ] Analytics: quais dashboards são usados de verdade vs ignorados?
[ ] Oitiva (qualidade de ligações): entra como parte de Vendas?
```

**GAP**:
```
[ ]
```

**DECISÕES PENDENTES**:
- [ ] Outbound sem HubSpot: qual a alternativa para dedup?
- [ ] Quais analytics são realmente usados pelo time de vendas?

**Esforço estimado**: ___ (P/M/G)

---

### 4. CUSTOMER SUCCESS `/cs`
**Gate**: G2 (Acompanhar Cliente v1)
**Investigador**: _______________
**9 páginas**

**ESTADO ATUAL**:
- Dashboard: KPIs (total clientes, health score médio, at-risk, renovações) → FUNCIONAL
- Health Scores: lista + detalhe por empresa (5 componentes) → FUNCIONAL
- Churn Predictions: predições com risk score, fatores, ações → FUNCIONAL
- NPS: surveys, respostas, score → FUNCIONAL
- Renovações: pipeline de renovação com status e risco → FUNCIONAL
- Journey: timeline de 7 estágios do ciclo de vida → FUNCIONAL
- Clientes 360: visão unificada → FUNCIONAL (mas endpoint parcial)
- Playbooks: → VAZIO (0% implementado)
- Assinaturas: → A VERIFICAR

**ESTADO IDEAL**:
```
[ ] O que "Acompanhar Cliente v1" significa na prática?
[ ] Health score: os 5 componentes e pesos estão corretos? CS valida?
[ ] Quais ações o CS toma quando um cliente está em risco?
[ ] Playbooks: quais cenários são críticos para a Fase 0?
[ ] NPS: frequência de envio? Quem responde?
[ ] Renovações: como é o processo hoje? O que muda?
[ ] Customer 360: quais informações são ESSENCIAIS vs nice-to-have?
```

**GAP**:
```
[ ]
```

**DECISÕES PENDENTES**:
- [ ] Playbooks entram na Fase 0?
- [ ] Health score: CS precisa validar os pesos/métricas
- [ ] Retention offers: oferecer descontos automaticamente?

**Esforço estimado**: ___ (P/M/G)

---

### 5. SUPORTE `/suporte`
**Gate**: G2 (Acompanhar Cliente v1) + G7 (Mensageria)
**Investigador**: _______________
**15 páginas**

**ESTADO ATUAL**:
- Dashboard: hub de navegação → FUNCIONAL
- Cliente 360 (/suporte/cliente/[orgId]): visão mais completa do sistema (4 tabs) → FUNCIONAL
- Inbox: inbox unificado de conversas → FUNCIONAL (complexo)
- Health: dashboard de saúde → FUNCIONAL
- NPS: dashboard NPS → FUNCIONAL
- Churn Alerts: alertas de churn → FUNCIONAL
- Renovações: pipeline → FUNCIONAL
- Onboarding: tracker de onboarding → FUNCIONAL
- Conversas LIA: conversas com IA → FUNCIONAL
- Chatbot: configuração de chatbot → FUNCIONAL
- Knowledge Base: base de conhecimento → FUNCIONAL
- Feedback: admin de feedback → FUNCIONAL
- Performance: métricas de agentes → FUNCIONAL
- Analytics: analytics de CS → FUNCIONAL
- WhatsApp: conexões WhatsApp → FUNCIONAL

**ESTADO IDEAL**:
```
[ ] Inbox: é o canal principal de atendimento? Funciona bem?
[ ] Cliente 360: falta alguma informação crítica?
[ ] Chatbot: está operando em produção? Com quais regras?
[ ] Knowledge Base: está atualizada? Quem mantém?
[ ] Performance: métricas são usadas pelo gestor de suporte?
```

**GAP**:
```
[ ]
```

**Esforço estimado**: ___ (P/M/G)

---

### 6. WHATSAPP `/interno/whatsapp`
**Gate**: G7 (Mensageria no monorepo)
**Investigador**: _______________
**12 páginas**

**ESTADO ATUAL**:
- Hub: página principal → A VERIFICAR
- Inbox: caixa de entrada → A VERIFICAR
- Conexões: gerenciamento de números WhatsApp (Evolution API) → FUNCIONAL
- Templates: templates de mensagem → A VERIFICAR
- Analytics: métricas de WhatsApp → A VERIFICAR
- Configurações: settings → A VERIFICAR
- Envio direto via API → FUNCIONAL
- Recebimento via webhook → PARCIAL

**ESTADO IDEAL**:
```
[ ] WhatsApp é usado para quais fluxos? (vendas? suporte? onboarding?)
[ ] Templates: quais são necessários?
[ ] Automação: mensagens automáticas entram na Fase 0?
[ ] Histórico: todas as mensagens ficam registradas por cliente?
[ ] Evolution API: estável? Custo? Alternativas?
```

**GAP**:
```
[ ]
```

**DECISÕES PENDENTES**:
- [ ] Stack de WhatsApp: Evolution API é definitivo?
- [ ] Templates de mensagem: quem define o conteúdo?

**Esforço estimado**: ___ (P/M/G)

---

### 7. AUTOMAÇÕES (dentro de `/produto/workflows` e `/api/automations`)
**Gate**: G4 (Automações internas)
**Investigador**: _______________

**ESTADO ATUAL**:
- Workflow Builder: tipos e executor definidos → EXECUÇÃO É STUB
- Marketing Automations: CRUD + cron 5min + enrollment → PARCIAL
  - Ações funcionais: send_email (Resend), send_whatsapp (Evolution), set_property, create_task
  - Ações stub: send_sms, create_deal, webhook, internal_notification
- Lead Scoring: schema existe → SEM PROCESSADOR
- Sequences: schema completo → SEM MOTOR DE EXECUÇÃO
- Cron jobs: 25 definidos → MAIORIA FUNCIONAL

**ESTADO IDEAL**:
```
[ ] Quais automações rodam FORA do Backstage hoje? (Zapier, Make, manual?)
[ ] Quais são CRÍTICAS para a operação? (sem elas, o que para?)
[ ] Workflow builder visual: necessário para Fase 0?
[ ] Lead scoring: como funciona o modelo ideal?
[ ] Sequences: o time de vendas precisa criar e executar sequences?
```

**GAP**:
```
[ ]
```

**DECISÕES PENDENTES**:
- [ ] Quais ferramentas externas de automação estão em uso?
- [ ] Workflow builder: Fase 0 ou Fase 1?
- [ ] Lead scoring: quais regras?

**Esforço estimado**: ___ (P/M/G)

---

### 8. DASHBOARD EXECUTIVO `/executive` e `/dashboard`
**Gate**: G5 (Dashboard unificado)
**Investigador**: _______________

**ESTADO ATUAL**:
- Dashboard hub: links para módulos → FUNCIONAL
- Executive: → A VERIFICAR

**ESTADO IDEAL**:
```
[ ] Quais métricas Gab/VDC querem ver ao abrir o Backstage?
[ ] Frequência de atualização: real-time? Diário?
[ ] Filtros necessários: período? plano? cohort? ICP?
[ ] Quem mais usa (além de liderança)?
```

**GAP**:
```
[ ]
```

**Esforço estimado**: ___ (P/M/G)

---

## MINIAPPS DE SUPORTE (importantes mas não são gate direto)

---

### 9. OITIVA `/oitiva`
**Investigador**: _______________
**8 páginas**

**ESTADO ATUAL**: Quality assurance de ligações. Scorecards, transcrições, avaliações, rankings.

**ESTADO IDEAL**:
```
[ ] Oitiva é usado ativamente? Por quem?
[ ] Transcrições: de onde vêm? (manual upload? integração telefonia?)
[ ] Scorecards: quem define os critérios?
[ ] Avaliações: automáticas (IA) ou manuais?
```

**Esforço estimado**: ___ (P/M/G)

---

### 10. MARKETING `/marketing`
**Investigador**: _______________
**4 páginas**

**ESTADO ATUAL**: Ads analytics (Meta + Google Ads), campanhas, dashboard.

**ESTADO IDEAL**:
```
[ ] Time de marketing usa o Backstage para quê?
[ ] Pipeline de conteúdo (visão Gab): entra na Fase 0?
[ ] SEO com IA: Fase 0 ou depois?
[ ] Quais ferramentas de marketing são usadas fora do Backstage?
```

**Esforço estimado**: ___ (P/M/G)

---

### 11. PRESTADORES `/prestadores`
**Investigador**: _______________
**12 páginas**

**ESTADO ATUAL**: Central de comando, funil, ranking, saturação, KPIs, knowledge base.

**ESTADO IDEAL**:
```
[ ] Gestão de prestadores: o que precisa funcionar para a operação?
[ ] Ranking: como é calculado? É usado?
[ ] Saturação: o que significa? Quem consulta?
[ ] Knowledge base de prestadores: atualizada?
```

**Esforço estimado**: ___ (P/M/G)

---

### 12. PESSOAS/RH `/rh`, `/colaboradores`, `/ausências`, etc.
**Investigador**: _______________
**18 páginas combinadas**

**ESTADO ATUAL**: Org chart, colaboradores, ausências, payroll, performance, PDI, recrutamento, people analytics.

**ESTADO IDEAL**:
```
[ ] Quais funções de RH são usadas hoje?
[ ] Payroll: integrado com algum sistema?
[ ] Performance/PDI: ativo ou parado?
[ ] Recrutamento: pipeline ativo?
[ ] Novo framework Construtores/Consultores: precisa de módulo?
```

**Esforço estimado**: ___ (P/M/G)

---

### 13. PRODUTO `/produto`
**Investigador**: _______________
**112 páginas (maior módulo)**

**ESTADO ATUAL**: Usuários, orgs, permissions, audit logs, content, academy, AI agents, workflows, design system, coupons, announcements, performance.

**ESTADO IDEAL**:
```
[ ] Quais sub-módulos são usados ativamente?
[ ] Quais são internos (para devs/produto) vs para toda empresa?
[ ] Design system: é mantido atualizado?
[ ] AI Agents: quais estão em produção?
[ ] Workflows: descrito acima na seção de Automações
```

**Esforço estimado**: ___ (P/M/G)

---

### 14. WIKI `/wiki`
**Investigador**: _______________
**5 páginas**

**ESTADO IDEAL**:
```
[ ] Wiki é usada? Por quem?
[ ] Substitui alguma ferramenta (Notion, Confluence)?
[ ] Precisa de trabalho na Fase 0?
```

**Esforço estimado**: ___ (P/M/G)

---

### 15. CHAT `/chat`
**Investigador**: _______________
**4 páginas**

**ESTADO IDEAL**:
```
[ ] Chat interno é usado? Substitui Slack?
[ ] Precisa de trabalho na Fase 0?
```

**Esforço estimado**: ___ (P/M/G)

---

### 16. PLANEJAMENTO + OKRs `/planejamento`, `/okrs`
**Investigador**: _______________

**ESTADO IDEAL**:
```
[ ] Planejamento estratégico: como funciona hoje?
[ ] OKRs: estão em uso? Quem acompanha?
[ ] Entra na Fase 0?
```

**Esforço estimado**: ___ (P/M/G)

---

### 17. AQUISIÇÃO `/aquisicao`
**Investigador**: _______________
**~6 páginas dentro de vendas**

**ESTADO ATUAL**: Lead Hunter, metas, planejamento, BDR/SDR, KPIs diários.

**ESTADO IDEAL**:
```
[ ] Lead Hunter: funciona? Quem usa?
[ ] KPIs diários: o time de aquisição consulta?
[ ] Metas: estão configuradas corretamente?
[ ] Integração com CRM: leads viram deals automaticamente?
```

**Esforço estimado**: ___ (P/M/G)

---

## MINIAPPS UTILITÁRIAS (provavelmente fora da Fase 0)

| # | Miniapp | Rota | Investigar? |
|---|---------|------|-------------|
| 18 | Cultura | /cultura | Verificar se precisa de trabalho |
| 19 | Analytics global | /analytics | Verificar se é usado |
| 20 | Meu Espaço | /meu-espaco | Provavelmente ok |
| 21 | Assistente IA | /assistente | Verificar estado |
| 22 | Features (flags) | /features | Verificar se é usado |
| 23 | Projetos | /projetos | Verificar se é usado |

---

## TEMPLATE DE INVESTIGAÇÃO

Para cada miniapp, a pessoa responsável deve:

### Passo 1: Navegar
- Abrir a miniapp no Backstage (staging ou produção)
- Clicar em TODAS as páginas e sub-páginas
- Anotar: o que carrega dados reais vs o que mostra vazio/erro/placeholder

### Passo 2: Testar fluxos
- Tentar completar os fluxos principais (criar, editar, visualizar, filtrar)
- Anotar: o que funciona end-to-end vs o que quebra no meio

### Passo 3: Conversar com usuário
- Perguntar para quem USA a miniapp no dia a dia:
  - "O que você usa aqui?"
  - "O que falta?"
  - "O que você faz FORA do Backstage que deveria estar aqui?"
  - "O que te incomoda?"

### Passo 4: Ler o código
- Verificar quais páginas são stubs vs implementadas
- Verificar quais APIs retornam dados reais vs mock
- Verificar dependências externas (HubSpot, Google Sheets, etc.)

### Passo 5: Definir o estado ideal
- Com base nos passos 1-4, escrever:
  - "Essa miniapp está PRONTA quando: [lista de critérios]"
  - "Para chegar lá, falta: [lista de gaps]"
  - "Decisões necessárias: [lista]"
  - "Esforço estimado: P/M/G"

---

## ATRIBUIÇÃO DE INVESTIGAÇÃO (PREENCHER NO KICKOFF)

| Miniapp | Investigador | Deadline |
|---------|-------------|----------|
| Financeiro | | |
| CRM | | |
| Vendas | | |
| Customer Success | | |
| Suporte | | |
| WhatsApp | | |
| Automações | | |
| Dashboard Executivo | | |
| Oitiva | | |
| Marketing | | |
| Prestadores | | |
| Pessoas/RH | | |
| Produto | | |
| Wiki | | |
| Chat | | |
| Planejamento/OKRs | | |
| Aquisição | | |

---

## APÓS A DISCOVERY

Quando todas as miniapps estiverem investigadas:

1. **Consolidação**: Guilherme consolida todos os gaps num único documento
2. **Priorização**: VDC + Gab priorizam o que entra na Fase 0 vs backlog
3. **Estimativa**: Time estima esforço real baseado nos gaps
4. **Kickoff de execução**: Aí sim, sprint planning com tarefas concretas
5. **Plano de trabalho**: Cronograma com atribuições e deadlines

---

## CHANGELOG

| Data | Alteração | Autor |
|------|-----------|-------|
| 2026-03-09 | Documento criado com estado atual pré-preenchido (mapeamento de código) | Guilherme + Claude |