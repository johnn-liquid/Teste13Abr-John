# Tokens — Colors

> Fonte: shadcn/ui preset `b1a0R3VpI` extraído via `npx shadcn@latest init`.
> Formato: oklch (perceptually uniform). Dark mode é o padrão do sistema.
> O CSS completo gerado vive em `.deliveries/tokens/globals.css`.

---

## Regra absoluta

**Nenhum valor de cor pode ser usado diretamente (hex, rgb, oklch literal).
Toda cor referenciada em um componente deve usar um token semântico (`var(--token)`).**

---

## Tokens semânticos — Dark Mode (valores canônicos)

O sistema opera em dark mode. Os valores abaixo são os do escopo `.dark`.

### Backgrounds e Superfícies

| Token | Valor OKLCH | Papel |
|---|---|---|
| `--background` | `oklch(0.141 0.005 285.823)` | Fundo base da aplicação |
| `--card` | `oklch(0.21 0.006 285.885)` | Superfície de cards e painéis elevados |
| `--popover` | `oklch(0.21 0.006 285.885)` | Superfície de popovers, dropdowns e tooltips |
| `--muted` | `oklch(0.274 0.006 286.033)` | Superfície de seções secundárias, backgrounds de input, tags |
| `--accent` | `oklch(0.274 0.006 286.033)` | Superfície de itens em hover/foco em listas e menus |
| `--secondary` | `oklch(0.274 0.006 286.033)` | Superfície de elementos secundários |
| `--sidebar` | `oklch(0.21 0.006 285.885)` | Superfície do painel lateral (navbar/sidebar) |

### Texto e Conteúdo

| Token | Valor OKLCH | Papel |
|---|---|---|
| `--foreground` | `oklch(0.985 0 0)` | Texto principal — corpo, títulos, labels |
| `--card-foreground` | `oklch(0.985 0 0)` | Texto sobre cards |
| `--popover-foreground` | `oklch(0.985 0 0)` | Texto sobre popovers |
| `--primary-foreground` | `oklch(0.141 0.005 285.823)` | Texto sobre elementos primary (ex: background escuro sobre botão branco) |
| `--secondary-foreground` | `oklch(0.985 0 0)` | Texto sobre elementos secondary |
| `--muted-foreground` | `oklch(0.705 0.015 286.067)` | Texto secundário — placeholders, metadados, captions |
| `--accent-foreground` | `oklch(0.985 0 0)` | Texto sobre accent |
| `--sidebar-foreground` | `oklch(0.985 0 0)` | Texto no sidebar |
| `--sidebar-primary-foreground` | `oklch(0.141 0.005 285.823)` | Texto sobre item primário do sidebar |
| `--sidebar-accent-foreground` | `oklch(0.985 0 0)` | Texto sobre item em hover no sidebar |

### Ações e Interação

| Token | Valor OKLCH | Papel |
|---|---|---|
| `--primary` | `oklch(1 0 0)` | Ação primária — o principal destaque é Branco |
| `--destructive` | `oklch(0.704 0.191 22.216)` | Ação destrutiva — delete, erro crítico |
| `--ring` | `oklch(0.552 0.016 285.938)` | Outline de foco (focus ring) |
| `--sidebar-primary` | `oklch(1 0 0)` | Item de navegação ativo no sidebar (branco) |

### Bordas e Divisores

| Token | Valor OKLCH | Papel |
|---|---|---|
| `--border` | `oklch(1 0 0 / 10%)` | Bordas de componentes (cards, inputs, dividers) |
| `--input` | `oklch(1 0 0 / 15%)` | Background e borda de campos de input |
| `--sidebar-border` | `oklch(1 0 0 / 10%)` | Borda do sidebar |
| `--sidebar-ring` | `oklch(0.552 0.016 285.938)` | Focus ring dentro do sidebar |

### Charts (uso em Artifact — Chart e Artifact — Graph)

| Token | Valor OKLCH | Papel |
|---|---|---|
| `--chart-1` | `oklch(0.809 0.105 251.813)` | Série 1 — azul claro |
| `--chart-2` | `oklch(0.623 0.214 259.815)` | Série 2 — azul médio |
| `--chart-3` | `oklch(0.546 0.245 262.881)` | Série 3 — azul escuro |
| `--chart-4` | `oklch(0.488 0.243 264.376)` | Série 4 — azul-violeta |
| `--chart-5` | `oklch(0.424 0.199 265.638)` | Série 5 — violeta |

---

## Tokens semânticos — Light Mode (referência)

O sistema não usa light mode por padrão. Estes valores existem apenas como referência técnica.

| Token | Valor OKLCH |
|---|---|
| `--background` | `oklch(1 0 0)` |
| `--foreground` | `oklch(0.141 0.005 285.823)` |
| `--card` | `oklch(1 0 0)` |
| `--primary` | `oklch(0.21 0.006 285.885)` |
| `--secondary` | `oklch(0.967 0.001 286.375)` |
| `--muted` | `oklch(0.967 0.001 286.375)` |
| `--muted-foreground` | `oklch(0.552 0.016 285.938)` |
| `--border` | `oklch(0.92 0.004 286.32)` |
| `--input` | `oklch(0.92 0.004 286.32)` |
| `--destructive` | `oklch(0.577 0.245 27.325)` |

---

## Radius

Derivado da variável base `--radius: 0.875rem` (14px).

| Token | Cálculo | Valor aproximado | Uso |
|---|---|---|---|
| `--radius-sm` | `--radius * 0.6` | ~8px | Badges, tags, chips |
| `--radius-md` | `--radius * 0.8` | ~11px | Inputs, selects, botões |
| `--radius-lg` | `--radius` | 14px | Cards, painéis |
| `--radius-xl` | `--radius * 1.4` | ~20px | Modais, drawers |
| `--radius-2xl` | `--radius * 1.8` | ~25px | Superfícies grandes |
| `--radius-3xl` | `--radius * 2.2` | ~31px | Hero sections |
| `--radius-4xl` | `--radius * 2.6` | ~36px | Uso excepcional |

---

## Regras de uso

### Hierarquia de superfícies (do mais profundo ao mais elevado)

```
--background          ← base da aplicação
  └── --sidebar       ← painel lateral
  └── --card          ← cards e painéis de conteúdo
        └── --popover ← dropdowns, tooltips, menus flutuantes
              └── --muted / --accent  ← elementos internos em hover
```

### Texto sobre cada superfície

| Superfície | Token de texto obrigatório |
|---|---|
| `--background` | `--foreground` |
| `--card` | `--card-foreground` |
| `--popover` | `--popover-foreground` |
| `--muted` | `--muted-foreground` (secundário) ou `--foreground` (principal) |
| `--primary` (botão) | `--primary-foreground` |
| `--destructive` (botão) | branco (`--foreground` escopo root) |
| `--sidebar` | `--sidebar-foreground` |

### Anti-patterns de cor

- **PROIBIDO**: usar `--card` como fundo de página — é para elementos elevados, não base
- **PROIBIDO**: usar `--muted-foreground` em texto de ação — é para texto decorativo/secundário
- **PROIBIDO**: usar `--primary` como cor de texto corrido — é exclusivo para elementos interativos
- **PROIBIDO**: empilhar `--card` sobre `--card` sem diferenciação — use `--muted` para o nível interno
- **PROIBIDO**: aplicar `--destructive` em avisos — destructive é para ações irreversíveis, não warnings
