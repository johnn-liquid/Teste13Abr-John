# Tokens — Typography

> Fonte: variáveis extraídas dos Figmas de referência (`.context/figmas.md`).
> Famílias: `Geist` — fonte única para interface, headings e labels.
> Toda referência tipográfica deve usar tokens semânticos — tamanhos em px soltos são proibidos.

---

## Regra absoluta

**Nenhum valor de tipografia pode ser hardcoded (px, rem, weight numérico solto).
Toda propriedade tipográfica referenciada em um componente deve usar token semântico.**

---

## Famílias tipográficas

| Token | Família | Uso |
|---|---|---|
| `--font-sans` | Geist | Interface completa — headings, corpo, inputs, labels, código |
| `--font-heading` | Geist | Headings e títulos (unificado) |

---

## Escala de tamanhos

| Token | Tamanho | Line Height | Uso |
|---|---|---|---|
| `--text-xs` | 11px | 16px | Micro-captions, metadados densos |
| `--text-sm` | 12px | 16px | Badges, timestamps, labels menores |
| `--text-base` | 14px | 20px | Corpo de texto, mensagens de chat, conteúdo principal |
| `--text-lg` | 16px | 24px | Subtítulos de seção, leads |
| `--text-xl` | 18px | 28px | Títulos de componente, KPI values |
| `--text-2xl` | 20px | 30px | Títulos de seção grandes |
| `--text-3xl` | 24px | 32px | Títulos de página, headings principais |

---

## Escala de pesos

| Token | Valor | Uso |
|---|---|---|
| `--font-light` | 300 | Texto decorativo, display em tamanhos grandes |
| `--font-regular` | 400 | Corpo, placeholder, mensagens |
| `--font-medium` | 500 | Labels, itens de menu, botões secondary |
| `--font-semibold` | 600 | Labels de destaque, headings de seção |
| `--font-bold` | 700 | Headings principais, KPI values, CTAs |

---

## Roles tipográficos canônicos

Cada papel define família + tamanho + peso + line-height + letter-spacing obrigatórios.

### Headings (Geist)

| Role | Família | Tamanho | Peso | Line Height | Letter Spacing | Uso |
|---|---|---|---|---|---|---|
| `heading-h4` | Geist | 24px | Bold (700) | 32px | -0.2px | Título principal de página |
| `heading-section` | Geist | 20px | SemiBold (600) | 30px | -0.16px | Título de seção dentro de página |
| `heading-card` | Geist | 16px | SemiBold (600) | 24px | -0.12px | Título de card ou painel |

### Labels (Geist)

| Role | Família | Tamanho | Peso | Line Height | Letter Spacing | Uso |
|---|---|---|---|---|---|---|
| `label-1` | Geist | 14px | Medium (500) | 20px | -0.18px | Labels principais, itens de navegação ativos |
| `label-2` | Geist | 12px | Medium (500) | 16px | -0.16px | Labels secundários, descrições de campo |
| `label-3` | Geist | 11px | SemiBold (600) | 16px | -0.12px | Micro-labels, badges, timestamps |

### Body (Geist)

| Role | Família | Tamanho | Peso | Line Height | Uso |
|---|---|---|---|---|---|
| `body-lg` | Geist | 16px | Regular (400) | 24px | Corpo principal, mensagens de chat |
| `body-md` | Geist | 14px | Regular (400) | 20px | Descrições, conteúdo de suporte |
| `body-sm` | Geist | 12px | Regular (400) | 16px | Notas, captions, metadados |

### Código / Mono (Geist Mono)

| Role | Família | Tamanho | Peso | Uso |
|---|---|---|---|---|
| `code-base` | Geist Mono | 14px | Regular (400) | Blocos de código inline e em artefatos |
| `code-sm` | Geist Mono | 12px | Regular (400) | Labels técnicos, tokens, variáveis |

---

## Regras de uso

### Hierarquia em uma tela

```
heading-h4         ← título da página (apenas um por tela)
  └── heading-section  ← título de área/seção
        └── heading-card    ← título de card ou painel
              └── label-1   ← item navegável, ação com nome
                    └── body-lg / body-md  ← conteúdo
                          └── body-sm / label-3  ← metadados
```

### Quando usar Geist
O Geist é a fonte única para todo o sistema. Use pesos menores para corpo e pesos maiores (SemiBold/Bold) com letter-spacing negativo para headings e labels de destaque.

### Letter spacing negativo
Letter spacing negativo discreto é recomendado para Geist em tamanhos acima de 16px para manter a personalidade técnica do sistema.

---

## Anti-patterns tipográficos

- **PROIBIDO**: usar tamanhos fora da escala — use sempre o token correspondente
- **PROIBIDO**: dois `heading-h4` na mesma tela — há apenas um título de página por viewport
- **PROIBIDO**: tamanhos fora da escala (`15px`, `17px`, `22px`) — use o tamanho mais próximo da escala
