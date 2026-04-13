# Tokens — Spacing

> Base: escala de 4px. Cada step é um múltiplo ou submúltiplo de 4px.
> Toda decisão de espaçamento — padding, margin, gap, inset — deve referenciar um token.
> Valores em px soltos são proibidos.

---

## Regra absoluta

**Nenhum valor de espaçamento pode ser hardcoded.
Use sempre `var(--space-N)` ou o equivalente de classe Tailwind mapeado ao token.**

---

## Escala de espaçamento

| Token | Valor | Uso típico |
|---|---|---|
| `--space-0` | 0px | Reset, sem espaço |
| `--space-1` | 4px | Gap mínimo entre elementos inline (ícone + label), padding de badge |
| `--space-1-5` | 6px | Padding interno de chips, micro-gaps |
| `--space-2` | 8px | Padding de botão compacto, gap entre itens de lista densa |
| `--space-3` | 12px | Padding interno de input, gap entre label e campo |
| `--space-4` | 16px | Padding padrão de card, gap entre itens de menu |
| `--space-5` | 20px | Padding de seção compacta, gap entre grupos de campo |
| `--space-6` | 24px | Padding de painel, gap entre cards |
| `--space-8` | 32px | Espaço entre seções dentro de uma página |
| `--space-10` | 40px | Espaço entre blocos maiores |
| `--space-12` | 48px | Padding de layout (inset lateral em desktop) |
| `--space-16` | 64px | Espaço de separação de regiões principais |

---

## Escala semântica (mapeamento de intenção)

| Token semântico | Valor base | Uso |
|---|---|---|
| `--space-component-xs` | `--space-1` (4px) | Padding interno de badge, tag, chip |
| `--space-component-sm` | `--space-2` (8px) | Padding interno de botão compacto |
| `--space-component-md` | `--space-3` (12px) | Padding interno de input, select |
| `--space-component-lg` | `--space-4` (16px) | Padding interno de card, painel |
| `--space-layout-sm` | `--space-6` (24px) | Gap entre cards no grid |
| `--space-layout-md` | `--space-8` (32px) | Gap entre seções |
| `--space-layout-lg` | `--space-12` (48px) | Inset lateral de página |
| `--space-layout-xl` | `--space-16` (64px) | Separação de regiões principais |

---

## Regras de uso

### Hierarquia de espaçamento

A densidade de espaço comunica hierarquia. Elementos mais próximos pertencem ao mesmo grupo; espaços maiores separam grupos distintos.

```
Viewport
└── --space-layout-lg (48px) inset lateral
      └── seções: gap --space-layout-md (32px)
            └── cards: gap --space-layout-sm (24px)
                  └── conteúdo interno: --space-component-lg (16px)
                        └── labels e campos: --space-component-md (12px)
                              └── ícone + texto: --space-component-sm (8px)
```

### Espaçamento em componentes interativos

| Componente | Padding horizontal | Padding vertical |
|---|---|---|
| Botão (default) | `--space-4` (16px) | `--space-2` (8px) |
| Botão (compacto) | `--space-3` (12px) | `--space-1-5` (6px) |
| Input | `--space-3` (12px) | `--space-2` (8px) |
| Card | `--space-4` (16px) | `--space-4` (16px) |
| Item de menu | `--space-3` (12px) | `--space-2` (8px) |
| Badge / Tag | `--space-2` (8px) | `--space-1` (4px) |

### Espaçamento no Chat

| Elemento | Espaço |
|---|---|
| Gap entre mensagens | `--space-3` (12px) |
| Padding da bubble de mensagem | `--space-3` (12px) horizontal, `--space-2` (8px) vertical |
| Padding do container de chat | `--space-4` (16px) |
| Gap entre sugestões | `--space-2` (8px) |

---

## Anti-patterns de espaçamento

- **PROIBIDO**: usar valores fora da escala (`10px`, `14px`, `18px`, `22px`) — aproxime para o token mais próximo
- **PROIBIDO**: espaçamento zero entre elementos interativos — área de toque mínima exige pelo menos `--space-2`
- **PROIBIDO**: usar espaçamento maior que `--space-6` dentro de um componente — use `--space-layout-*` apenas para layout de página
- **PROIBIDO**: gaps inconsistentes entre itens de uma mesma lista — escolha um gap e aplique a todos
