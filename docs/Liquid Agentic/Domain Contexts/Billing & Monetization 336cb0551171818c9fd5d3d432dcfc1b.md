# Billing & Monetization

Category: Domain
Consumers: D4, D5, D9, E1, E2, E3
Dependencies: D1
ID: D2
Status: Review
Type: Generic

# 🇧🇷 Português

## Metadados

- **Domínio:** Generic Subdomain
- **ID:** D2
- **Owner de design:** Luana Siqueira
- **Owner técnico:** A definir
- **Dependências:** D1
- **Quem consome:** D4, D5, D9, E1, E2, E3

---

## Definição

Billing & Monetization é o domínio que controla como a Liquid cobra pelo uso do produto e como a receita é gerada. Administra planos, pagamentos, consumo e gates (o que é free vs. pago). Gerencia duas fontes de receita distintas: software (SaaS) e transacional (taxa sobre liquidez).

---

## Princípios aplicáveis

**PLG (Product-Led Growth) →** Billing DEVE nascer junto com a aplicação. Sem billing, o produto não é um produto. O usuário precisa conseguir assinar, pagar e fazer upgrade sem falar com um vendedor. A jornada do free ao pago deve ser self-service.

**Liquidity Driven →** A receita de SaaS sozinha não sustenta o produto. A receita principal vem de taxas sobre operações de liquidez (D9). O billing precisa acomodar essas duas fontes: recorrente (software) e variável (transacional).

**Global →** Billing DEVE suportar multi-moeda. Na V1 pode começar em BRL, mas a arquitetura não pode impedir expansão.

---

## Regras de negócio

1. Billing é por **Workspace**, não por usuário individual. O Admin do Workspace gerencia o billing.
2. O produto DEVE ter uma camada gratuita que permita o primeiro contato sem pagamento (princípio PLG). O limite da camada gratuita ainda é [EM ABERTO].
3. Existem duas fontes de receita que DEVEM coexistir: receita de software (assinatura/uso da plataforma) e receita transacional (taxa sobre operações de liquidez — D9).
4. Conectores que envolvem custo por consulta (ex: bureau de crédito) DEVEM ter consumo vinculado ao billing do Workspace. O sistema DEVE impedir consultas se o crédito for insuficiente.
5. O billing DEVE ser self-service: o Admin pode assinar, mudar de plano e cancelar sem intervenção humana.
6. O sistema DEVE registrar consumo detalhado: quantas consultas a conectores pagos, quantas tasks, qual volume de dados processado. Esse registro alimenta tanto a cobrança quanto o analytics interno (E3 Settings — Consumo).
7. A taxa de liquidez (receita transacional) PODE ter fluxo financeiro separado do billing de software, já que envolve valores significativamente maiores e pode exigir contrato específico.

---

## Fixo vs. Variável

**FIXO:**

- Estrutura de billing por Workspace
- Mecanismo de gates: verificar se o Workspace tem billing ativo antes de executar ações pagas
- Registro de consumo por ação
- Self-service para gestão de plano

**VARIÁVEL:**

- Quais planos existem e seus limites (podem mudar conforme estratégia comercial)
- Quais ações são gratuitas e quais são pagas (gates)
- Custo por consulta de cada conector (varia por provedor e localidade)
- Moeda de cobrança (depende da localidade do Workspace — D3)

**SLOTS:**

- Slot de método de pagamento: a plataforma aceita diferentes métodos conforme localidade (cartão, boleto, PIX no Brasil; wire transfer internacionalmente)
- Slot de provedor de billing: a integração com gateway de pagamento é substituível

---

## Comportamentos esperados

- **Quando** um Workspace é criado, **o sistema** o coloca na camada gratuita automaticamente. Nenhuma ação de billing é necessária para começar a usar.
- **Quando** o usuário atinge o limite da camada gratuita, **o sistema** informa que a ação requer upgrade e oferece caminho self-service para o Admin assinar.
- **Quando** um Member tenta acionar conector pago sem billing ativo, **o sistema** bloqueia a ação e orienta o Admin a configurar billing.
- **Quando** o Admin configura billing, **o sistema** solicita dados de pagamento e ativa o plano imediatamente.
- **Quando** uma operação de liquidez (D9) é concluída, **o sistema** registra a taxa transacional separadamente do consumo de software.
- **Quando** o Admin acessa a seção de billing em Settings (E3), **o sistema** exibe: plano atual, consumo do período, histórico de pagamentos, e opções de upgrade/downgrade.

---

## Casos de uso de referência

**Caso 1 — Camada gratuita para novo usuário** Usuário se cadastra via PLG. Workspace pessoal criado na camada gratuita. Ele pode usar skills que dependem apenas de dados abertos e do próprio contexto. Ao tentar acionar um conector pago pela primeira vez, o sistema informa que é necessário ativar billing. O Admin (ele mesmo, no workspace pessoal) configura pagamento self-service.

**Caso 2 — Organização com consumo variável** Uma empresa ativa billing no Workspace. Os Members consomem conectores pagos ao longo do mês. O Admin acompanha o consumo em Settings. No final do ciclo, a cobrança reflete: assinatura fixa + consumo variável de conectores.

**Caso 3 — Receita transacional** O sistema identifica um ativo transacionável (D9 Liquidity). A operação é concluída com intermediação da Liquid. A taxa sobre a transação é registrada como receita transacional — separada do billing de software, potencialmente com fluxo financeiro e contratual próprio.

---

## Relações

- **RECEBE** contexto de Workspace de → **D1 Identity** (billing é por workspace)
- **ENVIA** status de billing para → **D4 Connectors** (gate de conectores pagos)
- **ENVIA** status de billing para → **D5 Skills** (gate de skills premium, se existirem)
- **ENVIA** dados de receita transacional para → **D9 Liquidity**
- **APARECE EM** → **E3 Settings** (plano, consumo, pagamentos)
- **APARECE EM** → **E1 Landing** (gate free→pago)
- **APARECE EM** → **E2 Chat** (bloqueio inline quando billing é necessário)

---

## Restrições

- NÃO define permissões de acesso. Apenas controla se ações pagas podem ser executadas. Permissões vivem em D1.
- NÃO processa a operação financeira de liquidez. Apenas registra a taxa. A lógica da transação vive em D9.
- V1 NÃO precisa suportar múltiplas moedas de cobrança. Pode começar em BRL. A arquitetura DEVE permitir expansão.
- NÃO é responsável pela precificação de conectores. Cada conector informa seu custo; billing apenas cobra.

---

## Decisões tomadas

| Decisão | Fonte | Data |
| --- | --- | --- |
| Billing DEVE nascer junto com a aplicação | Inception PT1 (CEO) | 01/04 |
| SaaS sozinho não sustenta — receita transacional é a principal | Inception PT1 (CEO) | 01/04 |
| Estimativa: 5-20% dos usuários chegam à transação, mas essa receita é mais qualificada | Inception PT1 (CEO) | 01/04 |
| Billing está "mais perdido" — precisa de sessão dedicada | Inception PT2 | 01/04 |

---

## Pontos em aberto

| # | Pergunta | Responsável | Impacto |
| --- | --- | --- | --- |
| 1 | Qual o modelo? Freemium + tiers? Pay-as-you-go? Créditos? | Yaak (sessão dedicada) | Define toda a lógica de gates e onboarding |
| 2 | O que é gratuito e o que é pago? Onde fica o limite da camada free? | Yaak + Pedro | Impacta conversão PLG diretamente |
| 3 | A cobrança é por seat (usuário) ou flat por Workspace? | Yaak | Impacta pricing e modelo multiplayer |
| 4 | A taxa de liquidez entra no mesmo painel de billing ou é fluxo financeiro separado? | Yaak | Impacta design de Settings e contabilidade |
| 5 | Multi-moeda: precisa suportar desde V1 ou basta arquitetura preparada? | Yaak | Impacta escopo de engenharia |
| 6 | Skills premium existem na V1? Ou todas as skills são acessíveis com billing ativo? | Pedro + Yaak | Impacta gates e catálogo de skills |

---

## Glossário local

| Termo | Definição |
| --- | --- |
| **Gate** | Ponto de controle que verifica se a ação pode ser executada com base no billing. Free vs. pago. |
| **Camada gratuita** | Nível de uso sem pagamento. Limite a definir. |
| **Receita de software (SaaS)** | Receita recorrente por assinatura/uso da plataforma. |
| **Receita transacional** | Taxa cobrada sobre operações de liquidez intermediadas pela Liquid. Receita principal do negócio. |
| **Consumo** | Registro de ações pagas executadas no período: consultas a conectores, volume processado, etc. |

---

# 🇺🇸 English

*Translation pending — will be added after PT version is reviewed and approved.*