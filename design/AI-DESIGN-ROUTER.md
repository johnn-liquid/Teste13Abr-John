# UI/UX & Design System Wiki — AI Router
_Este documento é o índice primário para ingestão de contexto de interface e regras de interação pelos Agentes de Inteligência Artificial. Sempre que tomar uma decisão de design ou UI, inicie o mapeamento do conhecimento por aqui._

Os arquivos dessa wiki estão particionados usando o conceito de domínios (progressive disclosure) para evitar o enchimento exagerado da janela de contexto. **Apenas carregue a subpasta referente à natureza específica da sua tarefa de UI/UX.**

## Mapa de Domínios do Conhecimento
Leia abaixo qual pasta atende ao problema que você quer resolver:

### 1- Filosofia & Contexto -> `/context/`
Acesse os arquivos em `/design/context/` quando precisar interagir com a arquitetura subjetiva e o propósito de certas escolhas de interface, visão executiva ou comportamento do fluxo.
- `01-cognitive-architecture.md`
- `02-architectural-decisions.md`
- `03-experience-context.md`
- `opt-base-ui-and-copy.md` (Tópico otimizado)
- `opt-entities-states.md` (Tópico otimizado)

### 2- Padrões de Interface & Workflows -> `/patterns/`
Acesse os arquivos em `/design/patterns/` quando você estiver arquitetando um novo fluxo, formulando microcopy ou criando um layout (como um Dashboard), pois aqui residem as "combinações prontas de sucesso".
- `01-functional-architecture.md`
- `02-interaction-models.md`
- `03-dashboard-generation.md`
- `04-microcopy-writing.md`
- `opt-ux-layouts-and-pages.md` (Tópico otimizado)
- `opt-agent-interaction-flow.md` (Tópico otimizado)
- `opt-dashboard-generation.md` (Tópico otimizado)

### 3- Engenharia Visual & Front-end -> `/design-system/`
Acesse a pasta `/design/design-system/` unicamente quando for instanciado para **gerar marcação, interagir com tokens, estilos Tailwind** e implementar fisicamente os componentes em código, pois esse é o seu guia rigoroso de pragmatismo técnico.
- `01-behavioral-rules.md` (A fusão atômica de código para a UI Base)
- `rulebook.md` (O manual duro de implementação)
- `AI-INSTRUCTIONS.md` (Instruções detalhadas de CSS e ShadCN)
- Diretórios técnicos em `/components/` e `/tokens/`

---
> **Instrução Especial ao Modelo (LLM):** 
> 1. Não importe múltiplos domínios para o contexto ao mesmo tempo, exceto se a tarefa for uma mescla complexa entre (por exemplo) escrever a regra comportamental (patterns) e também desenvolver a tela CSS (design-system).
> 2. Se encontrar o prefixo `opt-` nos arquivos de domínio, saiba que essa é a versão comprimida do autor original, com as diretivas mais velozes para aplicar o layout com o menor impacto de token context. Se houver informações faltantes, verifique as contrapartes regulares (`01-`, `02-`).
