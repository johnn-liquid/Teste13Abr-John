# Artifact — Graph

Atualizado em: 7 de abril de 2026
Group: Artefatos
Status: Draft

## O que é

Visualização de relacionamentos entre entidades do mercado — SPEs, incorporadoras, compradores, bancos e fundos. Representa estrutura relacional: quem está conectado a quem, e de que forma.

## Por que existe

O mercado imobiliário e financeiro opera em redes de relacionamento — uma incorporadora controla várias SPEs, um fundo detém participações em múltiplos empreendimentos, um comprador tem vínculos com garantidores. Essas estruturas não podem ser comunicadas por tabelas ou gráficos — exigem um formato que mostre conexão, não comparação.

## Como existe

### Tipos

| Tipo | Quando usar |
| --- | --- |
| `ownership` | Estrutura de propriedade e controle (SPE → incorporadora → fundo) |
| `credit` | Relações de crédito, garantia e dívida entre entidades |
| `network` | Mapa amplo de relacionamentos de uma entidade com seu ecossistema |

### Comportamento

- Nós são entidades clicáveis — o clique abre o Entity Card da entidade selecionada
- Arestas são relacionamentos tipados com rótulo
- Interativo: zoom, drag, filtro por tipo de relação
- Exportável como imagem

### Estados

- `loading` — skeleton do grafo enquanto dados são carregados
- `empty` — entidade sem conexões conhecidas (mensagem contextual)
- `partial` — conexões parcialmente disponíveis, dados incompletos do conector (indicador visual)

### Regra de quando usar

Use Graph quando o dado central é **a relação entre entidades**, não os atributos de uma entidade isolada. A pergunta-gatilho: *"quem está conectado a quem nesse contexto?"*

**Graph vs. Chart:** Chart mostra como um valor evolui ou se distribui. Graph mostra como entidades se relacionam. Se não há nós e arestas, não é Graph.

**Graph vs. Datatable:** Datatable compara atributos de múltiplas entidades em paralelo. Graph mostra as conexões estruturais entre elas.

**Graph vs. Card:** Card descreve uma entidade. Graph mostra o ecossistema ao redor dela.

**Não use** quando há apenas uma entidade no contexto — use Card.

**Não use** quando a relação pode ser comunicada em texto ou lista simples.

**Não use** quando os dados do conector não incluem relacionamentos estruturados entre entidades.

## Onde é usado

Painel split ou dashboard dominante. Nunca inline no chat — a densidade visual exige espaço adequado para ser legível.

## Quem usa

Gerado pelos Entity Agents, que acumulam conhecimento sobre entidades e seus relacionamentos ao longo do tempo. Consumido por analistas e gestores para entender estruturas de propriedade, risco de crédito e composição de carteira.