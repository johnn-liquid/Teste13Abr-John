# Componente — Agent Icon

## Nome canônico
`AgentIcon`

## Descrição
Representação visual do agente. Identidade iconográfica usada no ChatHeader, nas mensagens do agente (Text role=agent) e em listagens de agentes disponíveis. É a "face" do agente na interface.

---

## Anatomia

```
AgentIcon
├── IconContainer       ← círculo ou quadrado arredondado
│   └── IconContent     ← imagem, SVG ou iniciais
└── StatusDot (opcional) ← indicador de status online/offline/thinking
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `sm` | 24×24px — em mensagens inline, badges |
| `md` | 32×32px — em listas de agentes, sidebars |
| `lg` | 40×40px — no ChatHeader |
| `xl` | 56×56px — em cards de apresentação do agente |

---

## Estados obrigatórios

| Estado | StatusDot |
|---|---|
| `online` | Ponto verde (`--chart-2`) no canto inferior direito |
| `thinking` | Ponto animado pulsante (`--primary`) |
| `offline` | Ponto cinza (`--muted-foreground`) |
| `error` | Ponto vermelho (`--destructive`) |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `size` | `sm \| md \| lg \| xl` | `md` | Tamanho do ícone |
| `src` | `string` | — | URL da imagem do agente |
| `name` | `string` | obrigatório | Nome do agente (usado como alt e para gerar iniciais) |
| `status` | `online \| thinking \| offline \| error` | — | Se presente, exibe StatusDot |
| `shape` | `circle \| square` | `circle` | Forma do container |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background (sem imagem) | `--muted` |
| Iniciais (sem imagem) | `--muted-foreground` + `label-3` |
| Borda | `--border` |
| Radius circle | `--radius-full` |
| Radius square | `--radius-md` (sm/md), `--radius-lg` (lg/xl) |
| StatusDot online | `oklch(0.623 0.214 259.815)` via `--chart-2` |
| StatusDot thinking | `--primary` + animação pulse `--duration-deliberate` |
| StatusDot offline | `--muted-foreground` |
| StatusDot error | `--destructive` |

---

## Regras de uso

- **Use AgentIcon** para identificar visualmente qualquer agente no produto
- O `name` é sempre obrigatório — garante alt text e fallback com iniciais
- Quando `src` não está disponível, exibe as duas primeiras letras do `name`
- StatusDot aparece apenas quando o status é relevante para o contexto (chat ativo)
- Na variante `sm`, StatusDot é suprimido por ser pequeno demais para legibilidade

---

## Anti-patterns

- **PROIBIDO**: usar AgentIcon sem o prop `name`
- **PROIBIDO**: exibir StatusDot na variante `sm`
- **PROIBIDO**: usar cores fora dos tokens para personalizar o AgentIcon por agente
- **PROIBIDO**: AgentIcon sem fallback de iniciais

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
