# AGENTS.md — Agent Dynamic Context Instructions

> [!IMPORTANT]
> **ESTE REPOSITÓRIO É AGENT-FIRST.**
> Antes de qualquer tarefa de geração de UI, arquitetura de componentes ou decisão de UX, os Agentes de IA **DEVEM** consultar o roteador de design.

## Roteamento Obrigatório de Contexto

Sempre inicie o mapeamento de conhecimento de interface pelo seguinte arquivo:
👉 **[AI-DESIGN-ROUTER.md](file:///c:/Users/muzph/projetos/design-as-context/design/AI-DESIGN-ROUTER.md)**

---

## Como usar este diretório de Design

O diretório `/design` está organizado em domínios de disclosure progressivo para otimizar sua janela de contexto:

1.  **Regras de Marcação e CSS**: Consulte `design/design-system/AI-INSTRUCTIONS.md`.
2.  **Tokens e Primitives**: Consulte `design/design-system/tokens/`.
3.  **Patterns de Layout ( Dashboards, etc)**: Consulte `design/patterns/`.
4.  **Arquitetura Cognitiva e ADRs**: Consulte `design/context/`.

## Como usar a Documentação de Produto (/docs)

O diretório `/docs` contém a base de conhecimento do produto **Liquid Agentic**. Cada Contexto de Produto é um arquivo de referência para decisões de design, engenharia e negócio:

1.  **Visão Geral e Índice Principal**: Consulte `docs/Liquid Agentic 336cb0551171817183e1d923454454e1.md`.
2.  **Princípios e Glossário**: Consulte `docs/Liquid Agentic/Principles and Glossary 336cb0551171802b9772c31217b1df02.csv`.
3.  **Contextos de Domínio**: Consulte `docs/Liquid Agentic/Domain Contexts 4576c1c143d6498492710ab73ad500e9.csv`.
4.  **Contextos de Componentes**: Consulte `docs/Liquid Agentic/Components Contexts 032e28d0d4944ecc8c1d9e3f492d5cda.csv`.
5.  **Mapeamento de Estruturas**: Para referências de estruturação entre telas base e o Liquid, consulte `docs/Liquid Agentic/📎 Paperclip → Liquid — Mapeamento de Estruturas 33bcb0551171815db465fcc1212b76bc.md`.

## Protocolo de Decisão

- **Se for uma alteração visual**: Valide contra o `design-system`.
- **Se for um novo fluxo**: Valide visualmente contra o `interaction-models` em `patterns`, e funcionalmente através do Business Logic em `/docs`.
- **Se houver ambiguidade**: Consulte a `cognitive-architecture` em `context` ou as referências gerais no `/docs`.

---
*Liquid OS — Context Engineering for the Agentic Era.*
