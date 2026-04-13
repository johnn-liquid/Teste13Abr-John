# Componente — Artifact Graph

## Nome canônico
`ArtifactGraph`

## Descrição
Grafo ou diagrama de relacionamentos gerado pela IA. Para visualizar conexões entre entidades (redes, dependências, hierarquias, fluxos). Diferente do ArtifactChart que é para dados quantitativos, o ArtifactGraph é para estruturas relacionais.

---

## Anatomia

```
ArtifactGraph
├── ArtifactHeader          ← label "Grafo" + título + ações
│   └── ArtifactActions     ← exportar imagem, expandir
├── GraphCanvas             ← área interativa do grafo
│   ├── GraphNodes[]        ← nós do grafo
│   │   ├── NodeIcon
│   │   └── NodeLabel
│   ├── GraphEdges[]        ← arestas/conexões
│   │   └── EdgeLabel (opcional)
│   └── GraphControls       ← zoom +/-, fit to screen, reset
└── GraphLegend (opcional)  ← tipos de nós e arestas
```

---

## Tipos de grafo suportados

| Tipo | Quando usar |
|---|---|
| `network` | Relações entre entidades sem hierarquia |
| `tree` | Hierarquias (org chart, file tree) |
| `flowchart` | Fluxos de processo com decisões |
| `dag` | Directed Acyclic Graph — pipelines, dependências |

---

## Dados obrigatórios vs opcionais

| Campo | Obrigatoriedade |
|---|---|
| `nodes` | Obrigatório |
| `edges` | Obrigatório |
| `title` | Recomendado |
| `type` | Obrigatório |
| `legend` | Opcional |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `loading` | Spinner centralizado no GraphCanvas |
| `populated` | Grafo interativo renderizado |
| `error` | "Erro ao renderizar grafo" + retry |
| `empty` | "Nenhuma relação para exibir" |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `title` | `string` | — | Título do grafo |
| `type` | `network \| tree \| flowchart \| dag` | obrigatório | Tipo de visualização |
| `nodes` | `Node[]` | obrigatório | Lista de nós |
| `edges` | `Edge[]` | obrigatório | Lista de arestas |
| `height` | `number` | `320` | Altura do canvas |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background container | `--card` |
| Borda container | `--border` |
| GraphCanvas background | `--background` |
| Nó primário | `--primary` background + `--primary-foreground` texto |
| Nó secundário | `--muted` background + `--foreground` texto |
| Aresta | `--border` com stroke |
| NodeLabel | `label-3` + `--foreground` |
| EdgeLabel | `body-sm` + `--muted-foreground` |
| Radius container | `--radius-lg` |

---

## Comportamento dentro do Chat

- Altura padrão 320px — expandível via "Expandir"
- Grafo é pan+zoom interativo
- No mobile, apenas visualização estática sem interação

---

## Anti-patterns

- **PROIBIDO**: grafo com mais de 50 nós sem clustering/agrupamento
- **PROIBIDO**: usar ArtifactGraph para dados quantitativos — use ArtifactChart
- **PROIBIDO**: arestas sem direção em grafos do tipo `dag` ou `flowchart`

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
