# Componente — Artifact Link

## Nome canônico
`ArtifactLink`

## Descrição
Link externo referenciado pela IA. Exibição de URL com preview rico (título, descrição, domínio, imagem). Usado quando o agente cita uma fonte ou recomenda um recurso externo.

---

## Anatomia

```
ArtifactLink
├── ArtifactHeader          ← label "Fonte" ou "Link"
└── LinkCard                ← card clicável
    ├── LinkImage (opcional) ← OG image do destino
    ├── LinkContent
    │   ├── LinkDomain      ← domínio da URL (ex: "github.com")
    │   ├── LinkTitle       ← título da página
    │   └── LinkDescription ← descrição curta (OG description)
    └── LinkArrow           ← ícone de link externo
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `rich` | Com imagem + título + descrição (preview completo) |
| `compact` | Apenas domínio + título (sem imagem) |
| `inline` | Texto com ícone de link externo — dentro de Text do agente |

---

## Dados obrigatórios vs opcionais

| Campo | Obrigatoriedade |
|---|---|
| `url` | Obrigatório |
| `title` | Obrigatório |
| `domain` | Obrigatório |
| `description` | Opcional |
| `image` | Opcional |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `loading` | Skeleton do LinkCard |
| `populated` | Card com preview |
| `no-preview` | Apenas URL + domínio sem preview (fallback) |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `url` | `string` | obrigatório | URL de destino |
| `title` | `string` | obrigatório | Título do link |
| `domain` | `string` | obrigatório | Domínio para exibição |
| `description` | `string` | — | Descrição do destino |
| `image` | `string` | — | URL da OG image |
| `variant` | `rich \| compact \| inline` | `rich` | Modo de exibição |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background LinkCard | `--card` |
| Borda LinkCard | `--border` |
| Hover LinkCard | `--accent` background |
| LinkDomain | `--muted-foreground` + `label-3` |
| LinkTitle | `--card-foreground` + `label-2` |
| LinkDescription | `--muted-foreground` + `body-sm` |
| LinkArrow | `--muted-foreground` |
| Radius LinkCard | `--radius-lg` |
| Shadow | `--shadow-sm` |
| Transição hover | `--duration-fast` + `--ease-out` |

---

## Regras de uso

- **Use ArtifactLink** quando o agente citar uma fonte externa
- LinkCard abre o link em nova aba — nunca na mesma aba
- Máximo de 3 ArtifactLinks em sequência — mais que isso use ArtifactList

---

## Anti-patterns

- **PROIBIDO**: exibir URL crua sem LinkCard (nenhum link deve aparecer como texto puro)
- **PROIBIDO**: abrir link externo na mesma aba
- **PROIBIDO**: mais de 3 ArtifactLinks em sequência sem agrupar em ArtifactList

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
