# Artifact — Datatable

Atualizado em: 6 de abril de 2026
Group: Artefatos
Status: Draft

## O que é

Artefato tabular para conjuntos de dados estruturados com múltiplos atributos. É o formato ideal para carteiras de crédito, comparativos de empreendimentos, históricos de operações.

## Por que existe

Analistas de crédito e gestores de carteira trabalham com dados tabulares. O datatable permite que outputs de skill sejam navegáveis, filtráveis e exportáveis — transformando dados brutos em ferramenta de trabalho.

## Como existe

**Funcionalidades:**

- Ordenação por coluna
- Filtros por coluna (texto, range numérico, data)
- Busca global na tabela
- Paginação para datasets grandes
- Seleção de linhas para ações em lote
- Exportação CSV/XLSX
- Freeze de colunas (colunas-chave sempre visíveis)

**Comportamento:**

- Colunas configuráveis (mostrar/ocultar) para respeitar o limite atencional do usuário
- Paginação obrigatória para datasets grandes (quebra a carga cognitiva em *chunks* processáveis, diferente do scroll infinito da lista)
- Células podem conter valores formatados (moeda, percentual, data, badge de status)
- Linha clicável para abrir detalhes em painel lateral ou novo chat
- Pode ser gerado a partir de conectores (dados de carteira) ou de análise de documentos

### Variantes de Densidade

- `spacious` (padrão): células respiradas. Focado em baixa ansiedade na leitura e análises lentas onde o usuário confere linha a linha.
- `compact`: redução de padding e fonte `sm` para máxima densidade de dados na tela. Usado apenas quando a funcionalidade atua como uma "mesa de controle" operacional intensiva.

### Regra de quando usar (Tabela vs. Lista)

- Utilize para **> 3 atributos principais**, pois o alinhamento de grid (gestalt da continuidade) permite o escaneamento eficiente em "F" (F-pattern reading).
- Utilize sempre que for necessário comparar características estritas entre si ou forçar ordenação cruzada (coisas que listas quebram mentalmente).
- Evite tabelas no meio do Chat caso só tenham 1-2 colunas (neste caso opte por `artifact-list`).

**Exemplos de uso:**

- Carteira de crédito imobiliário (CPF, valor, status, banco)
- Comparativo de empreendimentos por critérios de viabilidade
- Histórico de consultas de birô da conta

## Onde é usado

Layout dashboard dominante ou split. Raramente inline no chat (apenas tabelas pequenas).

## Quem usa

Gerado pelos Skill Agents a partir de conectores ou documentos. Consumido por analistas e gestores de carteira.