# BACKSTAGE — DISCOVERY v2: Estado Ideal de Cada Miniapp

> **Objetivo**: Definir o "estado legal" de cada miniapp/macro-area do Backstage.
> Cada pessoa do time investiga suas areas e preenche: o que esta pronto,
> o que falta, e como se parece quando esta ENTREGUE.
>
> Este documento e o INPUT para o plano de execucao.
> Sem discovery completa, nao tem kickoff.
>
> **Referencia**: PRD v6 — Backstage e o sistema operacional da Freelaw.
> **Macro-areas**: Financeiro, Comercial (Vendas+CRM+Aquisicao), Operacional (CS+Suporte+Prestadores+Oitiva),
> Produto & Tech, Pessoas, Marketing, Comunicacao (Chat+Wiki+WhatsApp).

**Status**: EM DISCOVERY
**Deadline de preenchimento**: [DEFINIR NO KICKOFF]
**Responsavel geral**: Guilherme
**CEO**: Ju
**Stakeholders-chave**: Ju (CEO), VDC (CTO), Carol (Gerente CS/Suporte), Time de Produto

---

## COMO USAR ESTE DOCUMENTO

Para cada miniapp/macro-area:

1. **Abra o Backstage em staging/producao** e navegue pela miniapp
2. **Abra o codigo** e leia o que existe vs o que e stub
3. **Preencha as 4 secoes**:
   - ESTADO ATUAL: o que funciona hoje de verdade
   - ESTADO IDEAL: como essa miniapp se parece quando esta "pronta" para a Fase 0
   - GAP: a diferenca entre atual e ideal
   - DECISOES: o que precisa ser decidido antes de construir

4. **Classifique o esforco**: P (pequeno, <3 dias), M (medio, 3-10 dias), G (grande, >10 dias)

---

## MINIAPPS CORE (Gates da Fase 0)

Estas miniapps sao diretamente ligadas aos 7 gates. Sao PRIORIDADE.

---

### 1. FINANCEIRO `/financeiro`
**Gate**: G1 (Gestao financeira centralizada) + G5 (Dashboard)
**Macro-area**: Financeiro
**Investigador**: _______________
**28 paginas | 30+ API routes**

**ESTADO ATUAL** (atualizado pelo audit de codigo — PRD v6):
- **GL (General Ledger)**: MUITO MADURO — 85 contas, 5 posting engines (billing, payroll, manual, expense, transfer), double-entry enforced, reconciliation engine (Iugu vs GL), DRE builder completo, unit economics calculator → FUNCIONAL
- **Budget Module**: orcamento por centro de custo, comparacao orcado vs realizado → FUNCIONAL
- **Business Plan**: import/export Google Sheets, cenarios, projecoes financeiras → FUNCIONAL
- **Billing ETL**: pipeline completo Iugu → GL, com fallback para raw_data (Iugu /financial_transactions morta desde dez/2025) → FUNCIONAL
- **24 Vercel crons**: billing sync, reconciliation, DRE refresh, unit economics, health check, budget alerts, etc. → FUNCIONAL
- Receita: overview, assinaturas, faturamento, billing, novo-MRR → FUNCIONAL
- Custos: despesas (Conta Azul), prestadores, nova-sede → FUNCIONAL
- Relatorios: DRE, balanco, DFC, KPIs, auditoria, relatorio diario → FUNCIONAL
- Analise: churn financeiro, inadimplencia, unit economics, analytics → FUNCIONAL
- Planejamento estrategico: cenarios, metas → FUNCIONAL
- Configuracoes: categorias, centros de custo → FUNCIONAL
- Conciliacao: Iugu vs GL com reconciliation engine → FUNCIONAL
- ERP (Freelaw One) → PLACEHOLDER ("em construcao")
- Recebimentos → STUB
- Estruturas organizacionais → STUB ("em breve")

**ESTADO IDEAL** (preencher):
```
[ ] O que precisa funcionar para considerar "entregue"?
[ ] Quem vai usar essa miniapp no dia a dia?
[ ] Quais decisoes de negocio dependem dos dados daqui?
[ ] Google Sheets pode continuar como ferramenta auxiliar ou precisa ser eliminado?
[ ] ERP (Freelaw One) entra na Fase 0 ou fica para depois?
[ ] Os 24 crons estao todos saudaveis? Algum falhando silenciosamente?
```

**GAP** (preencher apos investigar):
```
[ ] Lista de funcionalidades faltantes
[ ] Lista de bugs/problemas encontrados
[ ] Dependencias externas que precisam ser resolvidas
[ ] Crons falhando ou com dados desatualizados
```

**DECISOES PENDENTES**:
- [ ] ERP entra na Fase 0? (afeta escopo significativamente)
- [ ] Google Sheets: eliminar ou conviver?
- [ ] Recebimentos: precisa estar pronto?
- [ ] Billing ETL: raw_data fallback e estavel o suficiente?

**Esforco estimado**: ___ (P/M/G)

---

### 2. CRM `/crm` (MA-CRM: CRM como Sistema Operacional)
**Gate**: G3 (Pipeline e CRM unificados)
**Macro-area**: Comercial
**Investigador**: _______________
**12 paginas | ~100 arquivos**

> **Nota PRD v6**: CRM e agora um PACOTE (MA-CRM) consumido por Vendas, Prestadores, CS e Marketing.
> Nao e apenas uma miniapp — e infraestrutura compartilhada.

**ESTADO ATUAL** (atualizado pelo audit de codigo):
- Negocios (Deals): CRUD funcional completo, pipeline com stages visuais, leitura de v2_sales → FUNCIONAL
- Empresas (Companies): CRUD funcional, busca, filtros → FUNCIONAL
- Contatos (Contacts): CRUD funcional, associacao com empresas → FUNCIONAL
- Atividades (Activities): timeline unificada por empresa/contato/deal → FUNCIONAL
- Pipeline: visualizacao Kanban com drag-and-drop por estagio → FUNCIONAL
- HubSpot webhook handler: recebe eventos do HubSpot para sync → FUNCIONAL
- Testes: suite extensiva de testes para CRM entities → FUNCIONAL
- Sequences: UI existe, dados vem do HubSpot sync → PARCIAL (dependente HubSpot)
- Providers: pipeline de prestadores via CRM → DEPENDENTE HubSpot
- Dashboard: metricas de CRM → FUNCIONAL
- Configuracoes: settings do CRM → PARCIAL

**ESTADO IDEAL**:
```
[ ] CRM 100% independente do HubSpot?
[ ] Sequences precisam ser criadas e executadas nativamente?
[ ] Qual o fluxo completo de um lead: da descoberta ao fechamento?
[ ] Provider onboarding: como funciona sem HubSpot?
[ ] Quais campos/dados do HubSpot NAO existem no v2_sales?
[ ] Importacao de dados historicos do HubSpot: necessaria?
[ ] Como MA-CRM sera consumido por Vendas, Prestadores, CS e Marketing?
[ ] Quais entidades do CRM sao compartilhadas vs especificas de cada area?
```

**GAP**:
```
[ ]
```

**DECISOES PENDENTES**:
- [ ] Timeline de desligamento do HubSpot
- [ ] Sequences: construir motor de execucao agora ou Fase 1?
- [ ] Dados historicos do HubSpot: migrar ou perder?
- [ ] MA-CRM como pacote: quem e owner? Qual a API publica?

**Esforco estimado**: ___ (P/M/G)

---

### 3. VENDAS `/vendas`
**Gate**: G3 (Pipeline e CRM unificados)
**Macro-area**: Comercial
**Investigador**: _______________
**31 paginas | 127 arquivos**

**ESTADO ATUAL** (atualizado pelo audit de codigo):
- Pipeline: visualizacao de deals por estagio com Kanban → FUNCIONAL
- Deals: gestao completa de negocios, historico, notas → FUNCIONAL
- Leads: gestao de leads com qualificacao → FUNCIONAL
- Sales People: gestao do time (BDRs, Closers, SDRs) → FUNCIONAL
- Commission: calculo de comissoes (BDR 1%, Closer 5%) → FUNCIONAL
- Commission Forecast: previsao de comissoes futuras baseada em pipeline → FUNCIONAL
- Goals: metas de vendas por periodo/pessoa → FUNCIONAL
- Tactical Planning: planejamento tatico de vendas → FUNCIONAL
- Performance Analytics: dashboards completos (CRM, churn cohort, form analysis, plan mix) → FUNCIONAL
- Outbound Automation: prospeccao com dedup → DEPENDENTE HubSpot (dedup + write)
- ClickSign Contracts: integracao com assinatura digital de contratos → FUNCIONAL
- Closer: secao especifica para closers → PARCIAL

**ESTADO IDEAL**:
```
[ ] Outbound pipeline: como funciona sem HubSpot?
[ ] Qual o fluxo ideal: BDR descobre lead → SDR qualifica → Closer fecha?
[ ] Commission: o modelo atual esta correto?
[ ] Analytics: quais dashboards sao usados de verdade vs ignorados?
[ ] Oitiva (qualidade de ligacoes): entra como parte de Vendas?
[ ] ClickSign: contratos estao fluindo corretamente em producao?
[ ] Tactical planning: e usado pelo time de vendas?
```

**GAP**:
```
[ ]
```

**DECISOES PENDENTES**:
- [ ] Outbound sem HubSpot: qual a alternativa para dedup?
- [ ] Quais analytics sao realmente usados pelo time de vendas?
- [ ] ClickSign: contrato padrao precisa de atualizacao?

**Esforco estimado**: ___ (P/M/G)

---

### 4. CUSTOMER SUCCESS `/cs`
**Gate**: G2 (Acompanhar Cliente v1)
**Macro-area**: Operacional
**Investigador**: _______________
**Stakeholder**: Carol (Gerente CS)
**9 paginas | 36 arquivos**

**ESTADO ATUAL** (atualizado pelo audit de codigo):
- Dashboard: KPIs (total clientes, health score medio, at-risk, renovacoes) → FUNCIONAL
- Health Scores: lista + detalhe por empresa (5 componentes), **daily cron 6AM** recalcula automaticamente → FUNCIONAL
- Churn Predictions: predicoes com risk score, fatores e acoes sugeridas, **weekly ML cron** → FUNCIONAL
- NPS: campaigns com envio, respostas, score tracking → FUNCIONAL
- Renovacoes: pipeline de renovacao com status, risco, retention offers e risk stats → FUNCIONAL
- Journey: timeline de 7 estagios do ciclo de vida do cliente → FUNCIONAL
- Clientes 360: visao unificada com timeline completa de interacoes → FUNCIONAL
- Playbooks: templates de acoes por cenario → FUNCIONAL (CRUD implementado, nao VAZIO)
- Retention Offers: ofertas de retencao por nivel de risco → FUNCIONAL

**ESTADO IDEAL**:
```
[ ] O que "Acompanhar Cliente v1" significa na pratica? (validar com Carol)
[ ] Health score: os 5 componentes e pesos estao corretos? Carol valida?
[ ] Quais acoes o CS toma quando um cliente esta em risco?
[ ] Playbooks: quais cenarios sao criticos para a Fase 0?
[ ] NPS: frequencia de envio? Quem responde?
[ ] Renovacoes: como e o processo hoje? O que muda?
[ ] Customer 360: quais informacoes sao ESSENCIAIS vs nice-to-have?
[ ] Churn prediction ML: modelo esta preciso? Validacao com dados reais?
```

**GAP**:
```
[ ]
```

**DECISOES PENDENTES**:
- [ ] Health score: Carol precisa validar os pesos/metricas
- [ ] Retention offers: oferecer descontos automaticamente?
- [ ] Churn ML: modelo precisa ser retreinado com dados recentes?

**Esforco estimado**: ___ (P/M/G)

---

### 5. SUPORTE `/suporte`
**Gate**: G2 (Acompanhar Cliente v1) + G7 (Mensageria)
**Macro-area**: Operacional
**Investigador**: _______________
**Stakeholder**: Carol (Gerente CS/Suporte)
**13 paginas**

**ESTADO ATUAL**:
- Dashboard: hub de navegacao → FUNCIONAL
- Cliente 360 (/suporte/cliente/[orgId]): visao mais completa do sistema (4 tabs) → FUNCIONAL
- Inbox: inbox unificado de conversas → FUNCIONAL (complexo)
- Health: dashboard de saude → FUNCIONAL
- NPS: dashboard NPS → FUNCIONAL
- Churn Alerts: alertas de churn → FUNCIONAL
- Renovacoes: pipeline → FUNCIONAL
- Onboarding: tracker de onboarding → FUNCIONAL
- Conversas LIA: conversas com IA → FUNCIONAL
- Chatbot: configuracao de chatbot → FUNCIONAL
- Knowledge Base: base de conhecimento → FUNCIONAL
- Feedback: admin de feedback → FUNCIONAL
- Performance: metricas de agentes → FUNCIONAL

**ESTADO IDEAL**:
```
[ ] Inbox: e o canal principal de atendimento? Funciona bem?
[ ] Cliente 360: falta alguma informacao critica? (validar com Carol)
[ ] Chatbot: esta operando em producao? Com quais regras?
[ ] Knowledge Base: esta atualizada? Quem mantem?
[ ] Performance: metricas sao usadas pelo gestor de suporte?
```

**GAP**:
```
[ ]
```

**Esforco estimado**: ___ (P/M/G)

---

### 6. WHATSAPP `/interno/whatsapp`
**Gate**: G7 (Mensageria no monorepo)
**Macro-area**: Comunicacao
**Investigador**: _______________
**7 paginas**

**ESTADO ATUAL** (atualizado pelo audit de codigo):
- Hub: pagina principal → FUNCIONAL
- Inbox: caixa de entrada de mensagens → FUNCIONAL
- Conexoes: gerenciamento de numeros WhatsApp (Evolution API) → FUNCIONAL
- Cloud API Webhook: integracao direta com WhatsApp Cloud API → FUNCIONAL
- Evolution API Sync: sincronizacao bidirecional via Evolution API → FUNCIONAL
- Team Conversations: conversas atribuidas por equipe → FUNCIONAL
- Envio direto via API → FUNCIONAL

**ESTADO IDEAL**:
```
[ ] WhatsApp e usado para quais fluxos? (vendas? suporte? onboarding?)
[ ] Templates: quais sao necessarios?
[ ] Automacao: mensagens automaticas entram na Fase 0?
[ ] Historico: todas as mensagens ficam registradas por cliente?
[ ] Evolution API vs Cloud API: qual e o padrao? Manter os dois?
```

**GAP**:
```
[ ]
```

**DECISOES PENDENTES**:
- [ ] Stack de WhatsApp: Evolution API + Cloud API — simplificar para um so?
- [ ] Templates de mensagem: quem define o conteudo?

**Esforco estimado**: ___ (P/M/G)

---

### 7. AUTOMACOES (dentro de `/produto/workflows` e `/api/automations`)
**Gate**: G4 (Automacoes internas)
**Macro-area**: Produto & Tech
**Investigador**: _______________

**ESTADO ATUAL**:
- Workflow Builder: tipos e executor definidos → EXECUCAO E STUB
- Marketing Automations: CRUD + cron 5min + enrollment → PARCIAL
  - Acoes funcionais: send_email (Resend), send_whatsapp (Evolution), set_property, create_task
  - Acoes stub: send_sms, create_deal, webhook, internal_notification
- Lead Scoring: schema existe → SEM PROCESSADOR
- Sequences: schema completo → SEM MOTOR DE EXECUCAO
- Cron jobs: 25 definidos → MAIORIA FUNCIONAL

**ESTADO IDEAL**:
```
[ ] Quais automacoes rodam FORA do Backstage hoje? (Zapier, Make, manual?)
[ ] Quais sao CRITICAS para a operacao? (sem elas, o que para?)
[ ] Workflow builder visual: necessario para Fase 0?
[ ] Lead scoring: como funciona o modelo ideal?
[ ] Sequences: o time de vendas precisa criar e executar sequences?
```

**GAP**:
```
[ ]
```

**DECISOES PENDENTES**:
- [ ] Quais ferramentas externas de automacao estao em uso?
- [ ] Workflow builder: Fase 0 ou Fase 1?
- [ ] Lead scoring: quais regras?

**Esforco estimado**: ___ (P/M/G)

---

### 8. DASHBOARD EXECUTIVO `/executive` e `/dashboard`
**Gate**: G5 (Dashboard unificado)
**Macro-area**: Transversal
**Investigador**: _______________

**ESTADO ATUAL**:
- Dashboard hub: links para modulos → FUNCIONAL
- Executive: → A VERIFICAR

**ESTADO IDEAL**:
```
[ ] Quais metricas Ju/VDC querem ver ao abrir o Backstage?
[ ] Frequencia de atualizacao: real-time? Diario?
[ ] Filtros necessarios: periodo? plano? cohort? ICP?
[ ] Quem mais usa (alem de lideranca)?
```

**GAP**:
```
[ ]
```

**Esforco estimado**: ___ (P/M/G)

---

## MINIAPPS DE SUPORTE (importantes mas nao sao gate direto)

---

### 9. OITIVA `/oitiva`
**Macro-area**: Operacional
**Investigador**: _______________
**8 paginas**

**ESTADO ATUAL** (atualizado pelo audit de codigo):
- Quality assurance de ligacoes com scorecards, transcricoes, avaliacoes, rankings → FUNCIONAL
- **4 crons ativos**:
  - `sync-calls`: a cada 10min, sincroniza ligacoes da telefonia → FUNCIONAL
  - `sync-evaluations`: a cada 15min, sincroniza avaliacoes → FUNCIONAL
  - `process-transcriptions`: a cada 5min, processa transcricoes pendentes → FUNCIONAL
  - `cleanup`: semanal, limpa dados antigos → FUNCIONAL
- AI Evaluation Agent: avaliacao automatica de ligacoes por IA → FUNCIONAL
- Scorecards CRUD: criacao/edicao de criterios de avaliacao, com testes → FUNCIONAL

**ESTADO IDEAL**:
```
[ ] Oitiva e usado ativamente? Por quem?
[ ] Transcricoes: sync a cada 10min e suficiente? Atraso aceitavel?
[ ] Scorecards: quem define os criterios? Estao atualizados?
[ ] AI Evaluation: a qualidade das avaliacoes automaticas e boa?
[ ] Rankings: sao usados pelo gestor de vendas/CS?
```

**Esforco estimado**: ___ (P/M/G)

---

### 10. MARKETING `/marketing`
**Macro-area**: Marketing
**Investigador**: _______________
**4 paginas**

**ESTADO ATUAL** (atualizado pelo audit de codigo):
- Ads analytics: dashboard com metricas → FUNCIONAL
- Google Ads API: integracao real com Google Ads para dados de campanhas → FUNCIONAL
- Meta Ads: integracao com Facebook/Instagram Ads → PARCIAL
- Campanhas: gestao de campanhas → FUNCIONAL

**ESTADO IDEAL**:
```
[ ] Time de marketing usa o Backstage para que?
[ ] Pipeline de conteudo (visao Ju): entra na Fase 0?
[ ] SEO com IA: Fase 0 ou depois?
[ ] Quais ferramentas de marketing sao usadas fora do Backstage?
[ ] Google Ads integration: dados estao corretos e atualizados?
```

**Esforco estimado**: ___ (P/M/G)

---

### 11. PRESTADORES `/prestadores`
**Macro-area**: Operacional
**Investigador**: _______________
**14 paginas | 77 arquivos**

**ESTADO ATUAL** (atualizado pelo audit de codigo):
- Provider 360: visao completa do prestador com historico, metricas, documentos → FUNCIONAL
- KPIs: indicadores por prestador (qualidade, tempo, satisfacao) → FUNCIONAL
- Ranking: ranking de prestadores por performance → FUNCIONAL
- Saturacao: controle de capacidade/saturacao por prestador → FUNCIONAL
- Capacity: gestao de capacidade disponivel → FUNCIONAL
- Governance: regras de governanca e compliance → FUNCIONAL
- Qualification: qualificacao de novos prestadores → FUNCIONAL
- Work Orders: ordens de servico → FUNCIONAL
- Incident Tracking: registro e acompanhamento de incidentes → FUNCIONAL
- Carteirizacao: distribuicao de clientes por prestador → FUNCIONAL
- Maria AI: agente de IA para insights sobre prestadores → FUNCIONAL
- Knowledge Base: base de conhecimento de prestadores → FUNCIONAL
- Funil: funil de onboarding de prestadores → FUNCIONAL

**ESTADO IDEAL**:
```
[ ] Gestao de prestadores: o que precisa funcionar para a operacao?
[ ] Ranking: como e calculado? E usado na pratica?
[ ] Saturacao/Capacity: quem consulta? Alerta automatico?
[ ] Maria AI: insights sao uteis? Quem usa?
[ ] Carteirizacao: regras automaticas ou manuais?
[ ] Consome MA-CRM para dados de provider?
```

**Esforco estimado**: ___ (P/M/G)

---

### 12. PESSOAS/RH `/rh`, `/colaboradores`, `/ausencias`, etc.
**Macro-area**: Pessoas
**Investigador**: _______________
**3 paginas backstage | 56 arquivos | 2 pacotes (@freelaw/hr-core, @freelaw/hr-sdk)**

> **Nota**: gestao.freelaw.ai E o backstage (mesmo Vercel deploy).
> RH tem 3 paginas dedicadas no backstage mas 56 arquivos no total, incluindo
> dois pacotes compartilhados: @freelaw/hr-core e @freelaw/hr-sdk.

**ESTADO ATUAL** (atualizado pelo audit de codigo):
- Org Chart: organograma com API CRUD completa → FUNCIONAL
- Payroll: folha de pagamento com hooks para dados reais → FUNCIONAL
- People Analytics: dashboards de metricas de pessoas → FUNCIONAL
- Colaboradores: gestao de colaboradores → FUNCIONAL
- Ausencias: controle de ausencias → FUNCIONAL
- @freelaw/hr-core: pacote com logica de negocio de RH → FUNCIONAL
- @freelaw/hr-sdk: SDK para consumo por outras areas → FUNCIONAL

**ESTADO IDEAL**:
```
[ ] Quais funcoes de RH sao usadas hoje?
[ ] Payroll: integrado com algum sistema externo?
[ ] Performance/PDI: ativo ou parado?
[ ] Recrutamento: pipeline ativo?
[ ] Novo framework Construtores/Consultores: precisa de modulo?
[ ] hr-core e hr-sdk: estao sendo consumidos por outras areas?
```

**Esforco estimado**: ___ (P/M/G)

---

### 13. PRODUTO `/produto`
**Macro-area**: Produto & Tech
**Investigador**: _______________
**112 paginas (maior modulo)**

**ESTADO ATUAL** (atualizado pelo audit de codigo):
- Usuarios: gestao completa → FUNCIONAL
- Organizacoes: CRUD completo → FUNCIONAL
- Permissions: sistema de permissoes → FUNCIONAL
- Audit Logs: logs de auditoria → FUNCIONAL
- Content/Academy: gestao de conteudo → FUNCIONAL
- AI Agents: agentes de IA em producao → FUNCIONAL
- Feature Flags: feature toggles ativos → FUNCIONAL
- Coupons: gestao de cupons → FUNCIONAL
- Announcements: comunicados → FUNCIONAL
- Performance: metricas de performance → FUNCIONAL
- Wiki: integrada (ver secao Wiki) → FUNCIONAL
- Chat: integrado (ver secao Chat) → FUNCIONAL

**ESTADO IDEAL**:
```
[ ] Quais sub-modulos sao usados ativamente?
[ ] Quais sao internos (para devs/produto) vs para toda empresa?
[ ] AI Agents: quais estao em producao? Metricas de uso?
[ ] Feature flags: quantas ativas? Processo de limpeza?
[ ] Workflows: descrito acima na secao de Automacoes
```

**Esforco estimado**: ___ (P/M/G)

---

### 14. DESIGN SYSTEM `/produto/design-system`
**Macro-area**: Produto & Tech
**Investigador**: _______________
**80+ componentes | 70+ paginas de documentacao**

> **Nota PRD v6**: Design System e maduro o suficiente para ter sua propria secao de discovery.
> E infraestrutura critica que alimenta todas as miniapps.

**ESTADO ATUAL** (atualizado pelo audit de codigo):
- 80+ componentes implementados (buttons, inputs, modals, tables, charts, etc.) → FUNCIONAL
- 70+ paginas de documentacao com exemplos e props → FUNCIONAL
- Storybook/docs pages para cada componente → FUNCIONAL
- Tokens de design (cores, espacamento, tipografia) → FUNCIONAL
- Tema dark/light → FUNCIONAL
- Acessibilidade basica → PARCIAL
- Designer's Bible (guia completo de design para a empresa) → NAO EXISTE

**ESTADO IDEAL**:
```
[ ] Todos os componentes estao sendo usados consistentemente nas miniapps?
[ ] Designer's Bible: quem vai criar? E prioridade Fase 0?
[ ] Tokens de design: estao sincronizados com Figma?
[ ] Acessibilidade: qual o nivel minimo aceitavel (WCAG AA)?
[ ] Componentes faltantes que bloqueiam alguma miniapp?
```

**GAP**:
```
[ ]
```

**DECISOES PENDENTES**:
- [ ] Designer's Bible: Fase 0 ou Fase 1?
- [ ] Figma sync: automatizar ou manual?

**Esforco estimado**: ___ (P/M/G)

---

### 15. WIKI `/wiki`
**Macro-area**: Comunicacao
**Investigador**: _______________
**5 paginas**

**ESTADO ATUAL** (atualizado pelo audit de codigo):
- Spaces: organizacao por espacos/areas → FUNCIONAL
- Documents: criacao e edicao de documentos → FUNCIONAL
- Search: busca full-text nos documentos → FUNCIONAL
- Templates: templates reutilizaveis → FUNCIONAL
- Notion Import: importacao de conteudo do Notion → FUNCIONAL

**ESTADO IDEAL**:
```
[ ] Wiki e usada? Por quem?
[ ] Substitui alguma ferramenta (Notion, Confluence)?
[ ] Notion import: migrar todo o conteudo ou manter dual?
[ ] Precisa de trabalho na Fase 0?
```

**Esforco estimado**: ___ (P/M/G)

---

### 16. CHAT `/chat`
**Macro-area**: Comunicacao
**Investigador**: _______________
**4 paginas**

**ESTADO ATUAL** (atualizado pelo audit de codigo):
- Channels: canais de comunicacao por equipe/tema → FUNCIONAL
- DMs: mensagens diretas entre usuarios → FUNCIONAL
- Threads: conversas em thread dentro de canais → FUNCIONAL
- Reactions: reacoes a mensagens → FUNCIONAL
- File Upload: envio de arquivos → FUNCIONAL
- Typing Indicators: indicadores de digitacao em tempo real → FUNCIONAL
- Presence: status de presenca (online/offline/away) → FUNCIONAL
- Slack Import: importacao de historico do Slack → FUNCIONAL

**ESTADO IDEAL**:
```
[ ] Chat interno e usado? Substitui Slack?
[ ] Slack import: migrar todo o historico ou comecar do zero?
[ ] Notificacoes: push notifications funcionam?
[ ] Precisa de trabalho na Fase 0?
```

**Esforco estimado**: ___ (P/M/G)

---

### 17. MA-13: LIA/IA (Producao de IA)
**Macro-area**: Produto & Tech
**Investigador**: _______________

> **Nota PRD v6**: LIA e a camada de IA da Freelaw em producao.
> Inclui chatbot, agentes, classificacao, scoring, avaliacoes automaticas.

**ESTADO ATUAL** (consolidado do audit de codigo):
- LIA Chatbot: atendimento ao cliente via chat com IA → FUNCIONAL (dentro de Suporte)
- AI Evaluation Agent (Oitiva): avaliacao automatica de ligacoes → FUNCIONAL
- Maria AI (Prestadores): insights e recomendacoes sobre prestadores → FUNCIONAL
- AI Classification: classificacao automatica de entidades → FUNCIONAL
- Churn ML (CS): modelo de predicao de churn → FUNCIONAL

**ESTADO IDEAL**:
```
[ ] Quais agentes IA estao em producao e quais sao experimentais?
[ ] Metricas de qualidade: como medir se a IA esta boa?
[ ] Custos de API: quanto estamos gastando com OpenAI/Anthropic?
[ ] Guardrails: quais limites existem para evitar respostas ruins?
[ ] Consolidar todos os agentes numa camada unica ou manter distribuidos?
```

**GAP**:
```
[ ]
```

**DECISOES PENDENTES**:
- [ ] LIA como produto: Fase 0 ou Fase 1?
- [ ] Consolidacao de agentes IA: centralizar ou manter por area?

**Esforco estimado**: ___ (P/M/G)

---

### 18. PLANEJAMENTO + OKRs `/planejamento`, `/okrs`
**Macro-area**: Transversal
**Investigador**: _______________

**ESTADO IDEAL**:
```
[ ] Planejamento estrategico: como funciona hoje?
[ ] OKRs: estao em uso? Quem acompanha?
[ ] Entra na Fase 0?
```

**Esforco estimado**: ___ (P/M/G)

---

### 19. AQUISICAO `/aquisicao`
**Macro-area**: Comercial
**Investigador**: _______________
**~6 paginas dentro de vendas**

**ESTADO ATUAL**: Lead Hunter, metas, planejamento, BDR/SDR, KPIs diarios.

**ESTADO IDEAL**:
```
[ ] Lead Hunter: funciona? Quem usa?
[ ] KPIs diarios: o time de aquisicao consulta?
[ ] Metas: estao configuradas corretamente?
[ ] Integracao com CRM: leads viram deals automaticamente?
```

**Esforco estimado**: ___ (P/M/G)

---

## MINIAPPS UTILITARIAS (provavelmente fora da Fase 0)

| # | Miniapp | Rota | Investigar? |
|---|---------|------|-------------|
| 20 | Cultura | /cultura | Verificar se precisa de trabalho |
| 21 | Analytics global | /analytics | Verificar se e usado |
| 22 | Meu Espaco | /meu-espaco | Provavelmente ok |
| 23 | Assistente IA | /assistente | Verificar estado |
| 24 | Features (flags) | /features | Verificar se e usado |
| 25 | Projetos | /projetos | Verificar se e usado |

---

## TEMPLATE DE INVESTIGACAO

Para cada miniapp, a pessoa responsavel deve:

### Passo 1: Navegar
- Abrir a miniapp no Backstage (staging ou producao)
- Clicar em TODAS as paginas e sub-paginas
- Anotar: o que carrega dados reais vs o que mostra vazio/erro/placeholder

### Passo 2: Testar fluxos
- Tentar completar os fluxos principais (criar, editar, visualizar, filtrar)
- Anotar: o que funciona end-to-end vs o que quebra no meio

### Passo 3: Testar com dados reais em producao
- **Nao basta "carregar a pagina"** — verificar se os dados sao reais e coerentes
- Validar fluxos end-to-end com dados de producao (ex: criar deal, ver no pipeline, fechar, ver em relatorio)
- Testar cross-area: dado criado numa miniapp aparece corretamente em outra?
  - Ex: deal criado em Vendas aparece no CRM? Health score de CS reflete dados de Suporte?
- Anotar: dados inconsistentes, desatualizados, ou que nao batem entre areas

### Passo 4: Conversar com usuario
- Perguntar para quem USA a miniapp no dia a dia:
  - "O que voce usa aqui?"
  - "O que falta?"
  - "O que voce faz FORA do Backstage que deveria estar aqui?"
  - "O que te incomoda?"

### Passo 5: Ler o codigo
- Verificar quais paginas sao stubs vs implementadas
- Verificar quais APIs retornam dados reais vs mock
- Verificar dependencias externas (HubSpot, Google Sheets, etc.)

### Passo 6: Verificar fluxo de dados cross-area
- Mapear quais dados dessa miniapp sao consumidos por OUTRAS miniapps
- Verificar se os dados fluem corretamente entre areas (ex: Financeiro → Dashboard, CRM → Vendas)
- Anotar: integrações quebradas ou dados que nao propagam

### Passo 7: Definir o estado ideal
- Com base nos passos 1-6, escrever:
  - "Essa miniapp esta PRONTA quando: [lista de criterios]"
  - "Para chegar la, falta: [lista de gaps]"
  - "Decisoes necessarias: [lista]"
  - "Esforco estimado: P/M/G"

---

## ATRIBUICAO DE INVESTIGACAO (PREENCHER NO KICKOFF)

| Macro-area | Miniapp | Investigador | Deadline |
|------------|---------|-------------|----------|
| **Financeiro** | Financeiro | | |
| **Comercial** | CRM (MA-CRM) | | |
| **Comercial** | Vendas | | |
| **Comercial** | Aquisicao | | |
| **Operacional** | Customer Success | Carol + | |
| **Operacional** | Suporte | Carol + | |
| **Operacional** | Oitiva | | |
| **Operacional** | Prestadores | | |
| **Comunicacao** | WhatsApp | | |
| **Comunicacao** | Chat | | |
| **Comunicacao** | Wiki | | |
| **Produto & Tech** | Automacoes | | |
| **Produto & Tech** | Produto | | |
| **Produto & Tech** | Design System | | |
| **Produto & Tech** | LIA/IA | | |
| **Pessoas** | Pessoas/RH | | |
| **Marketing** | Marketing | | |
| **Transversal** | Dashboard Executivo | | |
| **Transversal** | Planejamento/OKRs | | |

---

## APOS A DISCOVERY

Quando todas as miniapps estiverem investigadas:

1. **Consolidacao**: Guilherme consolida todos os gaps num unico documento
2. **Priorizacao**: VDC + Ju priorizam o que entra na Fase 0 vs backlog
3. **Estimativa**: Time estima esforco real baseado nos gaps
4. **Kickoff de execucao**: Ai sim, sprint planning com tarefas concretas
5. **Plano de trabalho**: Cronograma com atribuicoes e deadlines

---

## CHANGELOG

| Data | Alteracao | Autor |
|------|-----------|-------|
| 2026-03-09 | v2: Atualizado com findings do PRD v6 e audit de codigo. CEO Ju. Macro-areas. CRM como MA-CRM. Novas secoes: Design System, LIA/IA. Estados atuais detalhados com dados do audit (GL 85 contas, 5 posting engines, 24 crons, Oitiva 4 crons, etc). Template de investigacao expandido (teste producao + cross-area). Tabela de atribuicao com macro-areas e Carol como stakeholder CS/Suporte. | Guilherme + Claude |
| 2026-03-09 | v1: Documento criado com estado atual pre-preenchido (mapeamento de codigo) | Guilherme + Claude |