# Platform Operations

Category: Domain
Consumers: —
Dependencies: D4, D5, D9
ID: D10
Status: Review
Type: Operating

# 🇧🇷 Português

## Metadados

- **Domínio:** Operating Subdomain
- **ID:** D10
- **Dependências:** D4, D5, D9

---

## Definição

Platform Operations é o domínio interno da Liquid: ferramentas, processos e métricas que permitem ao time operar, expandir e monitorar o produto. Inclui o admin interno, o processo de criação contínua de novas skills e conectores, e o dashboard operacional do pipeline de liquidez. **O usuário final não interage com este domínio.**

---

## Princípios aplicáveis

**Agent First →** A prioridade não é criar cada skill/conector manualmente, mas sim criar o **processo** que permite que agentes internos façam deep research, identifiquem necessidades e desenvolvam novos módulos continuamente.

**Liquidity Driven →** O admin interno é a torre de controle do pipeline de liquidez, onde o time de crédito e transação vê oportunidades qualificadas pelo Liquidity Agent (D6/D9).

---

## Regras de negócio

1. O admin interno NÃO é acessível pelo usuário final. É ferramenta do time Liquid.
2. O sistema DEVE suportar criação de novos conectores e skills sem deploy (requisito compartilhado com D4 e D5).
3. O admin DEVE exibir métricas de uso: skills mais usadas, conectores com mais falhas, volume de consultas, pipeline de liquidez.
4. O processo de expansão (novas skills/conectores) DEVE ser o mais autônomo possível — idealmente com agentes internos que fazem deep research e propõem novos módulos.
5. Oportunidades qualificadas pelo Liquidity Agent DEVEM ser visíveis no admin para o time operacional.

---

## Fixo vs. Variável

**FIXO:**

- Painel de métricas de uso
- Pipeline de liquidez para o time operacional
- Mecanismo de publicação de novos conectores/skills

**VARIÁVEL:**

- Quais métricas são exibidas (evoluem conforme o produto cresce)
- Quais conectores/skills estão sendo desenvolvidos
- Critérios de priorização de novas expansões

---

## Comportamentos esperados

- **Quando** o time publica um novo conector, **o sistema** o disponibiliza para Workspaces com localidade compatível automaticamente.
- **Quando** o Liquidity Agent qualifica uma oportunidade, **o sistema** a exibe no pipeline do admin para o time operacional.
- **Quando** um conector apresenta taxa de erro alta, **o sistema** alerta o time no admin.

---

## Relações

- **GERENCIA** → **D4 Connectors** (criação e manutenção de conectores)
- **GERENCIA** → **D5 Skills** (criação e manutenção de skills Liquid)
- **RECEBE** pipeline de → **D9 Liquidity** (oportunidades qualificadas)
- **NÃO APARECE** em nenhum Contexto de Experiência do usuário (E1-E4)

---

## Restrições

- NÃO é escopo de design para o usuário final.
- NÃO é prioridade de design na V1 (pode ser dashboards internos simples).
- NÃO substitui a equipe. Automatiza e facilita, mas humanos decidem.

---

## Decisões tomadas

| Decisão | Fonte | Data |
| --- | --- | --- |
| Prioridade: criar o processo autônomo de adicionar skills e conectores | Inception PT1 (CEO) | 01/04 |
| A função do time não é criar cada conector individualmente, mas o processo | Inception PT1 (CEO) | 01/04 |
| Admin não interage diretamente com a aplicação principal | Inception PT2 | 01/04 |

---

## Pontos em aberto

| # | Pergunta | Responsável | Impacto |
| --- | --- | --- | --- |
| 1 | O admin interno precisa de design na V1 ou pode ser dashboard técnico simples? | Pedro | Priorização de esforço de design |
| 2 | Qual o grau de autonomia dos agentes internos na V1? Podem publicar conectores/skills sem revisão humana? | Yaak + Vinicius | Governança e qualidade |

---

## Glossário local

| Termo | Definição |
| --- | --- |
| **Admin Liquid** | Painel interno do time. Métricas, gestão de módulos, pipeline de liquidez. |
| **Processo de expansão** | Mecanismo (idealmente autônomo) de identificar, desenvolver e publicar novos conectores e skills. |
| **Pipeline operacional** | Fila de oportunidades de liquidez qualificadas, visíveis para o time de crédito/transação. |

---

# 🇺🇸 English

*Translation pending*