# Componente — Artifact Chart

## Nome canônico
`ArtifactChart`

## Descrição
Gráfico gerado como artefato pela IA. Visualização de dados quantitativos em formato visual (barras, linhas, pizza, área). Usa os tokens de chart do sistema para consistência de cor.

---

## Anatomia

```
ArtifactChart
├── ArtifactHeader          ← label "Gráfico" + tipo + ações
│   ├── ChartTitle          ← título do gráfico
│   ├── ChartTypeLabel      ← "Barras" / "Linhas" / "Pizza" etc.
│   └── ArtifactActions     ← copiar imagem, exportar dados, expandir
├── ChartArea               ← área de renderização do gráfico
│   ├── ChartCanvas         ← o gráfico em si
│   ├── ChartLegend         ← legenda das séries
│   └── ChartTooltip        ← tooltip interativo nos pontos
└── ChartFooter             ← fonte dos dados + timestamp
```

---

## Tipos de gráfico suportados

| Tipo | Quando usar |
|---|---|
| `bar` | Comparação entre categorias |
| `line` | Evolução temporal ou tendências |
| `area` | Volume acumulado ao longo do tempo |
| `pie` | Distribuição de partes de um todo (máx. 6 fatias) |
| `donut` | Distribuição com valor central destacado |
| `scatter` | Correlação entre duas variáveis |

---

## Dados obrigatórios vs opcionais

| Campo | Obrigatoriedade |
|---|---|
| `title` | Obrigatório |
| `type` | Obrigatório |
| `data` | Obrigatório |
| `source` | Recomendado |
| `legend` | Obrigatório quando há múltiplas séries |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `loading` | Skeleton retangular da área do gráfico + shimmer |
| `populated` | Gráfico interativo completo |
| `error` | Ícone de erro + "Não foi possível gerar o gráfico" + retry |
| `empty` | "Sem dados para este período" com ícone de gráfico vazio |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `title` | `string` | obrigatório | Título do gráfico |
| `type` | `bar \| line \| area \| pie \| donut \| scatter` | obrigatório | Tipo de visualização |
| `data` | `ChartData` | obrigatório | Dados estruturados para o gráfico |
| `height` | `number` | `240` | Altura em px da ChartArea |
| `showLegend` | `boolean` | `true` | Exibir legenda |
| `source` | `string` | — | Fonte dos dados |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background container | `--card` |
| Borda container | `--border` |
| Shadow | `--shadow-sm` |
| ChartTitle | `--card-foreground` + `heading-card` |
| Série 1 | `--chart-1` |
| Série 2 | `--chart-2` |
| Série 3 | `--chart-3` |
| Série 4 | `--chart-4` |
| Série 5 | `--chart-5` |
| Eixos e grid | `--border` |
| Labels de eixo | `--muted-foreground` + `body-sm` |
| Background tooltip | `--popover` |
| Texto tooltip | `--popover-foreground` + `body-sm` |
| Radius container | `--radius-lg` |
| Padding container | `--space-4` |

---

## Comportamento dentro do Chat

- Renderizado diretamente na MessageList
- Altura padrão de 240px — expansível para 480px clicando em "Expandir"
- Tooltips ativados no hover dos pontos/barras
- ArtifactActions sempre visíveis (não apenas hover)

---

## Anti-patterns

- **PROIBIDO**: usar cores fora dos tokens `--chart-*` para séries
- **PROIBIDO**: gráfico de pizza com mais de 6 fatias — use barras
- **PROIBIDO**: omitir legenda quando há múltiplas séries
- **PROIBIDO**: ChartArea sem altura definida (causa CLS)
- **PROIBIDO**: usar ArtifactChart para dado único — use ArtifactKPI

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
