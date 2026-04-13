# Liquidity

Category: Domain
Consumers: E1, E2
Dependencies: D4, D5, D6, D8
ID: D9
Status: Review
Type: Operating

# 🇧🇷 Português

## Metadados

- **Domínio:** Operating Subdomain
- **ID:** D9
- **Owner de design:** A definir
- **Owner técnico:** A definir
- **Dependências:** D4, D5, D6, D8
- **Quem consome:** E1, E2

---

## Definição

Liquidity é o domínio que representa a tese central do negócio: converter uso do produto em receita transacional. Define o conceito de liquidez, o funil de conversão (aberto → conversão → fidelização → liquidez), o score de qualificação de ativos, e as operações de transação intermediadas pela Liquid.

---

## Princípios aplicáveis

**Liquidity Driven →** Princípio central. Toda ação no produto é pipeline para liquidez. A receita transacional é a principal fonte de receita, mesmo vindo de uma minoria dos usuários (5-20%). Liquidez = qualquer operação em que a Liquid cobra taxa sobre o valor transacionado.

---

## Regras de negócio

1. O funil de produto tem 4 camadas: **camada aberta** (atrair), **conversão** (usuário), **fidelização** (dados sensíveis), **liquidez** (transação).
2. A liquidez é atingida quando o sistema identifica que um ativo é transacionável E o cliente é elegível. Nesse momento, o Liquidity Agent (D6) qualifica o cliente no pipeline.
3. O score de liquidez avalia ativos em múltiplas dimensões (qualidade do ativo, fluxo de pagamentos, investidores potenciais, etc.).
4. Quando o score é alto e critérios de elegibilidade são atendidos, o agente oferece intermediação, o chat "muda de tom" de informativo para operacional.
5. Após a qualificação, o ativo se torna um "ativo embalado", compilado, quase imutável, com integridade garantida pelas entidades (D8).
6. A fidelização se manifesta quando o cliente traz dados sensíveis para a plataforma, estabelecendo confiança para uso recorrente.
7. A taxa de liquidez é registrada em D2 Billing como receita transacional.

---

## Fixo vs. Variável

**FIXO:**

- Funil de 4 camadas como estrutura do produto
- Mecanismo de qualificação via Liquidity Agent (D6)
- Transição de informativo → operacional na conversa
- Registro de taxa transacional em D2

**VARIÁVEL:**

- Quais dimensões compõem o score de liquidez (podem evoluir)
- Quais critérios de elegibilidade a Liquid define (mudam conforme estratégia)
- Quais tipos de ativo são transacionáveis (expandível por mercado)
- Quais operações de liquidez existem (venda de carteira, intermediação de crédito, etc.)

---

## Comportamentos esperados

- **Quando** o Liquidity Agent identifica uma oportunidade qualificável durante a interação, **o sistema** sinaliza ao Orchestrator (D6) que a conversa tem potencial de liquidez.
- **Quando** o score de liquidez atinge o threshold e os critérios de elegibilidade são atendidos, **o sistema** muda a natureza da conversa, de análise informativa para proposta operacional.
- **Quando** uma operação de liquidez é concluída, **o sistema** registra a taxa transacional em D2 Billing.

---

## Casos de uso de referência

**Caso 1 — Funil completo** Usuário chega pela camada aberta (E1). Cria conta (conversão). Sobe dados de negócio para o KB — D7 (fidelização). O Liquidity Agent avalia os dados em background, identifica que o ativo atende critérios. O agente oferece intermediação. Se o usuário aceita, a operação entra no pipeline e gera receita transacional.

**Caso 2 — Liquidez identificada em análise rotineira** Usuário usa uma skill de análise. O resultado é um artefato com métricas. Em background, o Liquidity Agent avalia o score. O ativo é qualificável. O agente complementa a resposta com: "este ativo tem potencial de transação, quer que eu aprofunde?"

---

## Relações

- **RECEBE** dados de → **D4 Connectors**, **D5 Skills**, **D8 Entities** (insumos para scoring)
- **RECEBE** identificação de oportunidade de → **D6 Agents** (Liquidity Agent)
- **ENVIA** receita transacional para → **D2 Billing**
- **APARECE EM** → **E1 Landing** (topo de funil), **E2 Chat** (transição informativo→operacional)

---

## Restrições

- NÃO é uma funcionalidade que o usuário "acessa". É uma lógica que emerge do uso do produto.
- V1 pode ter o funil conceitual completo sem todas as operações de liquidez implementadas. O mínimo é: identificar oportunidade + qualificar no pipeline.
- NÃO substitui o time operacional da Liquid. O sistema qualifica; o time de crédito/transação da Liquid pode atuar na execução.

---

## Decisões tomadas

| Decisão | Fonte | Data |
| --- | --- | --- |
| Funil de 4 camadas: aberto → conversão → fidelização → liquidez | Inception PT1 (CEO) | 01/04 |
| Liquidez = qualquer operação com taxa sobre valor transacionado | Inception PT1 (CEO) | 01/04 |
| 5-20% chegam à transação, mas receita é mais qualificada | Inception PT1 (CEO) | 01/04 |
| Fidelização = quando o cliente traz dados sensíveis | Inception PT2 (CEO) | 01/04 |
| Ativo qualificado vira "ativo embalado" — compilado e quase imutável | Inception PT1 (CEO) | 01/04 |

---

## Pontos em aberto

| # | Pergunta | Responsável | Impacto |
| --- | --- | --- | --- |
| 1 | Quais são as dimensões do score de liquidez na V1? | Yaak | Define o que o Liquidity Agent avalia |
| 2 | A transição informativo→operacional no chat: como sinalizar sem ser invasivo? | Luana | Experiência e confiança do usuário |
| 3 | Quais operações de liquidez a V1 suporta? Todas ou apenas qualificação? | Yaak + Pedro | Escopo de engenharia e time operacional |

---

## Glossário local

| Termo | Definição |
| --- | --- |
| **Liquidez / Liquidity** | Operação em que a Liquid cobra taxa sobre valor transacionado. Receita principal. |
| **Score de liquidez** | Avaliação multidimensional de um ativo para determinar se é transacionável. |
| **Ativo embalado** | Ativo qualificado, compilado e com integridade garantida, pronto para transação. |
| **Pipeline de liquidez** | Fluxo onde clientes qualificados são direcionados para operações de transação. |

---

# 🇺🇸 English

*Translation pending.*

---

# 🇺🇸 English

*Translation pending*