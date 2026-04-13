# Rulebook — Árvores de Decisão e Anti-patterns

> Consulte este arquivo quando o componente ou pattern correto não for imediatamente óbvio.
> Se o cenário não está aqui, é um gap — documente e pergunte antes de inventar.

---

## Como usar este rulebook

1. Identifique o cenário de decisão
2. Localize a árvore correspondente
3. Siga os ramos até o componente/ação correto
4. Se o caminho terminar sem resposta → GAP: pergunte

---

## Árvores de decisão

### 01 — Feedback de ação do usuário

```
Preciso dar feedback de uma ação do usuário?
├── É uma ação destrutiva irreversível? (delete, reset, revoke)
│     └── → Modal de confirmação destrutiva
│           ├── Título: "Tem certeza?" + nome da entidade
│           ├── Descrição: consequências claras
│           ├── Botão: "Confirmar" (--destructive)
│           └── Botão: "Cancelar" (ghost)
│
├── A ação está sendo processada? (loading)
│     └── → Spinner no botão que iniciou a ação (inline loading)
│           └── Não use modal nem overlay para loading de ação simples
│
├── A ação foi concluída com sucesso?
│     └── → Toast (duração 4s, canto inferior direito)
│           ├── Cor: sucesso (--chart-2)
│           └── Mensagem: máximo 60 caracteres
│
├── Ocorreu um erro de sistema? (API, timeout, 500)
│     └── → Banner inline no topo do conteúdo afetado
│           ├── Descrição do erro + ação de retry
│           └── Não use modal para erros de sistema
│
├── É um erro de validação de campo?
│     └── → Mensagem inline abaixo do campo
│           ├── Cor: --destructive
│           └── Texto: body-sm
│
└── É um aviso não-crítico? (informação, dica)
      └── → Banner informativo inline (não toast, não modal)
```

---

### 02 — Qual artefato usar para o output da IA

```
A IA precisa exibir um output. Que tipo de dado é?
│
├── Valor numérico único com contexto (métrica, KPI)
│     └── → ArtifactKPI
│
├── Uma entidade com múltiplos atributos
│     └── → ArtifactCard
│
├── Lista de itens simples (sem colunas)
│     └── → ArtifactList
│
├── Múltiplos registros com colunas definidas
│     └── → ArtifactDatatable
│
├── Visualização quantitativa (comparação, tendência)
│     └── → ArtifactChart
│           ├── Comparação entre categorias → bar
│           ├── Evolução temporal → line ou area
│           ├── Distribuição percentual → pie ou donut (máx 6 fatias)
│           └── Correlação entre variáveis → scatter
│
├── Relações entre entidades / diagrama
│     └── → ArtifactGraph
│
├── Texto longo estruturado (relatório, e-mail, código)
│     └── → ArtifactDocument
│
├── Fonte ou URL externa
│     └── → ArtifactLink
│
├── A IA precisa de dados do usuário para continuar
│     └── → ArtifactInputRequest
│
└── Texto conversacional simples
      └── → Text (role=agent)
```

---

### 03 — Qual pattern de tela usar

```
Que tipo de tela preciso montar?
│
├── Mostrar uma coleção de registros com filtros
│     └── → ListPage
│
├── Mostrar detalhes de um item específico
│     └── → DetailPage
│
├── Criar ou editar dados estruturados
│     └── → FormFlow
│           ├── ≤ 4 campos → modal-form
│           ├── > 4 campos, 1 etapa → single-step
│           └── Múltiplas etapas → multi-step
│
├── Interface de chat/conversa com agente
│     └── → AIWorkflow
│
└── Nenhum dos acima
      └── → GAP: descreva o caso e pergunte
```

---

### 04 — Quantos botões primários posso ter?

```
Quantos botões primary nesta tela/seção?
│
├── Viewport completa → máximo 1 botão primary
│
├── Modal → máximo 1 botão primary
│
├── Card → máximo 1 botão primary
│
├── FormActions → 1 SubmitButton (primary) + BackButton (ghost)
│
└── Se precisar de 2 ações igualmente importantes:
      └── 1 primary + 1 secondary/outline
            └── Nunca 2 primary
```

---

### 05 — Quando usar modal vs página vs inline

```
Preciso mostrar conteúdo adicional. Como?
│
├── Confirmação de ação destrutiva
│     └── → Modal (sempre)
│
├── Formulário simples (≤ 4 campos)
│     └── → Modal
│
├── Formulário complexo (> 4 campos)
│     └── → Página dedicada (FormFlow)
│
├── Detalhe de item em lista
│     └── → DetailPage (navegação)
│           └── Excepcionalmente: Sheet lateral (se contexto permite manter lista visível)
│
├── Configurações contextuais
│     └── → Popover ou Dropdown
│
├── Conteúdo que o usuário pediu (artefato)
│     └── → Inline no Chat (nunca em modal)
│
└── Mensagem de erro ou sucesso
      └── → Toast (sucesso/info transitório)
            → Banner inline (erro persistente)
            → Nunca em modal
```

---

### 06 — Hierarquia visual de ações

```
Esta tela tem múltiplas ações. Como organizo?
│
├── Ação principal da tela → botão primary (1 por viewport)
│
├── Ação secundária importante → botão secondary ou outline
│
├── Ação de cancelamento/retorno → botão ghost ou link
│
├── Ações adicionais (3+) → overflow menu (3 pontos)
│     └── Ações destrutivas: sempre no final do menu, separadas por divider
│
└── Ação destrutiva isolada → botão ghost --destructive
      └── Nunca botão primary --destructive (exceto modal de confirmação)
```

---

### 07 — Estados de componentes com dados

```
O componente vai exibir dados externos (API). Quais estados são obrigatórios?
│
├── loading → skeleton animado (nunca spinner de página inteira para dados parciais)
├── empty → mensagem + CTA de próxima ação
├── error → mensagem de erro + retry
└── populated → dados completos
│
└── Se o dado pode ser filtrado:
      └── filtered-empty → "Sem resultados para os filtros" + botão limpar filtros
```

---

### 08 — Tipografia: qual role usar

```
Que tipo de texto preciso estilizar?
│
├── Título principal da página → heading-h4 (Geist Bold 24px)
├── Título de seção → heading-section (Geist SemiBold 20px)
├── Título de card/painel → heading-card (Geist SemiBold 16px)
├── Label de navegação / destaque → label-1 (Geist Medium 14px)
├── Label secundário / campo → label-2 (Geist Medium 12px)
├── Micro-label / badge / timestamp → label-3 (Geist 11px)
├── Corpo principal / mensagem → body-lg (Geist Regular 14px)
├── Corpo secundário / descrição → body-md (Geist Regular 12px)
├── Nota / caption → body-sm (Geist Regular 11px)
└── Código → code-base (Geist Mono 14px) ou code-sm (12px)
```

---

### 09 — Qual token de cor para texto

```
Texto sobre qual superfície?
│
├── Sobre --background → --foreground (principal) ou --muted-foreground (secundário)
├── Sobre --card → --card-foreground
├── Sobre --popover → --popover-foreground
├── Sobre --primary (botão) → --primary-foreground
├── Sobre --destructive (botão) → branco (--foreground root)
├── Sobre --sidebar → --sidebar-foreground
└── Sobre --muted → --foreground (principal) ou --muted-foreground (descritivo)
```

---

### 10 — Qual superfície usar como background

```
Que tipo de container preciso?
│
├── Base da página → --background
├── Sidebar / painel lateral → --sidebar
├── Card / painel de conteúdo elevado → --card
├── Dropdown / popover / tooltip → --popover
├── Seção secundária / input → --muted
└── Item em hover/foco → --accent
│
└── PROIBIDO: --card como fundo de página
```

---

### 11 — Navegação: active state

```
Como indicar item ativo na navegação?
│
├── NavItem na sidebar → background --sidebar-primary, texto --sidebar-primary-foreground
├── Tab no ContentTabs → underline --primary, texto --foreground
├── Chip/filtro selecionado → background --primary, texto --primary-foreground
└── Item de lista selecionado → background --accent, texto --foreground
```

---

### 12 — Quando usar skeleton vs spinner

```
Preciso indicar que algo está carregando. Uso skeleton ou spinner?
│
├── Carregando conteúdo de área delimitada (card, tabela, lista)
│     └── → Skeleton (placeholder da forma do conteúdo)
│
├── Carregando ação de botão (submit, etc.)
│     └── → Spinner inline no botão
│
├── Processamento de tarefa longa (>3s) com progresso mensurável
│     └── → ProgressBar com StatusBar
│
├── Processamento de tarefa longa sem progresso mensurável
│     └── → Spinner + texto descritivo da etapa
│
└── Carregando página inteira (navegação)
      └── → Skeleton da estrutura da página
            └── Nunca spinner de página inteira bloqueante
```

---

### 13 — Formulários: quando validar

```
Quando devo validar os dados do formulário?
│
├── Ao sair do campo (onBlur) → validação de formato e obrigatoriedade
├── Ao tentar submeter → validação completa de todos os campos
├── Enquanto digita → apenas para:
│     ├── Força de senha
│     ├── Disponibilidade de slug/username (debounce 500ms)
│     └── Campos com caracteres permitidos específicos
└── Nunca validar apenas no submit sem feedback por campo
```

---

### 14 — Seleção de Componente (ShadCN-First)

```
Preciso de um componente de UI. Qual usar?
│
├── O componente existe nativamente no ShadCN/UI?
│     └── → Sim: Adotar a primitive do ShadCN.
│           ├── Já está no catálogo local (ds/components/)?
│           │     └── → Sim: Seguir definição local (props, variantes).
│           │     └── → Não: Propor instalação via CLI e ignorar criação manual.
│           └── Importante: Aplicar tokens do DS via CSS variables obrigatoriamente.
│
├── O componente é um artefato específico de IA ou feature proprietária?
│     └── → Sim: Verificar `ds/components/ia-views/` ou `sistema/`.
│           └── Se não existir → GAP: perguntar.
│
└── Não existe no ShadCN nem é um artefato conhecido?
      └── → GAP IDENTIFICADO. Não invente uma solução com tags HTML puras.
```

---

## Anti-patterns globais

### Hierarquia e layout

| Anti-pattern | Por que é errado | Alternativa |
|---|---|---|
| Dois botões `primary` na mesma viewport | Destrói a hierarquia de ação — usuário não sabe o que fazer | 1 primary + 1 secondary/outline |
| `--card` como fundo de página | Card é elevação, não base — causa problemas de contraste e hierarquia | Use `--background` |
| Empilhar `--card` sobre `--card` sem diferenciação | Perda de hierarquia visual | Use `--muted` para o nível interno |
| Layout sem primitives (Stack, Grid, AppShell) | Tela inconsistente entre contextos e breakpoints | Sempre use as primitives de layout |
| Scroll da página inteira para o Chat | Chat tem scroll independente por design | `ChatScrollArea` com overflow interno |

### Tipografia

| Anti-pattern | Por que é errado | Alternativa |
|---|---|---|
| Dois `heading-h4` na mesma tela | Hierarquia ambígua — o que é o título principal? | Um único `heading-h4` por viewport |
| Tamanhos fora da escala (13px, 15px) | Quebra consistência visual | Usar o tamanho mais próximo da escala |

### Cores e tokens

| Anti-pattern | Por que é errado | Alternativa |
|---|---|---|
| Valores hardcoded (#1e1e1e, 14px) | Quebra a rastreabilidade — impossível auditar e manter | `var(--token-name)` |
| `--muted-foreground` em texto de ação | `--muted-foreground` é para metadados, não ações | `--foreground` para ações |
| `--primary` como cor de texto corrido | `--primary` é para elementos interativos, não texto | `--foreground` para texto |
| `--destructive` em avisos não-críticos | Dilui o impacto de ações realmente destrutivas | Banner informativo com `--foreground` |
| Cores de chart (`--chart-*`) fora de artefatos | São paletas de série — não comunicam estado fora desse contexto | Use os tokens semânticos de estado |

### Componentes e artefatos

| Anti-pattern | Por que é errado | Alternativa |
|---|---|---|
| Text (role=agent) para dados tabulares | Texto plano não permite scan, ordenação, exportação | ArtifactDatatable |
| ArtifactChart para dado único | Gráfico de uma série não agrega valor | ArtifactKPI |
| Artifact fora do contexto de mensagem | Artefatos nascem de uma interação — exibir solto confunde | Sempre dentro da MessageList, vinculado à mensagem |
| Componente fora do catálogo inventado | Nenhuma garantia de consistência futura | Usar composição dos componentes existentes |
| Link como texto puro (URL exposta) | Ilegível e sem contexto de destino | ArtifactLink (variante inline ou rich) |
| Criar UI customizada com equivalente no ShadCN | Ignora a fundação técnica do projeto e dificulta manutenção | Usar primitive ShadCN + Tokens do DS |
| Usar ShadCN sem customizar tokens | Quebra a identidade visual única do projeto | Sobrescrever CSS variables nativas com tokens do DS |

### Estados e feedback

| Anti-pattern | Por que é errado | Alternativa |
|---|---|---|
| Modal para mensagem de sucesso | Modal bloqueia e exige ação para fechar algo positivo | Toast (4s, auto-dismiss) |
| Toast para erro persistente | Toast desaparece — erro pode ser perdido | Banner inline permanente |
| Spinner de página inteira para dados parciais | Bloqueia toda a interface desnecessariamente | Skeleton da área que está carregando |
| Formulário sem estado de validação inline | Usuário descobre erros apenas ao submeter | Validação no onBlur por campo |
| Componente sem estado `empty` ou `error` | IA pode gerar tela sem esses estados — bugs de UX inevitáveis | Sempre especificar os três estados: loading, empty, error |

### Dark mode e motion

| Anti-pattern | Por que é errado | Alternativa |
|---|---|---|
| Light mode | O sistema não tem light mode | Dark mode sempre |
| `transition: all 0.3s ease` | "all" anima propriedades não-intencionais; easing não é do sistema | Especificar propriedade + usar `--ease-*` + `--duration-*` |
| Animação sem `prefers-reduced-motion` | Exclui usuários com sensibilidade a movimento | Usar `--duration-instant` quando `prefers-reduced-motion: reduce` |
