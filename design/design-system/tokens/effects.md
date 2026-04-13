# Tokens — Effects

> Cobre: border-radius · sombras · blur · motion (duração + easing).
> Todos derivados das variáveis extraídas dos Figmas de referência + defaults criados para motion.
> Nenhum efeito visual pode ser aplicado com valor hardcoded.

---

## Regra absoluta

**Radius, shadow, blur e motion devem sempre referenciar um token semântico.
Valores arbitrários como `border-radius: 10px` ou `transition: 0.2s ease` são proibidos.**

---

## Border Radius

Base: `--radius: 0.875rem` (14px). Todos os valores derivam desta variável.

| Token | Cálculo | Valor aproximado | Uso |
|---|---|---|---|
| `--radius-none` | 0 | 0px | Sem arredondamento — bordas retas intencionais |
| `--radius-sm` | `calc(var(--radius) * 0.6)` | ~8px | Badges, tags, chips, tooltips |
| `--radius-md` | `calc(var(--radius) * 0.8)` | ~11px | Inputs, selects, botões, itens de menu |
| `--radius-lg` | `var(--radius)` | 14px | Cards, painéis, dropdowns |
| `--radius-xl` | `calc(var(--radius) * 1.4)` | ~20px | Modais, drawers, sheets |
| `--radius-2xl` | `calc(var(--radius) * 1.8)` | ~25px | Superfícies de destaque |
| `--radius-3xl` | `calc(var(--radius) * 2.2)` | ~31px | Containers hero, banners |
| `--radius-4xl` | `calc(var(--radius) * 2.6)` | ~36px | Uso excepcional — apenas com aprovação |
| `--radius-full` | 9999px | Circular | Avatares, botões pill, progress circular |

### Regras de radius

- **Cards e painéis**: sempre `--radius-lg`
- **Inputs e botões**: sempre `--radius-md`
- **Badges e tags**: sempre `--radius-sm`
- **Modais e drawers**: sempre `--radius-xl`
- **Avatares e ícones circulares**: sempre `--radius-full`
- **PROIBIDO**: misturar radius em elementos do mesmo nível hierárquico (ex: dois cards com radius diferentes)

---

## Sombras (Elevation)

Sistema de elevação em 5 níveis + flying para elementos flutuantes.

| Token | Composição | Uso |
|---|---|---|
| `--shadow-xs` | `blur: 2px, spread: 0, rgba(0,0,0,0.05)` | Separação sutil — hover em items de lista |
| `--shadow-sm` | `blur: 2px + 3px (compound)` | Cards sobre background, inputs em focus |
| `--shadow-md` | `blur: 4px + 6px (compound)` | Cards elevados, dropdowns |
| `--shadow-lg` | `blur: 6px + 15px (compound)` | Popovers, sidebars abertas, painéis de detalhe |
| `--shadow-flying` | `blur: 40px, rgba(0,0,0,0.4)` | Modais, dialogs, sheets — elementos que flutuam sobre tudo |

### Semântica de elevação

| Nível | Token de sombra | Quando usar |
|---|---|---|
| Nível 0 (base) | nenhuma | Fundo da página, sidebar |
| Nível 1 (card) | `--shadow-sm` | Cards, painéis embutidos |
| Nível 2 (dropdown) | `--shadow-md` | Menus, popovers, tooltips |
| Nível 3 (overlay leve) | `--shadow-lg` | Side panels, contextuais |
| Nível 4 (modal) | `--shadow-flying` | Modais, dialogs, drawers completos |

### Regras de sombra

- A sombra aumenta com a elevação — nunca aplique `--shadow-flying` em cards inline
- Combinação de sombra + border (`--border`) é permitida para level 1
- A partir do level 2, a sombra substitui a border como delimitador visual

---

## Blur (Backdrop)

| Token | Valor | Uso |
|---|---|---|
| `--blur-md` | `backdrop-filter: blur(12px)` | Fundo de modais, sidebars com efeito glassmorphism, overlays |

### Regras de blur

- Usar `--blur-md` apenas em elementos com `background-color` semi-transparente
- Nunca aplicar blur em elementos sem posicionamento absoluto/fixed
- Não usar blur em elementos dentro do fluxo de texto

---

## Motion

Tokens de duração e easing criados para o sistema (não presentes nos Figmas originais).

### Durações

| Token | Valor | Uso |
|---|---|---|
| `--duration-instant` | 0ms | Mudanças de estado sem animação (preferência de usuário: reduzir motion) |
| `--duration-fast` | 100ms | Feedbacks imediatos — hover, focus ring, ripple |
| `--duration-normal` | 200ms | Transições de estado padrão — cor, opacity, border |
| `--duration-moderate` | 300ms | Expansões e colapsos — accordions, dropdowns, toasts |
| `--duration-slow` | 500ms | Entradas de componentes complexos — modais, sheets, sidebars |
| `--duration-deliberate` | 700ms | Animações de conteúdo gerado pela IA — typing effect, stream |

### Easings

| Token | Valor CSS | Uso |
|---|---|---|
| `--ease-linear` | `linear` | Progress bars, loading spinners contínuos |
| `--ease-out` | `cubic-bezier(0, 0, 0.2, 1)` | Entradas de elementos (aparecem suavemente) |
| `--ease-in` | `cubic-bezier(0.4, 0, 1, 1)` | Saídas de elementos (somem acelerando) |
| `--ease-in-out` | `cubic-bezier(0.4, 0, 0.2, 1)` | Transições de estado (hover, focus, expand) |
| `--ease-spring` | `cubic-bezier(0.175, 0.885, 0.32, 1.275)` | Feedbacks lúdicos — confirmações, success states |

### Regras de motion

```
Entrada de elemento  → --ease-out + --duration-moderate ou --duration-slow
Saída de elemento    → --ease-in + --duration-fast ou --duration-normal
Transição de estado  → --ease-in-out + --duration-normal
Hover / Focus        → --ease-out + --duration-fast
Progress / Loading   → --ease-linear
```

### Acessibilidade de motion

- Quando `prefers-reduced-motion: reduce` estiver ativo, use `--duration-instant` para todas as transições
- Animações decorativas devem ser suprimidas completamente com `prefers-reduced-motion`
- Animações funcionais (loading spinners, progress) podem manter duração mínima com motion reduzido

---

## Anti-patterns de efeitos

- **PROIBIDO**: usar `border-radius: 10px` solto — use `--radius-md`
- **PROIBIDO**: `box-shadow: 0 2px 4px rgba(0,0,0,0.1)` inline — use `--shadow-sm`
- **PROIBIDO**: `transition: all 0.3s ease` — especifique a propriedade e use os tokens de motion
- **PROIBIDO**: `backdrop-filter: blur(8px)` — use `--blur-md` (12px é o valor do sistema)
- **PROIBIDO**: aplicar `--shadow-flying` em cards ou itens de lista
- **PROIBIDO**: animar `width` ou `height` sem `will-change` — causa reflow, use `transform` ou `max-height`
