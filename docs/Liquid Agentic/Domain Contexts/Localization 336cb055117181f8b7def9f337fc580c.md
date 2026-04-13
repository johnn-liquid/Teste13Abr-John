# Localization

Category: Domain
Consumers: D4, D5, D8, E1, E3
Dependencies: —
ID: D3
Status: Review
Type: Generic

# 🇧🇷 Português

## Metadados

- **Domínio:** Generic Subdomain
- **ID:** D3
- **Owner de design:** A definir (Yaak no FigJam)
- **Owner técnico:** A definir
- **Dependências:** —
- **Quem consome:** D4, D5, D8, E1, E3

---

## Definição

Localization é o domínio que gerencia regionalização: idioma da interface, moeda de exibição, localidade que determina formatos (data, número, endereço), e regras de compliance por país. É o que torna o produto Global sem reconstruí-lo para cada mercado.

---

## Princípios aplicáveis

**Global →** Princípio central deste domínio. Multi-idioma e multi-moeda desde o dia 1. Produto começa em inglês, bilíngue por natureza. A localidade impacta quais conectores e skills são relevantes.

**Regulated Market →** Operar em outro país pode exigir instâncias locais de armazenamento de dados. A localidade é o gatilho para regras de compliance específicas.

---

## Regras de negócio

1. O produto DEVE começar em inglês. Suporte bilíngue (PT-BR + EN) desde a V1.
2. A localidade opera em dois níveis: **Workspace** (localidade padrão da organização) e **Profile** (preferência individual do usuário). O Profile pode sobrescrever o idioma da interface, mas formatos de dados financeiros seguem o Workspace.
3. A localidade do Workspace DEVE determinar quais conectores (D4) e skills (D5) são exibidos como disponíveis. Um conector de bureau brasileiro não aparece para um Workspace mexicano.
4. Formatos de moeda, data e número DEVEM se adaptar à localidade: R$1.000,00 no Brasil; $1,000.00 nos EUA; etc.
5. Labels, placeholders e mensagens do sistema DEVEM ser externalizáveis (i18n). Strings hardcoded NÃO são aceitáveis.
6. O conteúdo da camada aberta (E1 Landing — painel de mercado) DEVE ser sensível à localidade do visitante para fins de SEO e relevância.

---

## Fixo vs. Variável

**FIXO:**

- Sistema de i18n: toda string da interface é externalizável
- Dois níveis de localidade: Workspace (dados) e Profile (interface)
- Mecanismo de filtragem de conectores/skills por localidade

**VARIÁVEL:**

- Quais idiomas estão disponíveis (cresce ao longo do tempo)
- Quais localidades são suportadas (cada uma traz formatos, compliance e catálogo de conectores/skills próprios)
- Quais dados regulatórios são exigidos por localidade (slot de compliance no Profile — D1)
- Conteúdo de mercado da camada aberta muda por localidade

---

## Comportamentos esperados

- **Quando** o Workspace é criado, **o sistema** detecta a localidade do Admin (via browser ou IP) e sugere como padrão. O Admin pode alterar.
- **Quando** o Profile tem idioma diferente do Workspace, **o sistema** exibe a interface no idioma do Profile, mas dados financeiros (moeda, formatos) seguem a localidade do Workspace.
- **Quando** a localidade do Workspace é alterada, **o sistema** atualiza a lista de conectores e skills disponíveis.
- **Quando** um visitante deslogado acessa a camada aberta, **o sistema** detecta a localidade provável e adapta o conteúdo de mercado.

---

## Casos de uso de referência

**Caso 1 — Workspace brasileiro, membro internacional** Empresa brasileira cria Workspace com localidade BR. Um consultor baseado nos EUA é convidado. Ele configura seu Profile para inglês. Vê a interface em inglês, mas valores financeiros aparecem em BRL (moeda do Workspace). Conectores disponíveis são os brasileiros (Serasa, dados governamentais BR).

**Caso 2 — Expansão para novo mercado** A Liquid decide operar no México. Nova localidade "MX" é criada com: idioma padrão espanhol, moeda MXN, formatos de data/número mexicanos, e catálogo de conectores/skills específicos. Workspaces com localidade MX passam a ver esses recursos automaticamente.

---

## Relações

- **ENVIA** filtro de localidade para → **D4 Connectors** (quais conectores exibir)
- **ENVIA** filtro de localidade para → **D5 Skills** (quais skills são aplicáveis)
- **ENVIA** requisitos de compliance para → **D1 Identity** (slot de dados regulatórios no Profile)
- **ENVIA** moeda e formato para → **D2 Billing** (moeda de cobrança)
- **APARECE EM** → **E3 Settings** (configuração de idioma e localidade)
- **APARECE EM** → **E1 Landing** (adaptação de conteúdo por localidade)

---

## Restrições

- NÃO traduz conteúdo gerado por agentes. A localidade controla a interface, não o output dos agentes (que depende da skill e do idioma do input do usuário).
- V1 suporta dois idiomas: EN e PT-BR. Outros idiomas são escopo futuro, mas a arquitetura i18n DEVE estar pronta.
- NÃO define regras de compliance específicas por país. Apenas sinaliza quais campos/regras se aplicam conforme a localidade.

---

## Decisões tomadas

| Decisão | Fonte | Data |
| --- | --- | --- |
| Produto começa em inglês | Inception PT1 (CEO) | 01/04 |
| Bilíngue por natureza desde o início | Inception PT1 (CEO) | 01/04 |
| Em caso de internacionalização, pode ser necessário instâncias locais de armazenamento | Inception PT1 (CEO) | 01/04 |

---

## Pontos em aberto

| # | Pergunta | Responsável | Impacto |
| --- | --- | --- | --- |
| 1 | Quem define a localidade padrão do Workspace: detecção automática ou seleção manual? | Luana | Experiência de onboarding |
| 2 | Conteúdo da camada aberta (SEO) precisa estar em português para o mercado BR desde V1, mesmo com interface em inglês? | Yaak + Rafael | Estratégia de go-to-market |
| 3 | Instâncias locais de armazenamento: quando isso se torna necessário e quem decide? | Yaak + Vinicius | Arquitetura de infraestrutura |

---

## Glossário local

| Termo | Definição |
| --- | --- |
| **Localidade** | Combinação de país + idioma + moeda + formatos que define o contexto regional do Workspace ou Profile. |
| **i18n** | Internacionalização — prática de externalizar todas as strings da interface para suportar múltiplos idiomas. |
| **Compliance por localidade** | Regras regulatórias que variam por país e impactam quais dados são exigidos e como são armazenados. |

---

# 🇺🇸 English

*Translation pending*