# BACKSTAGE FREELAW — PLANO DE TRABALHO

> Documento operacional do time de Backstage.
> Cada pessoa sabe exatamente o que fazer, quando, e o que bloqueia.
> Atualizado a cada sprint planning (segunda-feira).

**Versão**: 1.0 | **Data**: 2026-03-09
**Líder**: Guilherme | **Owner**: VDC
**Time**: Alex, Biel, Clarinha, Davi
**Prazo hard**: 30 de Abril de 2026

---

## PARTE 1: DIAGNÓSTICO REAL DO CÓDIGO

### Estado de cada Gate (mapeado contra o código em 09/mar)

| Gate | Descrição | % Pronto | Esforço restante | Risco |
|------|-----------|----------|------------------|-------|
| **G1** | Gestão financeira centralizada | **95%** | Baixo | Baixo |
| **G2** | Acompanhar Cliente v1 | **73%** | Médio | Médio |
| **G3** | Pipeline/CRM unificado (matar HubSpot) | **70%** | **Alto** | **Alto** |
| **G4** | Automações internas | **60%** | Alto | Médio |
| **G5** | Dashboard unificado de métricas | **90%** | Baixo | Baixo |
| **G6** | Integrações estabilizadas | **85%** | Baixo-Médio | Baixo |
| **G7** | Mensageria no monorepo | **85%** | Médio | Médio |

### O que descobrimos

**G1 e G5 estão quase prontos.** Módulo financeiro é robusto — receita, custos, DRE, KPIs, business plan, reconciliação, unit economics. Única dependência externa: Google Sheets para Business Plan.

**G2 está melhor do que parecia.** Health score com 5 componentes implementado, Customer 360 API com 9 fontes de dados, NPS completo, churn predictions funcionando. Gaps: playbooks vazio, retention offers engine construída mas não conectada, sem log formal de interações.

**G3 é o gate mais crítico.** CRM nativo funciona (contacts, companies, deals, pipelines, activities). MAS ainda depende do HubSpot para: sync de dados entrando, outbound (dedup + write), execução de sequences, onboarding de prestadores. Schema de sequences está completo mas **não tem motor de execução**.

**G4 tem framework mas falta execução.** Workflow builder tem tipos e executor, mas execução é STUB (simula trabalho). Automações de marketing tem CRUD e cron rodando a cada 5min, mas ações como SMS, webhooks, deals estão vazias. Lead scoring tem schema mas sem processador.

**G6 está estável.** Iugu e Conta Azul sincronizando. Stripe só webhook. Banco Inter pronto mas inativo. Omie tem schema mas sem sync.

**G7 opera parcialmente.** WhatsApp via Evolution API funciona. Email via Resend funciona. Falta: SMS, templates avançados, histórico consolidado por cliente.

---

## PARTE 2: METODOLOGIA

### 2.1 Princípios inegociáveis

1. **Uma pessoa = uma missão por sprint.** Sem context switching. Se você está no G2, você só faz G2.
2. **Vertical first.** Termina um fluxo end-to-end antes de começar outro.
3. **Demo ou não existiu.** Toda sexta, cada pessoa demonstra o que entregou funcionando.
4. **Bloqueio = escala imediata.** Se está bloqueado, fala no daily. Se não resolve em 24h, escala para Guilherme. Se não resolve em 48h, escala para VDC.
5. **PRD é bounding.** Se não está neste documento, não entra no sprint. Mudanças passam por Guilherme.
6. **Zero construção no legado.** Toda energia no monorepo.

### 2.2 Rituais

| Ritual | Quando | Duração | Quem | Output |
|--------|--------|---------|------|--------|
| **Daily standup** | Seg-Sex, 9h | 15min | Time completo | Bloqueio + foco do dia |
| **Sprint planning** | Segunda, 9h30 | 45min | Time completo | Tarefas da semana atribuídas |
| **Sync técnico** | Quarta, 14h | 30min | Guilherme + devs | Decisões técnicas, code review |
| **Comitê Produto & Backstage** | Quinta, horário empresa | 60min | + Time Migração | Dependências cruzadas |
| **Demo & Report** | Sexta, horário empresa | 30min | Empresa toda | Demonstração do entregue |
| **Sprint retro + PRD review** | Sexta, 16h | 30min | Time completo | Ajustes no plano |

### 2.3 Definição de "Pronto" (DoD)

Uma tarefa só está PRONTA quando:
- [ ] Código commitado no monorepo (branch + PR)
- [ ] Typecheck passa (`bun run typecheck`)
- [ ] Funcionalidade testável em staging
- [ ] Demonstrável (screenshot ou vídeo se UI, curl se API)
- [ ] Sem @ts-nocheck ou `as any` novo
- [ ] Auth implementada na rota (se API pública)

### 2.4 Board de tarefas

Usaremos um board Kanban com 5 colunas:
```
BACKLOG → SPRINT → EM PROGRESSO → REVIEW → DONE
```

Cada card tem:
- Gate (G1-G7)
- Pessoa responsável
- Critério de aceite (1-3 bullets)
- Dependências (se houver)
- Sprint alvo

---

## PARTE 3: ATRIBUIÇÃO POR PESSOA

### Visão geral

```
GUILHERME (Líder)
├── Arquitetura de dados, decisões técnicas, PRD
├── Code review final de todas as PRs
├── Desbloqueio de dependências com outros times
├── Contribuição direta em tarefas críticas (G3 HubSpot migration)
└── Comunicação com VDC e Gab

ALEX (Financeiro + Integrações)
├── Sprint 1-2: G6 (integrações) + G1 (financeiro polish)
└── Sprint 3-4: G5 (dashboard executivo) + G1 (eliminar Google Sheets)

BIEL (CRM + Pipeline + Cadências)
├── Sprint 1-2: G3 (pipeline nativo, matar HubSpot)
└── Sprint 3-4: G3 (sequences engine) + G4 (automações)

CLARINHA (CS + Cliente 360)
├── Sprint 1-2: G2 (cliente 360, health score, interações)
└── Sprint 3-4: G2 (playbooks, retention offers) + G5 (métricas CS)

DAVI (Mensageria + Automações)
├── Sprint 1-2: G7 (mensageria consolidada) + G4 (action executor)
└── Sprint 3-4: G4 (workflow execution) + G7 (templates)
```

---

## PARTE 4: PLANO SPRINT A SPRINT

---

### SPRINT 1 — FUNDAÇÃO E ESTABILIZAÇÃO
**Período**: 10-21 mar (2 semanas)
**Tema**: Auditar, estabilizar integrações, iniciar gates críticos

---

#### GUILHERME — Sprint 1

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| G-1.1 | Validar PRD com VDC e Gab | Todos | PRD aprovado e comunicado ao time | — |
| G-1.2 | Configurar board de tarefas (Linear/GitHub Projects) | Todos | Board operando com todos os cards do Sprint 1 | — |
| G-1.3 | Mapear TODAS as ferramentas externas em uso | G4,G6 | Lista completa: ferramenta → função → substituto → status | Time |
| G-1.4 | Alinhar dependências com time Migração (Juju) | Todos | Documento de dependências cruzadas assinado por ambos | Juju |
| G-1.5 | Definir com GTM (Didico): ICP e planos/pricing | G2,G3 | ICP documentado, planos nomeados com features/preços | Didico |
| G-1.6 | Decisão: timeline de desligamento do HubSpot | G3 | Data-alvo definida, plano de migração aprovado | VDC,Gab |
| G-1.7 | Decisão: 167 API routes sem auth — priorizar quais? | G6 | Lista de routes críticas a proteger + cronograma | Time |

**Entregável da demo**: PRD validado + board funcionando + mapa de dependências

---

#### ALEX — Sprint 1

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| A-1.1 | Auditoria de TODAS as integrações | G6 | Para cada uma das 14: status (ok/falha/inativo), última sync, erros recentes, SLA |  — |
| A-1.2 | Implementar monitoramento de integrações | G6 | Alertas Sentry/Slack para falhas de sync. Dashboard de status de cada integração | A-1.1 |
| A-1.3 | Estabilizar sync Iugu (se issues encontrados) | G6 | Zero falhas de sync em 7 dias consecutivos | A-1.1 |
| A-1.4 | Estabilizar sync Conta Azul (se issues encontrados) | G6 | Zero falhas de sync em 7 dias consecutivos | A-1.1 |
| A-1.5 | Documentar cada integração | G6 | Para cada: o que sincroniza, frequência, credenciais necessárias, fallback | A-1.1 |
| A-1.6 | Auditar módulo financeiro — listar gaps vs G1 | G1 | Lista precisa do que falta para G1 (checklist binário) | — |

**Entregável da demo**: Dashboard de monitoramento de integrações + relatório de auditoria

---

#### BIEL — Sprint 1

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| B-1.1 | Mapear toda dependência HubSpot no código | G3 | Lista de TODOS os arquivos que importam/chamam HubSpot API. Classificar: crítico/médio/baixo | — |
| B-1.2 | Mapear dados que só existem no HubSpot (não no v2_sales) | G3 | Lista de campos/entidades que perdem se desligar HubSpot | B-1.1 |
| B-1.3 | Substituir dedupHubspot() por dedup local | G3 | Outbound pipeline funciona sem HubSpot token. Dedup consulta v2_sales.companies por domain | B-1.1 |
| B-1.4 | Substituir hubspotWrite() por escrita local | G3 | Novos leads/companies salvos em v2_sales, não em HubSpot | B-1.3 |
| B-1.5 | Testar outbound pipeline end-to-end sem HubSpot | G3 | Pipeline roda, descobre leads, dedup local, salva no v2_sales | B-1.4 |

**Entregável da demo**: Outbound pipeline rodando 100% sem HubSpot

---

#### CLARINHA — Sprint 1

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| C-1.1 | Auditar Customer 360 API (/api/customer-360) | G2 | Documento: quais das 9 fontes retornam dados reais vs mock. Testar com 5 clientes reais | — |
| C-1.2 | Auditar health score (/api/customer-success/health) | G2 | Verificar: scores estão sendo calculados? Cron de health roda? Dados reais para quantos clientes? | — |
| C-1.3 | Garantir /suporte/cliente/[orgId] funciona end-to-end | G2 | Página carrega para qualquer cliente ativo. Todas as 4 tabs (timeline, tickets, conversas, NPS) mostram dados | C-1.1 |
| C-1.4 | Listar gaps do G2: o que falta para "Acompanhar Cliente v1" | G2 | Checklist binário: o que funciona vs o que falta | C-1.1,C-1.2 |
| C-1.5 | Definir com CS: quais métricas compõem o health score | G2 | Documento aprovado pelo CS com pesos e thresholds | Didico/CS |

**Entregável da demo**: Customer 360 funcionando para 5 clientes reais + gap analysis

---

#### DAVI — Sprint 1

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| D-1.1 | Auditar mensageria: WhatsApp (Evolution), Email (Resend) | G7 | Status real: funciona? Envia? Recebe? Webhook de resposta funciona? | — |
| D-1.2 | Mapear todas as mensagens enviadas FORA do monorepo | G7 | Lista: quais emails/WhatsApp saem por ferramentas externas? Quais templates? | — |
| D-1.3 | Consolidar histórico de mensagens por cliente/lead | G7 | Criar ou validar tabela unified_interactions. Query: dado um cliente, listar TODAS as interações (WhatsApp + email + suporte) | D-1.1 |
| D-1.4 | Auditar automações: quais ações do action-executor funcionam? | G4 | Testar cada ação: send_email, send_whatsapp, set_contact_property, create_task. Documentar quais funcionam e quais são stub | — |
| D-1.5 | Auditar cron jobs: quais rodam, quais falham? | G4 | Lista dos 25 crons: último run, status, erros recentes | — |

**Entregável da demo**: Mapa completo de mensageria + automações + status de cada cron

---

### SPRINT 2 — FECHAR G1, G5, G6 + AVANÇAR G3
**Período**: 24 mar - 4 abr (2 semanas)
**Tema**: Fechar gates mais próximos, avançar migração HubSpot

---

#### GUILHERME — Sprint 2

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| G-2.1 | Revisar PRs e decisões técnicas do Sprint 1 | Todos | Todas as PRs do Sprint 1 merged ou com feedback claro | — |
| G-2.2 | Validar G6 para fechamento | G6 | Checklist do gate verificado item a item com Alex | A-1.* |
| G-2.3 | Desenhar schema de unified_interactions (se necessário) | G2,G7 | Schema aprovado, migration pronta | D-1.3 |
| G-2.4 | Alinhar com Migração: dados do Studio sincronizando para Backstage | G2,G5 | Métricas de uso do Studio acessíveis via API/query | Juju |

---

#### ALEX — Sprint 2

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| A-2.1 | Resolver gaps do G1 (identificados em A-1.6) | G1 | Cada item do checklist fechado | A-1.6 |
| A-2.2 | Eliminar dependência Google Sheets no Business Plan | G1 | Business Plan editável 100% na UI do Backstage. Import/export via CSV/JSON como fallback | — |
| A-2.3 | Dashboard executivo unificado | G5 | UMA página com: MRR, churn rate, novos clientes, receita, despesas, health score médio. Filtros: período, plano | — |
| A-2.4 | Validar G1 para fechamento | G1 | Checklist do gate verificado. Nenhuma operação financeira depende de ferramenta externa | A-2.1,A-2.2 |
| A-2.5 | Validar G5 para fechamento | G5 | Dashboard executivo aprovado por VDC/Gab | A-2.3 |

**Entregável da demo**: Dashboard executivo + G1 fechado + G5 fechado

---

#### BIEL — Sprint 2

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| B-2.1 | Migrar sync de contacts do HubSpot para criação nativa | G3 | Novos contacts criados diretamente no v2_sales (sem passar por HubSpot) | B-1.* |
| B-2.2 | Migrar sync de companies do HubSpot para criação nativa | G3 | Novas companies criadas diretamente no v2_sales | B-2.1 |
| B-2.3 | Migrar sync de deals do HubSpot para criação nativa | G3 | Novos deals criados diretamente no v2_sales | B-2.2 |
| B-2.4 | Migrar provider onboarding para não depender do HubSpot | G3 | /crm/providers funciona sem HubSpot API call | B-2.1 |
| B-2.5 | Executar "last sync" do HubSpot e congelar | G3 | Rodar sync completa final. Marcar como deprecated. Desabilitar cron de sync | B-2.1-4 |

**Entregável da demo**: CRM 100% nativo. HubSpot sync desligado.

---

#### CLARINHA — Sprint 2

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| C-2.1 | Resolver gaps do G2 (identificados em C-1.4) | G2 | Cada item do checklist corrigido | C-1.4 |
| C-2.2 | Health score calculado para 100% dos clientes ativos | G2 | Cron de health score rodando. Todos os 459 clientes com score calculado | C-1.2 |
| C-2.3 | Alertas automáticos para clientes em risco | G2 | Quando health < 40 (critical): notificação Slack + marca na UI | C-2.2 |
| C-2.4 | Conectar RetentionEngine a /api/customer-success/retention-offers | G2 | Endpoint funcional: dado um cliente, retorna oferta de retenção calculada | — |
| C-2.5 | Tab de interações no Customer 360 mostrando dados reais | G2 | Timeline unificada: tickets + conversas + WhatsApp + NPS. Dados reais, não mock | D-1.3 |

**Entregável da demo**: Health score para todos os clientes + alertas automáticos + retention offers

---

#### DAVI — Sprint 2

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| D-2.1 | Implementar ações faltantes no action-executor | G4 | send_sms (mesmo que stub com log), create_deal, send_internal_notification, webhook — todos funcionais ou com fallback logado | D-1.4 |
| D-2.2 | Garantir cron de automações (5min) rodando sem falhas | G4 | Zero erros em 7 dias. Logs estruturados. Métricas de execução | D-1.5 |
| D-2.3 | Histórico consolidado de mensagens acessível via API | G7 | GET /api/customer-360/{id}/messages retorna: WhatsApp + Email + Suporte ordenado por data | D-1.3 |
| D-2.4 | Templates de mensagem gerenciáveis | G7 | CRUD de templates (WhatsApp + Email). Time pode criar/editar templates pelo Backstage | — |
| D-2.5 | Webhook de mensagens recebidas (WhatsApp) processando | G7 | Mensagens recebidas via Evolution webhook são salvas e aparecem no histórico | D-2.3 |

**Entregável da demo**: Automações rodando + templates de mensagem + histórico consolidado

---

### SPRINT 3 — FECHAR G2, G3, G7
**Período**: 7-18 abr (2 semanas)
**Tema**: Fechar os gates de maior complexidade

---

#### GUILHERME — Sprint 3

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| G-3.1 | Validar G2 para fechamento | G2 | Checklist verificado com Clarinha e CS | C-2.* |
| G-3.2 | Validar G3 para fechamento | G3 | HubSpot desligado. CRM 100% nativo. Validado com Vendas | B-2.* |
| G-3.3 | Validar G7 para fechamento | G7 | Mensageria consolidada no monorepo. Validado com operação | D-2.* |
| G-3.4 | Apresentar status dos gates para VDC e Gab | Todos | 5/7 gates fechados (G1,G2,G3,G5,G6). G4 e G7 em fechamento | — |

---

#### ALEX — Sprint 3

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| A-3.1 | Adicionar métricas de CS ao dashboard executivo | G5 | Health score médio, clientes em risco, NPS. Atualização diária | C-2.2 |
| A-3.2 | Adicionar métricas de aquisição ao dashboard | G5 | Novos leads, conversão por etapa, CAC. Dados do v2_sales | B-2.* |
| A-3.3 | Implementar sync Banco Inter (se priorizado) | G6 | Transações bancárias sincronizando. Reconciliação automática | G-1.7 |
| A-3.4 | Polish final do módulo financeiro | G1 | Completar stubs: ERP placeholder, Recebimentos, Estruturas | — |

---

#### BIEL — Sprint 3

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| B-3.1 | Implementar sequence execution engine | G3,G4 | Cron job que: (1) busca enrollments com nextStepAt <= now, (2) envia email via Resend, (3) atualiza enrollment e step_execution | D-2.1 |
| B-3.2 | Implementar criação nativa de sequences | G3 | POST /api/crm/sequences cria sequence com steps. UI permite criar/editar | B-3.1 |
| B-3.3 | Implementar tracking de opens/clicks/replies | G3 | Webhook do Resend processando eventos. Dados visíveis na UI de sequences | B-3.1 |
| B-3.4 | Implementar exit conditions | G3 | Sequence para quando: reply detectado, meeting agendado, unsubscribe | B-3.3 |

**Entregável da demo**: Sequences funcionando end-to-end: criar, enrollar, enviar, trackear, pausar

---

#### CLARINHA — Sprint 3

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| C-3.1 | Implementar Playbooks básico | G2 | 3 playbooks pré-definidos: (1) Cliente novo — onboarding, (2) Cliente em risco — intervenção, (3) NPS detractor — recovery. Cada playbook: lista de ações + assignee + status | — |
| C-3.2 | Dashboard CS unificado | G2 | Uma página com: clientes por status de saúde, pipeline de renovações, NPS score, top 10 em risco | C-2.* |
| C-3.3 | Conectar dados do Studio (uso) ao health score | G2 | Componente "usage" do health score alimentado por dados reais de uso do Studio | G-2.4 |
| C-3.4 | Validação final G2 com stakeholders CS | G2 | Demo para time de CS. Feedback incorporado. Gate aprovado | C-3.1-3 |

---

#### DAVI — Sprint 3

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| D-3.1 | Implementar lead scoring processor | G4 | Cron job que calcula score de leads baseado em regras de lead_scoring_rules. Score visível no CRM | — |
| D-3.2 | Finalizar mensageria: consolidar TODOS os canais | G7 | Qualquer mensagem (WhatsApp, email, suporte) aparece no histórico unificado do cliente | D-2.3,D-2.5 |
| D-3.3 | UI de automações básica | G4 | Página no Backstage para: listar automações, ativar/desativar, ver enrollment count, ver logs de execução | — |
| D-3.4 | Testar automações end-to-end | G4 | Cenário completo: trigger (contact created) → ação (send email) → log (execução registrada) | D-2.1,D-2.2 |

**Entregável da demo**: Lead scoring + mensageria unificada + UI de automações

---

### SPRINT 4 — FECHAR G4 + BUFFER + VALIDAÇÃO FINAL
**Período**: 21-30 abr (10 dias)
**Tema**: Fechar último gate, polish, validação com stakeholders

---

#### GUILHERME — Sprint 4

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| G-4.1 | Validar G4 para fechamento | G4 | Automações internas substituem ferramentas externas críticas | D-3.* |
| G-4.2 | Validar G7 para fechamento | G7 | Mensageria 100% no monorepo. Histórico consolidado | D-3.2 |
| G-4.3 | Gate review completa (7/7) com VDC e Gab | Todos | Apresentação formal: cada gate demonstrado e aprovado | Todos |
| G-4.4 | Documentar estado final + handoff para Fase 1 | Todos | Documento de estado de cada módulo. Débito técnico catalogado | — |
| G-4.5 | Retrospectiva da Fase 0 | Todos | Lições aprendidas. O que manter, mudar, parar | Time |

---

#### ALEX — Sprint 4

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| A-4.1 | Polish dashboard executivo | G5 | Feedback de VDC/Gab incorporado. Performance otimizada | — |
| A-4.2 | Testes end-to-end do módulo financeiro | G1 | Cenários: nova assinatura → MRR atualiza → DRE reflete → dashboard mostra | — |
| A-4.3 | Documentar integração financeira | G1,G6 | Runbook de cada integração: como funciona, como debugar, como resetar | — |

---

#### BIEL — Sprint 4

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| B-4.1 | Testes end-to-end do CRM nativo | G3 | Cenários: criar lead → qualificar → criar deal → mover pipeline → fechar | — |
| B-4.2 | Testes end-to-end de sequences | G3 | Cenário: criar sequence → enrollar 10 contacts → emails enviados → opens trackados | B-3.* |
| B-4.3 | Confirmar HubSpot 100% desligado | G3 | Remover HUBSPOT_ACCESS_TOKEN de env. Nada quebra. Cron de sync desabilitado | — |
| B-4.4 | Documentar CRM/Pipeline/Sequences | G3 | Como usar, como debugar, como criar sequences | — |

---

#### CLARINHA — Sprint 4

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| C-4.1 | Testes end-to-end do CS | G2 | Cenários: novo cliente → health calculado → score baixo → alerta → playbook acionado | — |
| C-4.2 | Treinamento do time de CS na ferramenta | G2 | Sessão de 1h com CS. Documentação básica de uso | — |
| C-4.3 | Documentar Customer 360 e Health Score | G2 | Como funciona o cálculo, como interpretar, como agir | — |

---

#### DAVI — Sprint 4

| # | Tarefa | Gate | Critério de aceite | Dep. |
|---|--------|------|--------------------|------|
| D-4.1 | Testes end-to-end de automações | G4 | 3 cenários: marketing automation, lead scoring trigger, sequence execution | — |
| D-4.2 | Testes end-to-end de mensageria | G7 | Enviar WhatsApp + Email pelo Backstage. Resposta aparece no histórico | — |
| D-4.3 | Documentar automações e mensageria | G4,G7 | Como criar automação, como enviar mensagem, como ver histórico | — |
| D-4.4 | Listar ferramentas externas ELIMINADAS vs MANTIDAS | G4 | Lista final: o que saiu, o que ficou (e por que ficou) | — |

---

## PARTE 5: DEPENDÊNCIAS ENTRE TIMES

```
TIME BACKSTAGE depende de:

MIGRAÇÃO (Juju):
├── Monorepo estável e deployável          → Semana 1 (crítico)
├── Dados do Studio acessíveis via query   → Semana 4 (para health score)
├── Métricas de uso do Studio              → Semana 4 (para dashboard)
└── 100% clientes migrados                 → Semana 8 (para G2 completo)

GTM (Didico):
├── Definição de ICP                       → Semana 1 (para segmentação)
├── Definição de planos/pricing            → Semana 2 (para financeiro)
├── Narrativa de venda                     → Semana 4 (para sequences)
└── Jornada do cliente                     → Semana 4 (para playbooks CS)

PESSOAS (Ju H):
└── Framework construtores/consultores     → Informativo (não bloqueia)
```

---

## PARTE 6: CRITÉRIOS DE FECHAMENTO DOS GATES

### G1 — Gestão financeira centralizada
- [ ] Toda operação financeira consultável no Backstage
- [ ] Zero dependência de planilhas externas para dados financeiros
- [ ] Business Plan editável sem Google Sheets
- [ ] Sync Iugu + Conta Azul estável (7 dias sem falha)
- [ ] DRE, KPIs, Unit Economics funcionando com dados reais

### G2 — Acompanhar Cliente v1
- [ ] Customer 360 funcional para qualquer cliente ativo
- [ ] Health score calculado para 100% dos clientes
- [ ] Alertas automáticos para clientes critical (< 40)
- [ ] Timeline de interações consolidada (WhatsApp + email + suporte)
- [ ] Retention offers calculáveis via API
- [ ] 3 playbooks CS operando

### G3 — Pipeline e CRM unificados
- [ ] HubSpot sync desligado
- [ ] Contacts, companies, deals criados nativamente
- [ ] Pipeline de vendas 100% no Backstage
- [ ] Outbound pipeline funciona sem HubSpot
- [ ] Sequences criadas e executadas nativamente
- [ ] Provider onboarding sem HubSpot

### G4 — Automações internas
- [ ] Marketing automations rodando (cron 5min, ações funcionais)
- [ ] Lead scoring processor ativo
- [ ] Ferramentas externas de automação eliminadas (ou justificadas)
- [ ] UI para gerenciar automações no Backstage
- [ ] Logs de execução acessíveis

### G5 — Dashboard unificado de métricas
- [ ] UM dashboard executivo: MRR, churn, aquisição, operação, CS
- [ ] Filtros: período, plano, cohort
- [ ] Dados em tempo real (ou <24h)
- [ ] Aprovado por VDC e Gab

### G6 — Integrações estabilizadas
- [ ] Auditoria das 14 integrações completa
- [ ] Monitoramento ativo com alertas (Sentry/Slack)
- [ ] Zero falhas críticas em 7 dias
- [ ] Documentação de cada integração

### G7 — Mensageria no monorepo
- [ ] WhatsApp enviando e recebendo dentro do monorepo
- [ ] Email transacional funcionando (Resend)
- [ ] Histórico consolidado por cliente (todos os canais)
- [ ] Templates gerenciáveis pelo time
- [ ] Webhook de respostas processando

---

## PARTE 7: RISCOS E MITIGAÇÕES

| # | Risco | Prob. | Impacto | Mitigação | Owner |
|---|-------|-------|---------|-----------|-------|
| R1 | HubSpot migration mais complexa que esperado | Alta | Crítico | Plano B: dual-run (Backstage primário, HubSpot read-only) | Biel |
| R2 | Migração atrasa e bloqueia dados do Studio | Alta | Alto | Trabalhar com dados mock até integração. Comitê quinta | Guilherme |
| R3 | Sequence execution engine não fica pronto | Média | Alto | Priorizar: criar+enrollar manual. Envio automático vem depois | Biel |
| R4 | Stakeholders pedem mudanças de UX no Sprint 4 | Média | Médio | Envolver stakeholders no Sprint 2 (cedo, não tarde) | Clarinha |
| R5 | 167 routes sem auth exploradas em produção | Baixa | Crítico | Priorizar routes públicas. WAF como mitigação imediata | Guilherme |
| R6 | Time sobrecarregado com bugs em produção | Média | Alto | Regra: max 20% do tempo em bugs. Resto é construção | Guilherme |
| R7 | Escopo cresce além dos 7 gates | Alta | Alto | Este documento é bounding. Não entra sem aprovação | Guilherme |

---

## PARTE 8: COMUNICAÇÃO

### Canais
- **Daily**: Slack #backstage-daily (async antes das 9h, sync às 9h)
- **Bloqueios**: Slack #backstage-bloqueios (qualquer hora, resposta em <4h)
- **PRs**: GitHub, com review obrigatório de Guilherme
- **Decisões**: Registradas neste documento (changelog)

### Escalação
```
Bloqueio técnico → Sync quarta com Guilherme (ou Slack imediato se urgente)
Bloqueio de dependência → Guilherme resolve em 24h ou escala para VDC
Bloqueio de decisão → VDC decide em 48h ou escala para Gab
```

### Report semanal (sexta)
Cada pessoa preenche:
```
NOME:
ENTREGUE esta semana: [lista]
BLOQUEADO em: [lista ou "nada"]
FOCO semana que vem: [lista]
GATE status: G_ = __% (mudou +_% esta semana)
```

---

## CHANGELOG

| Data | Versão | Alteração | Autor |
|------|--------|-----------|-------|
| 2026-03-09 | 1.0 | Documento criado com diagnóstico real e plano completo | Guilherme + Claude |
