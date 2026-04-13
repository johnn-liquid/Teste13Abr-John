# Componente — Knowledge Base

## Nome canônico
`KnowledgeBase`

## Descrição
Base de conhecimento consultável pelo agente. Painel ou seção que exibe os documentos, arquivos e fontes de dados que o agente tem acesso. Pode ser configurada por workspace ou por conversa.

---

## Anatomia

```
KnowledgeBase
├── KBHeader                ← título + ação de adicionar fonte
│   ├── Title ("Base de Conhecimento")
│   └── AddButton
├── KBSearch                ← input de busca dentro da base
├── SourceList              ← lista de fontes disponíveis
│   └── SourceItem[]        ← item individual de fonte
│       ├── SourceIcon      ← ícone por tipo (documento, URL, DB)
│       ├── SourceName      ← nome da fonte
│       ├── SourceMeta      ← tipo + data de indexação
│       └── SourceActions   ← ativar/desativar, remover
└── KBFooter                ← contagem de fontes + status de indexação
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `panel` | Painel lateral dentro do layout with-sidebar do Chat |
| `modal` | Modal de configuração da base de conhecimento |
| `compact` | Lista simplificada sem search — para sidebar colapsada |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `empty` | Ilustração + CTA "Adicionar primeira fonte" |
| `loading` | SourceList com skeletons |
| `populated` | Lista de fontes com status de indexação |
| `indexing` | SourceItem com spinner + "Indexando..." |
| `error` | SourceItem com ícone de erro + ação de retry |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `variant` | `panel \| modal \| compact` | `panel` | Layout do componente |
| `sources` | `Source[]` | obrigatório | Lista de fontes |
| `onAdd` | `function` | obrigatório | Callback de adição de fonte |
| `onRemove` | `function(sourceId)` | obrigatório | Callback de remoção |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background (panel) | `--sidebar` |
| Background (modal) | `--card` |
| Título | `--foreground` + `label-1` |
| SourceName | `--foreground` + `body-md` |
| SourceMeta | `--muted-foreground` + `body-sm` |
| Borda separadora | `--border` |
| Status indexando | `--primary` |
| Status erro | `--destructive` |
| Radius dos itens | `--radius-md` |

---

## Regras de uso

- **Use KnowledgeBase** para gerenciar o contexto de dados que o agente acessa
- Cada SourceItem tem toggle para ativar/desativar sem remover a fonte
- Fontes desativadas ficam com opacity 50% e não são consultadas pelo agente
- KBFooter exibe "N fontes ativas" para comunicar o estado atual ao usuário

---

## Anti-patterns

- **PROIBIDO**: exibir KnowledgeBase sem indicar quais fontes estão ativas vs inativas
- **PROIBIDO**: remover fonte sem confirmação de destructive action
- **PROIBIDO**: limitar o número de fontes sem comunicar o limite ao usuário

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
