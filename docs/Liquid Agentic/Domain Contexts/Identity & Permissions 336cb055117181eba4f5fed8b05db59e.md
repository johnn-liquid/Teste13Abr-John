# Identity & Permissions

Category: Domain
Consumers: D2, D4, D5, D6, D7, D8, E2, E3, E4
Dependencies: —
ID: D1
Status: Review
Type: Generic

# 🇧🇷 Português

## Metadados

- **Domínio:** Generic Subdomain
- **ID:** D1
- **Owner técnico:** A definir
- **Dependências:** —
- **Quem consome:** D2, D4, D5, D6, D7, D8, E2, E3, E4

---

## Definição

Identity & Permissions é o domínio que organiza a estrutura de pessoas e acesso da plataforma. Resolve quatro perguntas: quem é o usuário (**Profile**), em qual organização ele atua (**Workspace**), em qual contexto de trabalho ele está (**Project**), e o que ele pode ver e fazer (**Roles**).

A hierarquia é: **Workspace → Projects → Tasks/Conversations/Artifacts**. Fora de Projects, o usuário tem seu espaço pessoal privado. Conteúdo pode ser movido entre o espaço pessoal e um Project — mover para dentro torna visível aos membros; criar fora mantém privado.

---

## Princípios aplicáveis

**PLG (Product-Led Growth) →** O login NÃO é o primeiro passo. O usuário interage com a camada aberta antes de se cadastrar. O login wall aparece após o primeiro input, quando o sistema já demonstrou valor via preview parcial. Cadastro com barreira mínima: login social como caminho primário.

**Multiplayer →** Cada pessoa tem identidade, papel e histórico próprio. Um Project compartilha contexto de trabalho entre seus membros automaticamente.

**Regulated Market →** Toda ação DEVE ser rastreável até um usuário identificado. Interação pré-login é stateless: sem persistência, sem conectores pagos, sem criação de entidades.

**Global →** Profile suporta preferências individuais de idioma e localidade, independente da configuração do Workspace.

---

## Regras de negócio

1. Todo usuário DEVE ter um Profile individual. Contas compartilhadas NÃO são suportadas.
2. Login social (Google, Microsoft) DEVE ser o método primário. E-mail/senha é fallback.
3. Todo usuário existe dentro de pelo menos um Workspace. Cadastro sem convite cria um Workspace pessoal automaticamente.
4. Um usuário PODE pertencer a múltiplos Workspaces. Profile é global; Workspaces são contextos.
5. Permissões são por **escopo individual no nível do objeto** (estilo Figma).
6. **Projects** são containers de trabalho compartilhado dentro de um Workspace. Tudo criado dentro de um Project é automaticamente visível para seus membros. Se o usuário quer algo privado, cria fora do Project — e pode mover para dentro depois.
7. O Workspace DEVE ter pelo menos um Admin.
8. Na V1, roles são fixos: **Admin** (acesso total ao Workspace) e **Member** (acessa tudo que foi compartilhado ou está nos Projects dos quais participa). Todo Member acessa todas as skills e conectores disponíveis — o gate é billing (D2), não permissão individual.
9. O sistema DEVE registrar log de ações por usuário para auditoria.
10. O login wall aparece após o primeiro input do usuário deslogado. O sistema entrega um preview parcial do resultado (suficiente para demonstrar valor, insuficiente para substituir o uso real) e solicita login para acesso completo. Após login, o resultado completo é revelado sem repetir o input.
11. Interação pré-login é stateless: sem persistência, sem conectores pagos, sem criação de entidades.

---

## Fixo vs. Variável

**FIXO:**

- Ciclo de vida: visita anônima → primeiro input → preview → login wall → cadastro → workspace → projects → ações rastreáveis
- Hierarquia: Workspace → Projects → Content. Espaço pessoal fora de Projects.
- Campos obrigatórios do Profile: nome, e-mail, método de autenticação, idioma
- Roles V1: Admin e Member (fixos, sem customização)
- Rastreabilidade: toda ação registra quem, quando, o quê
- Interação pré-login: stateless, sem custos, sem persistência

**VARIÁVEL:**

- Quais dados adicionais o Profile exige dependem da localidade e compliance (D3)
- Quais Projects existem, quem participa, e qual conteúdo vive dentro de cada um
- Conteúdo pode ser movido entre espaço pessoal e Projects, mudando visibilidade

**SLOTS:**

- Slot de dados de compliance: campo extensível no Profile onde dados regulatórios podem ser exigidos conforme a localidade (D3 determina quais)

---

## Comportamentos esperados

- **Quando** o usuário acessa a plataforma deslogado, **o sistema** exibe a camada aberta (E1): conteúdo público, painel de mercado, sugestões de input.
- **Quando** o usuário deslogado digita um input, **o sistema** processa, entrega preview parcial, e solicita login para resultado completo.
- **Quando** o usuário completa o cadastro via login wall, **o sistema** revela o resultado completo sem repetir o input. Cria Profile e Workspace pessoal automaticamente.
- **Quando** o usuário recebe convite para um Workspace, **o sistema** o associa com a role definida pelo Admin. Se não tem conta, cadastro é inline.
- **Quando** o Admin cria um Project, **o sistema** permite adicionar membros do Workspace. Tudo criado dentro é visível para esses membros.
- **Quando** o usuário cria conteúdo fora de qualquer Project, **o sistema** o mantém privado. O usuário pode mover para um Project depois.
- **Quando** o usuário pertence a múltiplos Workspaces, **o sistema** oferece Account Switch para trocar de contexto sem logout.
- **Quando** o Admin remove um membro, **o sistema** desvincula mas NÃO apaga ações já registradas. Conteúdo criado permanece conforme configuração de visibilidade.
- **Quando** um Member tenta acionar conector pago sem billing ativo, **o sistema** informa que a ação requer billing e orienta o Admin.

---

## Casos de uso de referência

**Caso 1 — Usuário descobre a plataforma via SEO** Usuário encontra conteúdo da Liquid em busca orgânica. Acessa a camada aberta. Vê sugestões de input relacionadas ao seu contexto. Digita uma pergunta. O sistema processa e entrega preview parcial. O usuário percebe valor e faz login com Google. Resultado completo aparece. Profile e Workspace pessoal são criados automaticamente. Ele está dentro do produto sem ter preenchido nenhum formulário.

**Caso 2 — Organização com Projects** Um Admin cria o Workspace da empresa e convida os membros do time. Cria um Project para um contexto de trabalho específico e adiciona os membros relevantes. Todas as tasks, conversas e artefatos criados dentro do Project são visíveis para o grupo. Um membro faz uma análise exploratória no espaço pessoal. Quando o resultado é útil, move para dentro do Project — os demais membros passam a ver automaticamente. Outro membro que não participa desse Project não tem acesso.

**Caso 3 — Identidade + billing como gate** Um Member aciona uma skill que requer conector pago. O sistema verifica: (a) é membro autenticado do Workspace? (D1), (b) billing ativo com crédito? (D2). Se ok, executa e registra: quem, quando, quais dados. Na V1, todo Member autenticado acessa qualquer skill — o gate é billing.

---

## Relações

- **ENVIA** identidade e role para → **todos os domínios** (toda ação carrega quem a executou)
- **ENVIA** identidade e role para → **D4 Connectors**, **D5 Skills**, **D7 KB** (autenticação Admin/Member)
- **ENVIA** contexto de workspace e project para → **D2 Billing** (cobrança por workspace)
- **ENVIA** preferência de idioma para → **D3 Localization**
- **APARECE EM** → **E1 Landing** (login wall após primeiro input)
- **APARECE EM** → **E3 Settings** (Profile, Users, Roles)
- **APARECE EM** → **E4 Sidebar** (Account Switch, Projects)
- **APARECE EM** → **E2 Chat** (identificação de quem conversa)

---

## Restrições

- NÃO define modelo de billing. Apenas verifica se o Workspace tem billing ativo. Lógica de planos e cobrança vive em D2.
- NÃO gerencia dados de negócio (carteiras, entidades). Apenas identifica o ator. Dados de negócio vivem em D7 e D8.
- NÃO suporta login anônimo para interação real com agentes. Camada aberta permite apenas preview.
- V1 NÃO tem permissões granulares por skill/conector. O gate é billing, não role.
- V1 NÃO tem roles customizados. Apenas Admin e Member.

---

## Decisões tomadas

| Decisão | Fonte | Data |
| --- | --- | --- |
| Permissão por escopo individual no nível do objeto (estilo Figma) | Inception PT2 (CEO) | 01/04 |
| Sem extrema granularidade de permissões na V1 | Inception PT2 (CEO) | 01/04 |
| Company/Workspace modelo Figma/Notion | Inception PT2 (CEO) | 01/04 |
| Profile e localidade parcialmente prontos do produto anterior | Inception PT2 | 01/04 |
| Profile/Users/Roles podem ser em modal no Settings | Inception PT2 | 01/04 |
| Projects substituem grupos como estrutura de compartilhamento | Proposta Luana — a validar | 02/04 |
| Conteúdo criado dentro de Project é visível; fora é privado; movível | Proposta Luana — a validar | 02/04 |
| Login wall após primeiro input, com preview parcial | Proposta Luana — a validar | 02/04 |
| Interação pré-login é stateless | Proposta Luana — a validar | 02/04 |

---

## Pontos em aberto

| # | Pergunta | Responsável | Impacto |
| --- | --- | --- | --- |
| 1 | Workspace fica junto ao Profile ou nível acima na hierarquia? | Luana + Yaak | Estrutura de Settings e Account Switch |
| 2 | Roles V1: apenas Admin e Member, ou existe Viewer? | Pedro + Yaak | Design de permissões e convites |
| 3 | Convite para Workspace: e-mail, link, ou ambos? | Luana (propor) | Onboarding multiplayer |
| 4 | Quem pode criar Projects? Admin ou qualquer Member? | Pedro + Yaak | Autonomia do usuário |
| 5 | KB é por Workspace, por Project, ou ambos? PT2 decidiu "pasta global única", mas Projects criam nível intermediário natural. | Yaak + Luana | Onde os dados vivem e quem vê |
| 6 | **Multiplayer em conversas (dúvida do Pedro):** Duas modalidades possíveis: (A) uma pessoa pode apenas **visualizar** uma conversa iniciada por outra (read-only — ex: liderança analisa atendimento/alçadas), ou (B) uma pessoa pode **entrar e continuar** uma conversa iniciada por outra (colaborativo — ex: dois analistas trabalhando no mesmo caso, ambos veem atualizações em tempo real). Pode ser que ambas existam para cenários diferentes. | Pedro + Yaak (Yaak sinalizou "bom ponto") | Define o modelo de colaboração do produto inteiro. Impacta D5 Skills, D6 Agents, E2 Chat |
| 7 | Dados obrigatórios no cadastro: só nome e e-mail, ou compliance exige mais? | Yaak + jurídico | PLG vs. Regulated Market |
| 8 | Preview parcial pré-login: qual nível de informação entrega? | Luana + Pedro | Taxa de conversão do login wall |
| 9 | Ao mover conteúdo entre Projects, histórico e contexto vinculado acompanham? | Luana + Vinicius | Experiência de reorganização |
| 10 | Continuidade pós-login (retomar pergunta pré-login): viabilidade técnica? | Vinicius | Se não funcionar, a experiência quebra |

---

## Glossário local

| Termo | Definição |
| --- | --- |
| **Profile** | Identidade individual: nome, e-mail, foto, idioma. Global — não pertence a um Workspace específico. |
| **Workspace** | Espaço organizacional. Um usuário pode ter múltiplos. |
| **Project** | Container de trabalho compartilhado dentro de um Workspace. Tudo dentro é visível para seus membros. Modelo Figma/Claude. |
| **Admin** | Role com acesso total ao Workspace: membros, billing, configurações. |
| **Member** | Role padrão. Acessa tudo nos Projects de que participa. Na V1, acessa todas as skills/conectores. |
| **Account Switch** | Troca entre Workspaces sem logout. |
| **Login wall** | Momento em que o sistema solicita cadastro. Aparece após o primeiro input, depois de preview parcial. |
| **Preview parcial** | Resultado limitado entregue ao usuário deslogado. Demonstra valor sem entregar resultado completo. |
| **Stateless** | Interação sem persistência. Nada do que acontece pré-login é salvo permanentemente. |

---

# 🇺🇸 English

*Translation pending — will be added after PT version is reviewed and approved.*