# Liquid OS — Design as Context

Este repositório atua como o **Context Engineering** do Liquid OS. Ele é o mapa de conhecimento estruturado projetado para rotear agentes de IA e engenheiros, transformando visão de produto e arquitetura visual em contexto de linguagem natural consumível por máquina.

## 🤖 Pontos de Entrada para Agentes (AI Entry Points)

Ao iniciar qualquer tarefa neste ecossistema, os agentes devem consultar os hubs centrais:

1. **[AGENTS.md](./AGENTS.md)**: Protocolo global e roteador dinâmico de contexto.
2. **[CLAUDE.md](./CLAUDE.md)**: Regras de comportamento e comandos específicos (Claude Code).
3. **[AI-DESIGN-ROUTER.md](./design/AI-DESIGN-ROUTER.md)**: Wiki e diretiva primária para decisões visuais e de interface.
4. **[UX Design Document (UXDD)](./.deliveries/2026-04-10-ux-design-document.md)**: Guia mestre de experiência e fluxos do Liquid OS.
5. **[Liquid Agentic Documentation](./docs/product/liquid-agentic-os.md)**: Base de conhecimento de produto.

## 🏗️ Estrutura Arquitetural do Conhecimento

A taxonomia do projeto está particionada para o menor consumo de tokens possível e *progressive disclosure* eficiente na camada de AI:

- **`/docs` (Regras de Negócio, Produto e Domínio)**
  - Base de conhecimento do conceito **Liquid Agentic**. Contém princípios, glossários, descritivos profundos de componentes (`Components Contexts`), domínios de negócio (`Domain Contexts`) e arquitetura lógica.
  
- **`/design` (Sistema Visual, Estético e Comportamental)**
  -  A base da experiência do usuário, englobando filosofia (`/context`), padrões interativos e *microcopy* (`/patterns`), além do guia rigoroso de pragmatismo técnico para codificação visual e tokens (`/design-system`).

## 🛠️ Stack Tecnológico de Referência

O ecossistema é construído usando padronização estrita de implementação:
- **Core Frontend**: Next.js (implementação técnica via repositório `liquify`)
- **Estilização e Componentes**: Tailwind CSS + ShadCN UI
- **Padrão de Construção**: Atomic Design estruturado
- **Abordagem Documental**: IA-First (Markdown semiótico e sintético adaptado para inferência sistêmica)

## 🚩 Princípios Imutáveis do Produto

1. **Global** (Infraestrutura *English-first*)
2. **Product-Led Growth** (Sistemas amigáveis e guiados a adoção autônoma)
3. **Agent First** (Planejado, arquitetado e lido por/para Inteligências Artificiais)
4. **Liquidity Driven** (Informações transferíveis de forma natural via conectores)
5. **Regulated Market** (Escalável e com alta rigidez de compliance)
6. **Multiplayer** (Colaboração Humana + IAs simultaneamente integradas)

---
### 🚩 Status do Projeto
- [x] **UX Design Document (UXDD)**: Mapeamento completo de fluxos e arquétipos.
- [x] **Agent OS Prototype**: Implementação de high-fidelity artifacts (KPI, Table, Chart) e Lifecycle HUD (LIQ-n).
- [x] **Design System Consolidation**: Unificação da documentação técnica e product-specific do componente `Navbar`.
- [x] **Repository Hygiene**: Configuração de padrões de exclusão no `.gitignore` para pastas de processos internos.
- [x] **Notion Comments Review (2026-04-10)**: Revisão e aplicação de comentários das páginas 4.3, 4.5 e 4.7 do Notion. Alterações: taxonomia de dashboards generalizada (Pipeline Dashboard), remoção de limites absolutos de geração substituídos por julgamento composicional do agente, formato de valores financeiros atualizado para locale-aware, arquétipos de entidade expandidos com contexto operacional completo.
- [ ] **Data Integration**: Conexão com bases reais (Alpha Portfolio).

*Liquid OS — Context Engineering for the Agentic Era.*
