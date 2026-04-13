# Landing (Open Layer)

Category: Experience
Consumers: E2, E3, E4
Dependencies: D1, D2, D3, D6, D9
ID: E1
Status: Drafting
Type: Open Layer

# 🇧🇷 Português

## Metadados

- **Área:** Experience Context
- **ID:** E1
- **Tipo:** Open Layer
- **Owner de design:** Luana Siqueira
- **Owner técnico:** A definir
- **Dependências:** D1 Identity, D2 Billing, D3 Localization, D6 Agents, D9 Liquidity
- **Quem consome:** E2 Chat, E3 Settings, E4 Sidebar (transição pós-login)

---

## Definição

Landing (Open Layer) é a experiência pública da Liquid — tudo o que um visitante vê e faz antes de se autenticar. É a primeira camada do funil de 4 camadas definido em D9 Liquidity: **aberta → conversão → fidelização → liquidez**.

Resolve três problemas simultâneos: **atrair** (SEO + conteúdo de mercado), **demonstrar valor** (input + preview parcial), e **converter** (login wall no momento certo). Não é uma landing page estática — é uma experiência funcional onde o visitante interage com o produto real antes de qualquer cadastro.

A camada aberta é produto, não marketing. O visitante interage com o sistema real — só que em modo stateless, sem persistência e sem conectores pagos.

---

## Princípios aplicáveis

**PLG (Product-Led Growth) →** O produto vende sozinho. A landing não empurra cadastro — demonstra valor. O visitante experimenta o produto antes de criar conta. O login wall aparece apenas quando o sistema já provou utilidade via preview parcial.

**Liquidity Driven →** A camada aberta é o topo do funil de liquidez (D9). O conteúdo de mercado exposto publicamente (indicadores, tendências, oportunidades) atrai o perfil de usuário que eventualmente chega à transação. SEO e conteúdo público são investimento em pipeline.

**Global →** O conteúdo da camada aberta se adapta à localidade do visitante (D3). Indicadores de mercado, moeda de exibição, idioma do conteúdo SEO — tudo varia por região. O visitante brasileiro vê dados em BRL e português; o internacional vê em USD e inglês.

**Agent First →** Mesmo na camada aberta, a interação é via agente orquestrador (D6). O input do visitante é processado pelo mesmo sistema de agentes do produto completo — apenas com restrições de escopo (sem conectores pagos, sem persistência). A experiência pré e pós-login é contínua.

---

## Estrutura da tela

A Landing é composta por componentes fixos (estruturais) e variáveis (conteúdo muda por contexto). Não é uma tela estática — é um sistema de slots.

### Fixo (estrutural)

**Header / Top Bar**

Logo Liquid + seletor de idioma + botão "Entrar". Sempre presente. Sem sidebar (aparece só após login).

**Painel de Mercado (Market Ticker)**

Faixa de indicadores financeiros no topo, abaixo do header. Mostra métricas públicas: volume negociado, carteiras analisadas, taxa média, inadimplência. Dados reais, atualizados. Serve como demonstração de inteligência da plataforma e conteúdo indexável para SEO.

**Barra de Input**

Campo de texto central — ponto de entrada único. Funciona como substituto de navegação tradicional. O visitante digita o que quer saber, não navega por menus. Placeholder contextual: "Pergunte qualquer coisa sobre crédito, carteiras ou mercado...".

**Preview Parcial**

Resultado limitado entregue ao visitante deslogado após o input. Mostra a estrutura completa da resposta + primeiras métricas visíveis. Restante em blur/truncado. Fórmula: suficiente para demonstrar valor, insuficiente para substituir o uso real.

**Login Wall**

Modal/overlay que aparece após o primeiro input + entrega de preview parcial. Posicionado sobre o conteúdo blurrado. Login social (Google, Microsoft) como caminho primário. E-mail/senha como fallback. Mensagem: "Seu resultado completo está pronto — crie sua conta para acessar." (D1)

### Variável (conteúdo muda por contexto)

**Sugestões de Input**

Prompts sugeridos abaixo da barra de input. Orientam o visitante sobre o que perguntar. Variam por localidade (D3), contexto de mercado e tendências. Funcionam como onboarding implícito: ensinam o que o produto faz sem explicar.

**Conteúdo SEO**

Páginas e blocos indexáveis por buscadores. Adaptados por localidade e mercado. Na V1 pode ser mínimo (painel de mercado já é conteúdo SEO). Cresce iterativamente.

**Social Proof / Indicadores de Atividade**

Sinais de uso real da plataforma: volume processado, número de operações, indicadores agregados. Cria confiança sem expor dados individuais. Pode incluir: "R$ 48B+ em ativos analisados", "2.300+ empresas ativas", "< 3min para primeira análise".

### Gates (transição free → pago)

**Gate de Autenticação**

Acionado após primeiro input + entrega de preview parcial. Lógica: demonstrou valor → solicita cadastro → revela resultado completo. (D1 regra 10)

**Gate de Billing**

Não aparece diretamente na camada aberta (pré-login é sempre free). Mas a lógica precisa estar mapeada para continuidade pós-login: se o usuário converteu e tenta ação paga, D2 decide o que acontece.

---

## Comportamentos esperados

### Chegada

- **Quando** um visitante deslogado acessa a plataforma pela primeira vez (direto ou via SEO), **o sistema** exibe a camada aberta: header, painel de mercado, barra de input central, sugestões de input, social proof. Conteúdo adaptado à localidade detectada (D3 — browser/IP).
- **Quando** o visitante navega pelo conteúdo público sem interagir com o input, **o sistema** permite navegação livre. Nenhum cadastro é solicitado. Sessão é stateless.

### Interação pré-login

- **Quando** o visitante digita um input na barra, **o sistema** processa a pergunta via agentes (D6), usando apenas dados abertos e skills públicas. Entrega preview parcial: estrutura + primeiros dados visíveis, restante em blur/truncado.
- **Quando** o preview parcial é renderizado, **o sistema** exibe o login wall imediatamente abaixo ou como overlay. Login social em destaque.
- **Quando** o visitante tenta fazer um segundo input sem se cadastrar, **o sistema** bloqueia com login wall. O produto permite apenas uma interação gratuita sem cadastro.

### Conversão (login wall)

- **Quando** o visitante clica em "Login com Google" ou "Login com Microsoft", **o sistema** autentica via OAuth, cria Profile e Workspace pessoal automaticamente (D1), revela o resultado completo sem repetir o input. Transição seamless.
- **Quando** o visitante escolhe e-mail/senha, **o sistema** exige nome, e-mail e senha. Após confirmação, mesmo fluxo: Profile + Workspace + resultado revelado.
- **Quando** o visitante já tem conta (login, não cadastro), **o sistema** autentica, vincula a sessão ao Profile existente, revela resultado completo. Se tem múltiplos Workspaces, entra no último ativo.

### Pós-conversão

- **Quando** o login é concluído com sucesso, **o sistema** transiciona da camada aberta para a experiência completa (E2 Chat). O resultado do input pré-login aparece como primeira mensagem da conversa. A barra de input vira o chat. Sidebar (E4) aparece.
- **Quando** o cadastro é concluído mas billing não está ativo, **o sistema** permite uso livre dentro da camada gratuita (D2). Skills e conectores free funcionam. O gate de billing aparece quando o usuário tenta ação paga.

---

## Casos de uso de referência

**Caso 1 — Gestor financeiro descobre via SEO**

Gestor de uma PME busca "análise de carteira de recebíveis" no Google. Encontra conteúdo indexado da Liquid com indicadores de mercado. Acessa a camada aberta. Vê painel com indicadores atualizados e sugestões de input como "Qual o score de liquidez da minha carteira?". Digita: "quero analisar minha carteira de recebíveis". O sistema processa com dados abertos e entrega preview: estrutura da análise com primeiros números visíveis, gráficos em blur. Login wall aparece. Ele faz login com Google em 2 cliques. Resultado completo revelado. Profile e Workspace pessoal criados. Ele está dentro do produto.

**Caso 2 — Investidor internacional chega por indicação**

Investidor recebe link de um colega. Acessa a plataforma. O sistema detecta localidade (EUA) e exibe conteúdo em inglês com valores em USD (D3). Vê indicadores de mercado brasileiro adaptados. Digita "what are the best receivables opportunities in Brazil?". Preview parcial entregue com tendências de mercado. Login wall aparece em inglês. Investidor faz login. Resultado completo revelado — agora com acesso ao funil de liquidez (D9).

**Caso 3 — Visitante curioso sem intenção imediata**

Visitante acessa a landing por curiosidade. Navega pelo painel de mercado, lê indicadores, explora conteúdo público. Não digita nenhum input. Nenhum login é solicitado. Sessão é stateless. O visitante sai e pode voltar depois — o conteúdo público está sempre disponível. Na próxima visita, digita um input. Aí sim o ciclo preview → login wall se ativa.

---

## Relações

### Recebe de (dependências)

- **D1 Identity →** Lógica de login wall, login social, criação automática de Profile/Workspace. Regras de interação pré-login stateless.
- **D2 Billing →** Definição da camada gratuita (o que é free vs. pago). Gate de billing pós-login.
- **D3 Localization →** Localidade do visitante (idioma, moeda, formatos). Determina conteúdo do painel de mercado e SEO.
- **D6 Agents →** Orchestrator processa o input do visitante. Mesmo motor de agentes, escopo restrito (sem conectores pagos, sem persistência).
- **D9 Liquidity →** Funil de 4 camadas. A landing é a camada "aberta". Indicadores de mercado como pipeline de liquidez.

### Envia para (transição pós-login)

- **E2 Chat →** Após login, a landing transiciona para o chat. O input pré-login vira a primeira mensagem da conversa.
- **E3 Settings →** Profile e Workspace criados na conversão ficam acessíveis em Settings.
- **E4 Sidebar →** Após login, sidebar aparece com navegação completa (eixos de trabalho, organizador, support, company).

---

## Restrições

1. NÃO persiste nada pré-login. Interação é stateless: sem salvar conversa, sem criar entidades, sem conectores pagos. Se o visitante sair sem fazer login, tudo desaparece. (D1 regra 11)
2. NÃO permite mais de um input sem cadastro. O primeiro input é gratuito; o segundo requer login. Isso evita abuso e cria urgência natural para conversão.
3. NÃO exibe dados sensíveis de outros usuários. O painel de mercado mostra apenas indicadores agregados e públicos. Nenhum dado individual é exposto.
4. NÃO é uma landing page estática com formulário de contato. É experiência funcional com o produto real. O visitante interage com agentes de verdade.
5. NÃO substitui a experiência completa. O preview parcial é deliberadamente limitado — mostra o suficiente para demonstrar valor, nunca o bastante para substituir o uso real.
6. V1 NÃO precisa de múltiplas páginas de conteúdo SEO. Painel de mercado + sugestões de input + barra de input é o mínimo viável. Conteúdo SEO cresce iterativamente.
7. NÃO controla billing. Apenas referencia: se o usuário converteu e tenta ação paga, o gate de billing (D2) decide o que acontece.
8. NÃO tem sidebar, menu lateral, nem navegação complexa. É uma tela única, focada. A complexidade aparece após o login.

---

## Decisões tomadas

| Decisão | Fonte | Data |
| --- | --- | --- |
| PLG: produto vende sozinho, vendas são secundárias | Inception PT1 (CEO) | 01/04 |
| Produto começa em inglês, bilíngue desde o dia 1 | Inception PT1 (CEO) | 01/04 |
| Funil de 4 camadas: aberto → conversão → fidelização → liquidez | Inception PT1 (CEO) | 01/04 |
| Agent First: toda experiência via orquestrador + agentes | Inception PT1 (CEO) | 01/04 |
| Telas não-core: copiar do mercado, não demorar | Inception PT2 (CEO) | 01/04 |
| ⚠️ Login wall após primeiro input — preview parcial demonstra valor | Proposta Luana — a validar | 02/04 |
| ⚠️ Interação pré-login é stateless — sem persistência, sem conectores pagos | Proposta Luana — a validar | 02/04 |
| ⚠️ Continuidade pós-login — resultado completo revelado sem repetir input | Proposta Luana — a validar | 02/04 |

---

## Pontos em aberto

| # | Pergunta | Responsável | Impacto |
| --- | --- | --- | --- |
| 1 | Preview parcial: mostra dados reais truncados ou dados fictícios/placeholder? Qual nível de informação? | Luana + Pedro | Taxa de conversão do login wall |
| 2 | Um input gratuito ou mais? Permitir múltiplas interações sem login pode demonstrar mais valor, mas permite uso sem conversão. | Pedro + Yaak | Trade-off PLG vs. conversão |
| 3 | Conteúdo SEO da V1: painel de mercado é suficiente? Precisa de páginas adicionais (artigos, glossário, análises públicas)? | Yaak + Rafael | Aquisição orgânica |
| 4 | Conteúdo da camada aberta precisa estar em PT-BR para mercado BR, mesmo com interface em inglês? (herdado de D3 #2) | Yaak + Rafael | SEO no Brasil vs. posicionamento global |
| 5 | O painel de mercado é em tempo real ou atualizado periodicamente? | Vinicius + Yaak | Escopo de engenharia e custo de dados |
| 6 | A barra de input da landing é a mesma do chat (E2) ou versão simplificada? | Luana | Design system e consistência |
| 7 | Transição visual landing → chat pós-login: animação contínua ou troca de página? | Luana | Percepção de continuidade (PLG) |
| 8 | Continuidade pós-login: viabilidade técnica de manter o processamento pré-login e revelar pós-login? (herdado de D1 #10) | Vinicius | Se não funcionar, a experiência quebra |
| 9 | Human-in-the-Loop: na camada aberta, o agente pode pedir aprovação? Ou HITL só pós-login? (Context Map 5.5.8) | Luana + Pedro | Define se preview pode incluir ações que requerem OK |

---

## Referências ao Context Map v3

Este contexto de experiência se conecta a múltiplas layers do Context Map:

- **Layer 4 — Design:** A Landing materializa os princípios de 4.1 (Cognitive Architecture — progressive disclosure, redução de carga cognitiva), 4.2 (UX Functional Architecture — view map, navigation), e 4.3 (Interaction Model — Human → Agent via input, Agent → Human via preview).
- **Layer 5.5.8 — Human-in-the-Loop:** Na camada aberta, o modelo é "out-of-the-loop" (o visitante não decide nada do agente — apenas recebe o preview). Pós-login, transiciona para "on-the-loop".
- **Layer 5.2 — Bounded Contexts:** A Landing toca Identity (login wall), Billing (gates), Growth (onboarding, activation), e Notifications (pós-login).
- **Layer T1.1 — Governance:** O preview parcial é uma decisão de governance: quanto mostrar sem autenticação em um regulated market.

---

## Glossário local

| Termo | Definição |
| --- | --- |
| **Camada aberta (Open Layer)** | Tudo que o visitante vê e faz antes de se autenticar. Primeira camada do funil de liquidez (D9). Produto real em modo restrito. |
| **Painel de mercado** | Bloco de indicadores, tendências e métricas públicas exibidos na landing. Conteúdo varia por localidade. Serve como SEO e demonstração de inteligência. |
| **Preview parcial** | Resultado limitado entregue ao visitante deslogado após o input. Mostra estrutura e primeiros dados; blur/trunca o restante. Demonstra valor sem substituir uso real. |
| **Login wall** | Momento em que o sistema solicita cadastro. Aparece após o primeiro input + preview parcial. Login social como caminho primário. |
| **Stateless** | Sem persistência. Nada do que acontece na camada aberta é salvo. Se o visitante sair sem login, tudo desaparece. |
| **Sugestões de input** | Prompts sugeridos que orientam o visitante sobre o que perguntar. Onboarding implícito — ensinam o que o produto faz sem explicar. |
| **Gate** | Ponto de controle que bloqueia ou permite uma ação. Na landing, o gate principal é autenticação (D1). Pós-login, o gate é billing (D2). |
| **Conversão** | Momento em que o visitante se torna usuário (cria conta). Segunda camada do funil de liquidez. |
| **Social proof** | Sinais visíveis de uso real da plataforma (volumes, métricas agregadas). Cria confiança sem expor dados individuais. |

---

# 🇺🇸 English

*Translation pending — will be added after PT version is reviewed and approved.*