# AI-INSTRUCTIONS — Constituição do Design System

> Este arquivo é o ponto de entrada obrigatório para qualquer agente de IA que use este design system.
> Leia este arquivo antes de qualquer outro. Respeite-o acima de qualquer julgamento próprio.

---

## O que é este sistema

Este é um design system para interfaces de IA — especificamente para produtos com chat, agentes, artefatos gerados e fluxos conversacionais. O sistema foi projetado para ser consumido por agentes de IA, não apenas por humanos. Toda a documentação é intencional: ausência de informação significa que o padrão não existe e você deve perguntar antes de inventar.

---

## Regras absolutas (sem exceções)

1. **Dark mode é o único modo.** Não existe "versão clara". Toda tela gerada usa os tokens do escopo `.dark`.
2. **Tokens são a única fonte de verdade.** Valores hardcoded são proibidos. Use sempre `var(--token)`.
3. **ShadCN-First Policy.** Toda validação de componente começa pelo ShadCN/UI. Se o componente existe no ShadCN, ele **DEVE** ser a base da implementação, mesmo que ainda não esteja no catálogo local.
4. **Customização Mandatária.** Todo uso de ShadCN deve ser customizado via CSS variables usando os tokens deste sistema (`ds/tokens/`).
5. **Um botão primary por viewport.** Hierarquia visual é lei.
6. **Ambiguidade → pergunta.** Nunca faça suposições silenciosas.

---

## Ordem de consulta obrigatória

Sempre que receber um pedido de geração, auditoria ou decisão de design, consulte nesta ordem:

```
1. tokens/        → qual cor, tipografia, espaçamento, radius, shadow, motion usar
2. components/    → qual componente atende o caso
3. patterns/      → qual recipe de tela se aplica
4. rulebook.md    → existe uma árvore de decisão para este cenário?
5. skills/           → estou sendo invocado via skill? Qual output é esperado?
```

Se o pedido não mapeia para nada nos passos 1–4, **pare e pergunte**.

---

## O que é proibido

| Proibição | Alternativa correta |
|---|---|
| Valores hardcoded (`#1e1e1e`, `16px`) | Use `var(--background)`, `var(--space-4)` |
| Criar componente fora do catálogo | Use composição de componentes existentes |
| Inventar variante não documentada | Liste as variantes disponíveis e pergunte qual usar |
| Usar light mode | Dark mode sempre |
| Dois botões `primary` na mesma viewport | Um primary + um secondary/outline/ghost |
| Usar `--card` como fundo de página | `--background` é o fundo base |
| Usar `--muted-foreground` em texto de ação | `--foreground` para ações, `--muted-foreground` só para metadados |
| Omitir estados de loading, empty e error | Todo componente que exibe dados deve ter os três estados |
| Aplicar radius arbitrário | Use os tokens `--radius-sm` a `--radius-4xl` |
| Criar layout sem primitives | Use Stack, Inline, Grid e AppShell como base |

---

## Como processar um pedido de tela

```
Pedido recebido
│
├── Mapear componentes necessários
│     ├── 1. Existe no ShadCN/UI?
│     │     ├── Sim → Validar se já existe no catálogo local (ds/components/)
│     │     │     ├── Sim → Usar definição local e customização de tokens
│     │     │     └── Não → Adotar primitive ShadCN + Aplicar tokens do DS
│     │     └── Não → É um componente customizado? (ex: Artefatos IA)
│     │           ├── Sim → Usar definição local
│     │           └── Não → GAP: Perguntar ao designer
│
├── Identificar o tipo de tela (patterns/)
│     ├── Listagem de dados → pattern: list-page
│     ├── Detalhe de item → pattern: detail-page
│     ├── Formulário/wizard → pattern: form-flow
│     └── Fluxo de IA/chat → pattern: ai-workflow
│
├── Aplicar tokens
│     ├── Cores → tokens/colors.md
│     ├── Tipo → tokens/typography.md
│     ├── Espaço → tokens/spacing.md
│     └── Efeitos → tokens/effects.md
│
├── Verificar regras no rulebook.md
│     └── Existe anti-pattern sendo violado?
│
└── Gerar código HTML completo
      └── Sem valores hardcoded. Prioridade ShadCN. Salvar em .deliveries/.
```

---

## Como processar ambiguidade

Se o pedido for ambíguo ou incompleto, pergunte sobre:

- **Entidade principal**: o que está sendo exibido ou manipulado?
- **Ação primária**: qual é a ação mais importante do usuário nesta tela?
- **Estados necessários**: loading, empty, error são requeridos?
- **Hierarquia de ações**: quais ações são primárias, secundárias, destrutivas?
- **Contexto**: é uma tela standalone, modal, drawer ou painel?

Nunca assuma. Uma pergunta a mais é melhor que um componente inventado.

---

## Estrutura de arquivos do DS

```
.
├── ds/
    ├── AI-INSTRUCTIONS.md      ← você está aqui
    ├── tokens/                 ← cores, tipografia, espaçamento, efeitos
    ├── components/             ← catálogo de componentes
    ├── patterns/               ← recipes de tela
    ├── rulebook.md             ← árvores de decisão + anti-patterns
    └── skills/                 ← skills invocáveis (skills.md)
```

---

## Grupos de componentes

### Artefatos (outputs da IA)
Componentes que a IA usa para exibir resultados: `Artifact — Card`, `Artifact — Chart`, `Artifact — Datatable`, `Artifact — Document`, `Artifact — Graph`, `Artifact — Input Request`, `Artifact — KPI`, `Artifact — Link`, `Artifact — List`.

### Chat/Interação
Interface conversacional: `Chat`, `User Input`, `Structured Inputs`, `Text`, `Voice`, `Suggestions`, `Attachments`.

### Sistema
Infraestrutura do produto: `Agent Icon`, `Connectors`, `Files`, `Knowledge Base`, `Knowledge Base Indicator`, `Market`, `New Task`, `Skills`.

### Navegação
Orientação e estrutura: `Account Switch`, `Navbar`.

---

## O que fazer quando há gap no DS

Se você identificar que:
- Um cenário não está coberto por nenhum componente documentado
- Uma regra está ausente no rulebook
- Um token semântico não existe para o caso

→ **Não invente.** Informe o gap explicitamente ao designer. Use a estrutura:

```
GAP IDENTIFICADO:
Cenário: [descrição do que falta]
Componente mais próximo disponível: [nome]
Limitação: [por que não atende completamente]
Pergunta: [o que o designer precisa decidir]
```

---

## Métricas de aderência esperadas

- 100% dos valores visuais referenciados por token semântico
- 0 componentes fora do catálogo sem aprovação explícita
- Recipe correto selecionado para o tipo de tela
- Todos os estados obrigatórios (loading, empty, error) presentes quando aplicável
- Hierarquia de botões respeitada: no máximo 1 primary por viewport
