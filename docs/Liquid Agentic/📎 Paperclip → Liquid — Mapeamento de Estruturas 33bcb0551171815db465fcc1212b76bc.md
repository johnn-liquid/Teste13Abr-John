# 📎 Paperclip → Liquid — Mapeamento de Estruturas

# Objetivo

Mapear as estruturas do Paperclip que podem ser aproveitadas na Liquid, com foco em **ação e tarefa** — especificamente Issues (unidade de trabalho), Mesa de decisão (Kanban/aprovações), e Inbox (fluxo de aprovação entre usuários).

Correlacionado com os domínios D1–D10 e contextos E1–E4 da Liquid.

---

# 1. Issue como unidade atômica de trabalho

## O que o Paperclip faz

Issue é o objeto central. Cada issue tem ID sequencial (LIQ-n), título, status, assignee, data. Pode ser criada com ou sem agente vinculado. Vive dentro de um Project ou solta no workspace. Status: Backlog → Todo → In Progress → In Review → Done.

## O que a Liquid precisa absorver

Hoje nosso modelo é **chat-first** (modelo Claude): a unidade de trabalho é a conversa. Mas o Paperclip revela que para o mercado financeiro regulado, a conversa não basta — precisa de **rastreabilidade, status, assignee, e pipeline**.

## Proposta para a Liquid

A **Task** da Liquid (que hoje é basicamente uma conversa no chat) precisa evoluir para uma **Issue com ciclo de vida**:

| Atributo | Liquid (atual) | Liquid (proposta) | Ref. domínio |
| --- | --- | --- | --- |
| Identificador | Sem ID visível | ID sequencial (LIQ-n) | D1 (rastreabilidade) |
| Status | Sem status formal | Backlog → Em andamento → Em revisão → Concluída | Novo — Tasks & Cases |
| Assignee | Implícito (quem criou) | Explícito: pessoa ou agente | D1 (Identity) |
| Projeto | Tag na sidebar | Vínculo formal a um Project | D1 (Projects) |
| Agente vinculado | Orchestrator decide | Pode ser vinculado na criação | D6 (Agents) |
| Aprovação | Não existe | Status "Em revisão" + approval flow | Novo — Inbox |
| Auditoria | Log genérico | Toda mudança de status registrada | D1 regra 9, Context Map T1.6 |

## Impacto nos domínios

- **D1 Identity:** Issue precisa de assignee rastreável. Multiplayer: uma issue pode ser passada de uma pessoa para outra.
- **D5 Skills:** A skill executada dentro de uma issue fica vinculada ao registro.
- **D6 Agents:** O agente pode ser vinculado na criação da issue (padrão Paperclip). Isso é diferente do modelo atual onde o Orchestrator decide invisível.
- **D9 Liquidity:** O funil de liquidez pode ser mapeado como pipeline de issues: Análise → Qualificação → Proposta → Transação.
- **D10 Platform Ops:** Issues com status permitem métricas operacionais (tempo médio por etapa, gargalos).

---

# 2. Mesa de decisão (Kanban + Aprovações)

## O que o Paperclip faz

Kanban é uma **view** do Project, não uma entidade separada. 5 estados fixos. Opcional por escolha do usuário. O caso da incorporadora mostra o fluxo: corretor cria issue com CPF → fica parada em "Onboarding" → analista pega, roda Serasa → move para "In Progress" → segue o pipeline.

## O que a Liquid precisa absorver

No mercado financeiro, **nem toda decisão é automática**. Existem momentos em que:

- Um agente qualifica um ativo mas precisa de aprovação humana para avançar.
- Uma proposta de crédito precisa passar por uma mesa de aprovação.
- Um documento precisa de revisão antes de ser enviado ao cliente.

Isso é exatamente o **Human-in-the-Loop** do Context Map (5.5.8), mas materializado como **view de pipeline**.

## Proposta para a Liquid

### Pipeline como view de Issues

Assim como o Paperclip trata o Kanban como view do Project, a Liquid pode ter:

**View Kanban dentro do Project** — colunas representam estágios do pipeline. Configurável por projeto. Exemplos:

- Projeto "Análise de Carteiras": Recebida → Em análise → Score calculado → Proposta gerada → Concluída
- Projeto "Mesa de Crédito": Entrada → Análise automática → Aguardando aprovação → Aprovada/Rejeitada → Liquidez

**Coluna "Aguardando aprovação"** — é o ponto de Human-in-the-Loop. A issue fica travada até que um membro com permissão aprove ou rejeite. Isso gera um item no Inbox.

### Status model proposto (V1)

| Status | Descrição | Quem move |
| --- | --- | --- |
| Backlog | Criada, sem ação ainda | Usuário |
| Em andamento | Agente ou usuário trabalhando | Agente/Usuário |
| Aguardando aprovação | Precisa de decisão humana | Agente (automático) |
| Aprovada | Decisão tomada, segue fluxo | Aprovador (via Inbox) |
| Concluída | Trabalho finalizado | Agente/Usuário |
| Rejeitada | Decisão negativa | Aprovador (via Inbox) |

## Impacto nos domínios

- **D1 Identity:** Quem pode aprovar? Na V1, qualquer Admin ou Member do Project. Futuramente: roles de aprovador.
- **D2 Billing:** Ações pagas (consulta bureau) podem ser o gatilho do "Aguardando aprovação" — o agente para e pede OK antes de gastar crédito.
- **D6 Agents:** O Orchestrator precisa saber que, para certas ações, deve mover a issue para "Aguardando aprovação" em vez de executar direto.
- **D9 Liquidity:** O pipeline de liquidez (qualificação → proposta → transação) é naturalmente um Kanban com aprovações.
- **Context Map 5.5.8:** Human-in-the-Loop materializado como coluna no Kanban + item no Inbox.

---

# 3. Inbox como hub de decisão

## O que o Paperclip faz

Inbox não é só notificação — é um **hub de decisão**. Tabs: Mine, Recent, Unread, All. Cada item pode ser uma approval request de um Agent ou uma notificação de issue. Pedro destaca: "uma mesa de crédito — preciso mandar pra Luana aprovar pra mim".

## O que a Liquid precisa absorver

O Inbox resolve o problema de **comunicação assíncrona entre pessoas e agentes** em um contexto regulado:

- Agente quer gastar crédito (consulta bureau) → pede aprovação
- Agente qualificou ativo para liquidez → pede confirmação humana
- Membro do time precisa que outro membro revise uma análise
- Sistema detecta anomalia → alerta quem precisa decidir

## Proposta para a Liquid

### Estrutura do Inbox

| Elemento | Descrição | Ref. domínio |
| --- | --- | --- |
| Approval Requests | Pedidos de aprovação de agentes ou membros. Ação: Aprovar / Rejeitar | D6 (Agents), D2 (Billing gate) |
| Issue Notifications | Mudanças de status em issues que me afetam (assignee, menção, projeto) | D1 (multiplayer) |
| System Alerts | Alertas de billing (80% do crédito), erros de conector, anomalias | D2, D4, D10 |
| Agent Completions | Agente concluiu tarefa — resultado disponível para review | D6, D5 |

### Filtros (modelo Paperclip adaptado)

- **Pendentes** — itens que requerem minha ação (aprovações, decisões)
- **Recentes** — últimas 24h
- **Não lidas** — tudo que não vi
- **Todas** — histórico completo

### Fluxo de aprovação

1. Agente (ou membro) cria approval request vinculada a uma Issue
2. Request aparece no Inbox do aprovador com contexto: quem pediu, por quê, qual o custo/impacto
3. Aprovador decide: Aprovar / Rejeitar / Pedir mais informação
4. Issue avança ou volta no pipeline (Kanban)
5. Decisão registrada no audit log (D1 regra 9)

## Impacto nos domínios

- **D1 Identity:** Inbox é por usuário. Cada pessoa vê só o que é relevante para ela.
- **D2 Billing:** Gate de billing pode gerar approval request automático ("agente quer consultar bureau, custo R$ 0,50, aprovar?").
- **D6 Agents:** Agentes precisam saber como criar approval requests (novo tipo de output).
- **D10 Platform Ops:** Métricas de tempo de resposta em aprovações (SLA interno).
- **Context Map T1.6:** Audit trail de todas as decisões tomadas via Inbox.

---

# 4. Resumo: o que muda na arquitetura da Liquid

## Novos conceitos (vindos do Paperclip)

| Conceito | Status | Prioridade V1 | Domínios impactados |
| --- | --- | --- | --- |
| Issue com ciclo de vida (status, assignee, ID) | Novo | Alta | D1, D5, D6, D9 |
| Kanban como view do Project | Novo | Média | D1 (Projects) |
| Inbox como hub de decisão | Novo (já proposto nos wireframes) | Alta | D1, D2, D6, D10 |
| Approval flow (agente → humano → decisão) | Novo | Alta | D6, D2, Context Map 5.5.8 |
| Pipeline customizável por Project | Novo | Média | D1 (Projects) |
| Agente vinculado na criação da issue | Novo | Média | D6 |

## O que NÃO copiar do Paperclip

- **Agents como personas nomeadas (CEO, CTO):** Na Liquid, o Orchestrator roteia. O usuário não escolhe o agente. (Decisão CEO: Agent First)
- **Skills como arquivos .md no GitHub:** Na Liquid, skills são configuradas pelo Admin em Settings, não por código. (D5)
- **Routines, Goals:** Módulos secundários sem detalhes. Não prioritários para V1.
- **Dashboard plan-gated:** Na V1, dashboards não são prioridade. O chat é a interface principal.

## Impacto nos wireframes

Os wireframes atuais já contemplam Notifications (Inbox). Precisam ser atualizados para:

1. **Chat (E2):** Issue com status visível no header da conversa. Botão para mover status.
2. **Projects:** Adicionar view Kanban opcional dentro do projeto.
3. **Notifications:** Renomear para Inbox. Separar "Pendentes de ação" (approvals) de "Atividade" (notificações).
4. **Sidebar (E4):** Issues recentes mostram status além do tag de projeto.

---

# 5. Pontos em aberto

---

# 🇺🇸 English

*Translation pending — will be added after PT version is reviewed and approved.*