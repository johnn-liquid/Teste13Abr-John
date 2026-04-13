# Componente — Spinner

## Nome canônico
`Spinner`

## Descrição
Indicador de carregamento circular. NÃO é shadcn nativo — criado para uso em Button loading, Chat loading, e contextos de processamento. Diferente do Skeleton: Skeleton substitui conteúdo; Spinner indica ação em andamento.

---

## Anatomia

```
Spinner
└── SVG
    └── Circle (stroke circular com gap)
```

---

## Tamanhos

| Tamanho | Dimensões | Uso |
|---|---|---|
| `sm` | 16px × 16px | Dentro de botões compactos (Button size="sm") |
| `md` | 24px × 24px | Dentro de botões padrão, inputs de busca |
| `lg` | 32px × 32px | Páginas inteiras, modais |
| `xl` | 48px × 48px | Loading de página inteira (raro) |

---

## Estados

| Estado | Comportamento visual |
|---|---|
| `loading` | Spinner visível, animação de rotação contínua |
| `idle` | Spinner invisível (não renderizar) |

---

## Tokens aplicados

| Região | Token | Valor |
|---|---|---|
| Stroke color | `--muted-foreground` | cor da linha |
| Stroke width (sm) | 1.5px | espessura fina |
| Stroke width (md) | 2px | espessura padrão |
| Stroke width (lg+) | 2.5px | espessura grossa |
| Animation | rotação 360° contínua | rotação |
| Animation duration | `--duration-slow` (500ms) por volta | velocidade |
| Animation easing | `--ease-linear` | rotação constante |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `size` | `sm \| md \| lg \| xl` | `md` | Tamanho do spinner |
| `color` | `string` | `--muted-foreground` | Cor do stroke (raramente mudar) |
| `strokeWidth` | `number` | default por tamanho | Espessura do stroke |
| `className` | `string` | — | Classes adicionais |
| `aria-label` | `string` | `"Carregando"` | Accessibility label |

---

## Regras de uso

1. **Spinner ≠ Skeleton** — Spinner para ações processando; Skeleton para conteúdo carregando
2. **Nunca Spinner em página inteira** — para dados de página, usar Skeleton ou Banner com mensagem
3. **Spinner em Button** — substitui ícone da esquerda quando `loading={true}`
4. **Spinner em Chat** — usar quando agente está "digitando" (três pontos animados é preferível)
5. **Accessibility** — sempre incluir `aria-label="Carregando"` ou `aria-busy="true"` no parent
6. **Color override** — raramente mudar cor; padrão é `--muted-foreground`
7. **Animation** — respeitar `prefers-reduced-motion` (substituir por texto "Carregando..." se preferir)

---

## Exemplo de uso

```jsx
// Spinner padrão
<Spinner size="md" />

// Spinner em Button (via loading state)
<Button loading={isLoading}>
  {isLoading ? <Spinner size="sm" /> : <SaveIcon />}
  Salvar
</Button>

// Spinner em contexto inline
<div style={{ display: 'flex', alignItems: 'center', gap: '8px' }}>
  <Spinner size="sm" />
  <span>Processando...</span>
</div>

// Spinner em modal
<Modal>
  <ModalContent>
    <Spinner size="lg" />
    <p>Carregando seus dados...</p>
  </ModalContent>
</Modal>

// Skeleton vs Spinner em Chat
// Chat digitando → Suggestions com 3 pontos pulsantes (não Spinner)
// Action processando → Spinner no botão
// Dados carregando → Skeleton na área de conteúdo
```

---

## Composição com Button

O Spinner é usado automaticamente quando Button tem `loading={true}`:

```jsx
// Implementação conceitual do Button com Spinner
function Button({ loading, children, leftIcon, ...props }) {
  return (
    <button
      {...props}
      disabled={loading || props.disabled}
      style={{ opacity: loading ? 0.7 : 1 }}
    >
      {loading ? <Spinner size="sm" /> : leftIcon}
      {children}
    </button>
  );
}
```

---

## shadcn base

Não é shadcn nativo. Inspirado em: [Radix UI Progress](https://www.radix-ui.com/docs/primitives/components/progress) (estrutura SVG) e práticas de loading states.

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
