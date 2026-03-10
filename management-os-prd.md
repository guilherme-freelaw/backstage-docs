# BACKSTAGE — PRD: Management OS (Gestão, Reports, AI Cockpit)

> PRD dedicado da macro-área MA-MGMT.
> Para o PRD geral do Backstage, veja `backstage-prd.md`.
> Para o plano de trabalho, veja `backstage-plano-de-trabalho.md`.

**Versão**: 1.0 | **Data**: 2026-03-09
**Owner**: Guilherme (VDC), Diretor Financeiro
**Stakeholders**: Ju (CEO), Carol (Gerente CS), Didico (Gerente Aquisição)

---

## 0. VISÃO E PRINCÍPIO

**Visão**: Cada pessoa na Freelaw abre o Backstage e encontra um **AI Cockpit** com:
- O que é mais importante fazer **hoje** (prioridades contextualizadas),
- Números do negócio ligados ao seu papel,
- Metas e combinados que precisa cumprir (com cobrança e evidência),
- Chat/assistente que acelera execução,
- Ritmos de gestão (pulses, weekly, QBR) acontecendo de forma automática.

**Princípio central**: _"A IA não é um relatório: ela é o sistema operacional da gestão."_
Ela observa dados, pergunta o que falta, documenta decisões, cobra combinados, propõe plano, e mede resultado.

**Visão de empresa**: A Freelaw é uma empresa **powered by AI**. O CEO da Freelaw é uma IA — humanos representam a IA por enquanto. O sistema de gestão precisa ser desenhado para que a IA tenha **contexto máximo** para operar.

---

## 1. PROBLEMA QUE RESOLVE

Hoje, gestão e acompanhamento tendem a virar:
- **Dashboards soltos** (sem ação)
- **Rituais manuais** (sem evidência)
- **Decisões que não viram execução**
- **Falta de contexto narrativo** (qualitativo) do que está acontecendo no campo

Este PRD cria um sistema em que:
- Números + narrativas + decisões + tarefas + evidências ficam **conectados**
- O "CEO AI" transforma sinais em **direcionamento semanal**
- Cada time vê seu cockpit com **o que importa**

---

## 2. OBJETIVOS DO PRODUTO

### 2.1 Objetivos

1. **Ritmo de gestão automático** (diário/semanal/mensal) com mínimo esforço humano
2. **Single Source of Truth**: KPIs coesos entre Financeiro, Vendas, Marketing, CS
3. **Execução guiada por IA**: todo insight vira ação (owner, prazo, evidência)
4. **Captura de contexto do "chão"** (pulses/áudios) para alimentar decisões e PRDs
5. **Personalização por função**: a tela inicial muda por role e responsabilidades

### 2.2 Métricas de sucesso (KPIs do próprio sistema)

| Métrica | Descrição | Target |
|---------|-----------|--------|
| WAU por área | % de usuários ativos semanais no Backstage | > 80% |
| Taxa de conclusão de pulses | Pulses respondidos / pulses enviados | > 70% |
| Tempo médio de resposta pulse | Latência entre envio e resposta | < 2min (daily), < 5min (weekly) |
| Insights → Ações | % de insights que viram ações com owner e prazo | > 50% |
| Lead time detecção → evidência | Detecção → decisão → execução → evidência | < 7 dias |
| NPS interno | Qualidade percebida do Backstage / utilidade do cockpit | > 40 |
| Redução de tempo em reports | Tempo manual para produzir report semanal (antes vs depois) | -70% |

---

## 3. PERSONAS E COCKPITS

### 3.1 Personas iniciais

| Persona | Necessidade principal | Cockpit foco |
|---------|----------------------|--------------|
| **IC (individual contributor)** | "O que fazer hoje" + contexto mínimo | Tarefas prioritárias + chat IA |
| **Líder de área** | "Ver funil e riscos" + garantir execução + remover bloqueios | KPIs da área + combinados do time |
| **C-level/Founders** | "Saúde do negócio" + trade-offs + plano semanal | North star + riscos + decisões |
| **Operações/CS** | "Estado do cliente" + risco churn + plano de retenção | Health score + clientes prioritários |
| **Financeiro** | "Caixa, MRR, inadimplência, forecast vs real" | Caixa D+0 + alertas de desvio |

### 3.2 Cockpit por role

**Vendas**:
- Pipeline e forecast (semana/mês), atividades recomendadas, deals em risco
- "Hoje": top 5 contas para atacar + scripts e e-mails gerados pela IA

**Marketing**:
- Budget, CAC, leads, conversão por canal, experimentos em andamento
- "Hoje": sugestões de testes e otimizações + conteúdo a produzir

**CS**:
- Health score, churn risk, NPS, tickets, onboarding status
- "Hoje": lista priorizada de clientes para ação + playbooks

**Financeiro**:
- Caixa D+0, runway, inadimplência, MRR/ARR, margem, variações do BP
- "Hoje": cobranças prioritárias + alertas de desvios e decisões sugeridas

**Executivo**:
- North Star + KPIs vitais (crescimento, margem, churn, satisfação, caixa)
- "Esta semana": narrativa executiva + top riscos + decisões pendentes

---

## 4. COMPONENTES DO MANAGEMENT OS

### 4.1 Dashboards "vivos" (dados quantitativos)

**Camadas**:
- Company-level (north star)
- Area-level (por macro-área)
- Squad/Owner-level (por pessoa)

Cada KPI deve ter:
- Definição (fonte, fórmula, janela)
- Target e tolerâncias
- Tendência e decomposição (drivers)
- Explicação automática (IA)

### 4.2 Pulses (dados qualitativos e sinais do campo)

Mecanismo leve de captura de contexto humano para a IA.

| Tipo | Cadência | Duração | Conteúdo |
|------|----------|---------|----------|
| **Daily Pulse** | Diário | 30-60s | "Qual o maior desafio hoje? O que você vai destravar?" |
| **Weekly Pulse** | Semanal | 2-4min | "Top 3 vitórias, top 3 riscos, pedidos de ajuda" |
| **Incident Pulse** | Ad hoc | Variável | Quando a IA detecta sinal anômalo (queda de conversão, churn risk subiu) |

**Formatos**: texto curto + opção de áudio (transcrição automática) + tags (cliente, tema, área)

**A IA usa pulses para**:
- Completar o "porquê" por trás do número
- Justificar decisões
- Alimentar PRDs e protótipos
- Criar "tema da semana" do time

### 4.3 Weekly Report automático (narrativa + plano)

Toda semana o "CEO AI" gera:

| Seção | Conteúdo |
|-------|----------|
| **Resumo executivo** | O que aconteceu e por quê (quant + qual) |
| **Riscos & oportunidades** | Priorização (impacto x urgência) |
| **Decisões sugeridas** | 3-7 decisões "high leverage" |
| **Plano de ação** | Tarefas, owners, prazos, evidências esperadas |
| **Perguntas pendentes** | O que precisa ser respondido por humanos |

**O sistema acompanha**: quem leu, o que foi aceito/recusado, o que virou execução.

### 4.4 Combinados (commitments) e evidências

Objeto rastreável:

```
Commitment {
  título, descrição
  owner
  data (deadline)
  escopo (time/cliente)
  KPI relacionado (ou hipótese)
  evidência esperada (link, screenshot, documento, número)
  status: open | in_progress | at_risk | done | waived
  logs automáticos (o que mudou, por quem, baseado em quais dados)
}
```

**A IA**:
- Cobra e lembra
- Detecta atraso
- Sugere renegociação quando necessário
- Registra justificativa (para aprender)

---

## 5. CEO AI: COMPORTAMENTO E MODOS

### 5.1 Funções

| Função | Descrição |
|--------|-----------|
| **Sensemaking** | Juntar KPIs + pulses + histórico para explicar a semana |
| **Prioritização** | Escolher alavancas com base em impacto e restrições |
| **Orquestração** | "Peça input", "marque reunião", "mande pulse", "crie tarefa" |
| **Cobrança** | Acompanhar combinados e gerar follow-ups |
| **Documentação** | Gerar PRDs, specs e protótipos baseados no que ouviu/viu |

### 5.2 Modos de operação

| Modo | Descrição | Quando |
|------|-----------|--------|
| **Autopilot** | Sugere, humano aprova | Padrão — decisões recorrentes |
| **Copilot** | Humano pede, IA executa | Tarefas sob demanda |
| **Ops Mode** | IA detecta anomalia e aciona playbook | Incidentes e sinais críticos |

---

## 6. REQUISITOS FUNCIONAIS

### 6.1 Home / Cockpit

A home depende de:
- Role do usuário
- Squads e responsabilidades
- Metas ativas
- Clientes sob responsabilidade
- Tarefas e combinados
- Sinais (alertas) daquele dia

**Componentes mínimos na home**:
1. **Top priorities (hoje)**: 3-7 cards com "por quê", "próximo passo", "botões de ação"
2. **Metas e progresso**: targets + tendência + gap
3. **Alertas**: anomalias e riscos (com explicação)
4. **Chat com IA**: com ferramentas (gerar e-mail, resumo, plano, checklist, PRD)
5. **Combinados**: próximos vencimentos e atrasos

### 6.2 Pulses

- Criar pulse diário/semanal com templates por área
- Suportar áudio → transcrição (Whisper)
- Tagging: cliente, tema, severidade, blockers
- A IA sumariza pulses do time e cria "tema da semana"

### 6.3 Weekly Report

- Geração automática em dia e hora definidas
- Revisão (modo "approve") por liderança
- Publicação para público-alvo (por time e por nível)
- Comentários e "aceite" de decisões

### 6.4 Objetos de gestão

| Objeto | Relacionamentos |
|--------|----------------|
| KPI | → Alert → Decision |
| Decision | → Actions → Evidence |
| Evidence | → KPI (feedback loop) |
| Commitment | → Actions → Evidence |
| Pulse | → Decision (contexto qualitativo) |

---

## 7. REQUISITOS NÃO-FUNCIONAIS

- **Confiabilidade de dados**: Versionamento de definição de KPI, logs de cálculo, "data freshness"
- **Auditoria**: Qualquer decisão gerada pela IA referencia dados e pulses usados
- **Permissões**: RLS por área e papel; dados sensíveis só para quem deve ver
- **Design system**: Componentes padronizados do @freelaw/ui
- **Observabilidade**: Métricas de geração de report, falhas de pipeline, latência
- **Privacidade/consentimento**: Pulses em áudio com aviso claro e política de uso interno

---

## 8. KNOWLEDGE SCHEMA

### 8.1 O que a IA precisa como contexto máximo

1. KPIs (série temporal + definições + targets)
2. Eventos (lançamentos, incidentes, campanhas)
3. Pulses (texto/áudio transcrito + tags)
4. Registro de decisões e combinados (com outcomes)
5. Estrutura org (times, roles, responsabilidades)
6. Objetivos estratégicos (OKRs, metas trimestrais)
7. Estado do cliente (CRM, saúde, tickets, faturamento)

### 8.2 Knowledge Schema (tabelas)

| Tabela | Conteúdo |
|--------|----------|
| `knowledge_events` | Fatos relevantes (com fonte) |
| `knowledge_pulses` | Respostas humanas (com tags e embedding) |
| `knowledge_decisions` | Decisões, rationale, efeitos esperados |
| `knowledge_outcomes` | Evidências e resultados |
| `kpi_definitions` | Fórmula, fonte, cadência, owner |
| `kpi_values` | Séries históricas (com timestamps e freshness) |
| `management_actions` | Tarefas e playbooks |

---

## 9. RITMOS DE GESTÃO

### Diário
- Pulse diário (ICs e líderes)
- Alertas automáticos (anomalia)
- "Top 5 prioridades" por role

### Semanal
- Weekly report executivo + por área
- 3-7 decisões sugeridas
- Plano semanal com owners

### Mensal/Trimestral
- Review BP vs Realizado
- QBR: churn, NPS, pipeline, caixa, eficiência
- Aprendizado: "o que funcionou" (IA treina playbooks internos)

---

## 10. MVP (V1) — ESCOPO MÍNIMO

| Componente | Escopo MVP |
|------------|-----------|
| **AI Cockpit** | Home personalizada por role (4 roles: Vendas, CS, Marketing, Financeiro) |
| **Daily Pulse** | Texto + templates por área |
| **Weekly Pulse** | Texto + áudio opcional (transcrição Whisper) |
| **Weekly Report** | 1 executivo + 4 por área, gerado automaticamente |
| **Combinados** | CRUD com owner/prazo + acompanhamento + evidência |
| **Alertas** | Thresholds em 5 KPIs vitais |
| **Design System** | Cards, charts, tables, action buttons (já existe no @freelaw/ui) |

---

## 11. FASES (ROADMAP)

### Fase 1: "Gestão que se escreve sozinha"
- Pulses → síntese → weekly report → ações
- _Valor_: time gasta zero horas escrevendo reports

### Fase 2: "Gestão que executa"
- IA cria drafts de e-mails, tasks, tickets, updates de CRM/BP
- _Valor_: IA não só reporta, ela age

### Fase 3: "Gestão que aprende"
- Loop completo: ações → evidências → outcomes → playbooks melhores
- _Valor_: cada ciclo melhora o próximo automaticamente

---

## 12. CRITÉRIOS DE ACEITAÇÃO

1. Um usuário de CS abre o cockpit e vê:
   - 5 clientes priorizados com motivo (dados + pulse)
   - Botões "enviar mensagem", "criar plano", "abrir ticket"

2. Weekly report:
   - Sempre referencia dados (KPI X caiu Y% em período Z)
   - Inclui pelo menos 3 decisões propostas
   - Cria automaticamente actions com owner e prazo

3. KPI "churn" aparece igual no Financeiro e no CS (mesma definição e fonte)

4. Combinado criado na segunda → acompanhado diariamente → cobrado na sexta com evidência

---

## 13. RISCOS E MITIGAÇÃO

| Risco | Mitigação |
|-------|-----------|
| IA alucinar narrativa | Exigir citações internas (dados/pulses) para cada afirmação crítica |
| Excesso de pulse (fadiga) | Pulses curtos, amostragem, gatilhos por anomalia |
| Dados inconsistentes entre áreas | Governança de KPI definitions + "single definition" |
| Adoção baixa | Cockpit precisa entregar valor diário real (menos "dashboard", mais "ação") |

---

## 14. ENTREGÁVEIS PARA PROTOTIPAGEM

- [ ] Mapa de telas (Figma): Home/Cockpit por role + Pulse modal + Weekly Report + Commitment detail
- [ ] Glossário de objetos: KPI, Alert, Decision, Action, Evidence, Pulse
- [ ] Lista de KPIs vitais v1 (por área) e suas definições
- [ ] Templates de pulses (diário/semanal) por área
- [ ] Template de weekly report (executivo e por área)
- [ ] Fluxos: "alerta → pergunta → decisão → ação → evidência"

---

## CHANGELOG

| Data | Versão | Alteração | Autor |
|------|--------|-----------|-------|
| 2026-03-09 | 1.0 | Documento criado a partir de briefing de Guilherme (VDC) | Guilherme + Claude |
