# Registro de Conversa — Fase de Intenção e Descoberta

**Projeto:** Análise documental NovaTech
**Perfil do usuário:** Product Specialist
**Data:** 06/06/2026

---

## Interação 1 — Resumo: FAQ-atendimento

**Prompt do usuário:**

> Sou um Product Specialist e estou conduzindo uma fase de intenção e descoberta, preciso que seja criado um resumo em arquivo Markdown com base no documento principal.
>
> Entradas:
> - Documento-fonte: FAQ-atendimento.md
> - Nome do arquivo de saída: FAQ-atendimento-resumido.md
>
> Instruções obrigatórias:
> - Preserve apenas três seções no arquivo final: Título, Metadados e Resumo.
> - Não inclua introdução, conclusão, exemplos extras ou comentários.
> - O Resumo deve: ter entre 4 e 8 linhas; destacar temas cobertos; manter linguagem objetiva em pt-BR.
> - Saída em Markdown.
>
> Critério de aceitação: Se houver qualquer seção além de Título, Metadados e Resumo, a saída está incorreta.

**Ação executada:**

Leitura do arquivo `FAQ-atendimento.md` (localizado em `Prática 1/`) e geração do arquivo `FAQ-atendimento-resumido.md` com as três seções obrigatórias: Título, Metadados e Resumo (7 linhas). O resumo cobre devoluções/cargas perigosas, frete especial (PROC-042), tiers de clientes, seguro de carga, SLAs, cargas danificadas e autonomia do atendente para descontos.

**Arquivo gerado:** `FAQ-atendimento-resumido.md`

---

## Interação 2 — Mapa de temas e hipóteses de gaps

**Prompt do usuário:**

> Sou um Product Specialist e estou conduzindo uma fase de intenção e descoberta de um novo projeto e preciso que seja criado um documento de temas cobertos e hipóteses de gaps.
>
> Objetivo: Gerar um mapa de temas cobertos e hipóteses de gaps usando os 5 documentos de resumo como dados de entrada:
> - FAQ-atendimento-resumido.md
> - POL-001-politica-devolucao-resumido.md
> - PROC-042-frete-especial-v1-resumido.md
> - PROC-042-v2-frete-especial-revisado1-resumido.md
> - SLA-2024-tabela-sla-clientes-resumido.md
>
> Instruções de execução:
> - Liste os temas principais cobertos por cada documento.
> - Identifique possíveis conflitos e sobreposição entre documentos.
> - Levante hipóteses de gaps de informação que podem impactar atendimento, SLA, frete e devolução.
> - Para o mapa de temas cobertos listar o tema, documento de referência e status de cobertura do tema.
> - Para cada hipótese de gap, indique: evidência do gap, impacto positivo e negativo na operação e perguntas para descoberta.
> - Não inventar fatos além do que estiver nas entradas.
>
> Formato de saída: Seção 1 (Dados de Entrada) / Seção 2 (Mapa de temas cobertos) / Seção 3 (Conflitos) / Seção 4 (Hipóteses de gaps em tabela) / Seção 5 (Resumo executivo 6–10 linhas).
>
> Destino: entrega-final.md com título "Tarefa 1 - Etapa 1 | Mapa de temas e hipóteses de gaps".

**Ação executada:**

Leitura simultânea dos 5 arquivos de resumo. Geração de análise integrada com 25 temas mapeados, 5 conflitos detalhados e 8 hipóteses de gap, todas com evidência direta nos documentos de entrada, impacto positivo/negativo e perguntas de descoberta. Resumo executivo de 8 linhas destacando os dois principais eixos de risco: coexistência sem hierarquia das versões PROC-042 e FAQ contradizendo normativos em pontos críticos.

**Arquivo gerado:** `entrega-final.md`

**Conflitos identificados:**

| # | Conflito | Documentos | Severidade |
|---|----------|-----------|------------|
| 1 | Multiplicadores regionais divergentes | PROC-042 v1 × v2 | Alto |
| 2 | Prazo adicional de entrega (+2 vs +3 dias) | PROC-042 v1 × v2 | Médio |
| 3 | Descontos de volume — negociação vs. automático | PROC-042 v1 × v2 | Alto |
| 4 | Devolução de cargas perigosas — exceção vs. vedação | FAQ × POL-001 | Alto |
| 5 | Autonomia do atendente para desconto | FAQ × PROC-042 v2 | Médio |

**Gaps identificados:**

| # | Gap | Impacto |
|---|-----|---------|
| G1 | PROC-043 ausente (frete cargas perigosas) | Alto |
| G2 | Seguro de carga sem documento normativo | Alto |
| G3 | Ausência de critério formal de obsolescência PROC-042 | Crítico |
| G4 | Carga danificada em trânsito sem procedimento normativo | Alto |
| G5 | SLA de tracking por rota não formalizado | Médio |
| G6 | Critérios de classificação de tier não detalhados | Médio |
| G7 | Procedimento de crédito por violação de SLA ausente | Alto |
| G8 | Frete expresso para cargas perigosas sem procedimento normativo | Médio |

---

## Interação 3 — Extração desta conversa

**Prompt do usuário:**

> Extraia essa conversa e gere um arquivo Markdown com o nome: promp-entrada1.1v2.md

**Ação executada:**

Leitura do arquivo de referência `promp-entrada1.1md` (localizado em `Prática 1/`) para identificar o padrão de formatação. Geração do registro desta sessão no mesmo formato: cabeçalho, interações com prompt, ação executada e arquivos gerados.

**Arquivo gerado:** `promp-entrada1.1v2.md`

---

## Arquivos Gerados nesta Sessão

| Arquivo | Origem | Tipo |
|---------|--------|------|
| `FAQ-atendimento-resumido.md` | FAQ-atendimento.md | Resumo informal |
| `entrega-final.md` | 5 resumos (FAQ + POL-001 + PROC-042 v1 + v2 + SLA-2024) | Mapa de temas e hipóteses de gaps |
| `promp-entrada1.1v2.md` | Esta conversa | Registro de sessão |
