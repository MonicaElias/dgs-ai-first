# Prompt de Entrada 1.3 — Revisão e Refinamento do PRD para Pipeline de RAG

## Metadados

- **Data:** 09/06/2026
- **Papel do usuário:** Product Specialist
- **Fase do projeto:** Intenção e Descoberta
- **Empresa:** NovaTech
- **Artefatos gerados:** `feedback.md`, `PRD-NovaTech-Assistente-RAG-versao2.docx`

---

## Interação 1 — Geração do Feedback de Revisão

### Prompt do Usuário

**Papel solicitado ao assistente:** Revisor de produto

**Inputs fornecidos:**
- `PRD-NovaTech-Assistente-RAG-versao1.docx`

**Objetivo:**
Analisar o documento de Especificação de Requisitos do Produto (PRD) e identificar gaps, ambiguidades e inconsistências.

**Regras:**
- Refinar com base no feedback
- Não utilizar contexto externo ao documento e ao projeto NovaTech
- Verificar ambiguidades, inconsistências e gaps
- Verificar falta de critérios de aceite

**Estrutura da saída de dados:**
1. Item e Problema relacionado
2. Evidência do problema
3. Impactos e Riscos
4. Feedback sobre a especificação

**Saída dos dados:** Arquivo Markdown com o nome `feedback.md`

---

### Resposta do Assistente

#### Abordagem

O assistente leu o conteúdo do PRD na íntegra a partir da síntese disponível em `promp-entrada1.3versao1.md` (o arquivo `.docx` é binário e não legível diretamente). Analisou as 11 seções do PRD sistematicamente, cruzando seções entre si para identificar inconsistências internas, e avaliou a testabilidade de cada requisito pelo critério: "o QA conseguiria verificar este item com os dados fornecidos?"

#### Síntese dos Achados

**Total de achados: 15 itens**

| Categoria | Quantidade |
|---|---|
| Gaps Críticos | 6 |
| Ambiguidades | 4 |
| Inconsistências | 3 |
| Ausência de Critério de Aceite | 1 |
| Gaps (menor severidade) | 1 |

**Achados por seção:**

| # | Seção | Tipo | Severidade | Título Resumido |
|---|---|---|---|---|
| 01 | 2 — Escopo | Ambiguidade | Alta | Critério de elegibilidade no Confluence não testável |
| 02 | 2 — Escopo | Ambiguidade | Alta | "Versão mais recente" de planilhas sem critério técnico |
| 03 | 3 — Contradições | Gap Crítico | Alta | Regra de chamados requer integração com CRM não especificada |
| 04 | 2 e 3 | Inconsistência | Alta | PROC-042 v1 excluída da base mas tratada como regra de runtime |
| 05 | 4 — Falta de resposta | Inconsistência | Média | Canais de escalação com informações de contato incompletas |
| 06 | 5 — Atualização | Ambiguidade | Média | Prazo de 24h — horas corridas ou horas úteis? |
| 07 | 5 — Atualização | Gap Crítico | Média | Alerta de 60 dias sem destinatário, canal ou prazo de resposta |
| 08 | 6 — RF-05/RF-06 | Gap Crítico | Alta | Gatilho de detecção de novos documentos não especificado; sem RF de interface de curadoria |
| 09 | 7 — RNF-03 | Ambiguidade | Alta | Métrica de 90% de precisão sem metodologia de medição |
| 10 | 7 — RNF-05 | Gap Crítico | Média | Autenticação na "rede corporativa" não cobre home office |
| 11 | 9 — Rastreabilidade | Inconsistência | Média | Login do atendente exigido sem RF correspondente de autenticação |
| 12 | 7 e 9 | Gap Crítico | Alta | Logs com dados pessoais sem menção à LGPD |
| 13 | 8 — Critérios de Aceite | Ausência de AC | Alta | Nenhum critério de aceite para os 7 RNFs |
| 14 | 5 — Atualização | Gap | Média | Curadoria mensal sem dono, pauta ou critério de decisão |
| 15 | Produto (geral) | Gap Crítico | Alta | Interface do usuário completamente ausente no PRD |

**Pontos positivos identificados:**
- Curadoria de fontes bem fundamentada, com justificativas por documento
- Honestidade sobre gaps documentais (seção 4 lista temas sem cobertura)
- Pré-requisitos de lançamento claros (arquivamento PROC-042 v1)
- Estrutura dos RFs com ação, entrada e saída facilita escrita de casos de teste
- Mapeamento de riscos realista, incluindo risco comportamental (R-05)
- Objetivo do sistema conciso, orientado ao usuário e com meta quantificada

---

## Interação 2 — Geração do PRD Versão 2

### Prompt do Usuário

**Papel do usuário:** Product Specialist

**Inputs fornecidos:**
- `PRD-NovaTech-Assistente-RAG-versao1.docx`
- `feedback.md`

**Objetivo:**
Refinar o documento de especificação de requisitos do produto (PRD) com base no documento inicial e no feedback recebido. O escopo do projeto deve ser mantido.

Gerar nova versão da especificação (não técnica, mas precisa) que contenha:
1. Todo o contexto do projeto já especificado na versão 1 e refinado.
2. Mudanças aplicadas com justificativa baseada no feedback.

**Estrutura da saída de dados:**
1. Objetivo do Sistema (máximo 3 linhas)
2. Escopo das fontes de conhecimento indexadas e não indexadas
3. Regras para informações contraditórias
4. Regras para falta de respostas do assistente e comportamento esperado
5. Regras para requisitos de atualização da base de dados
6. Requisitos funcionais em tabela: ID, Nome do Requisito, Descrição (Ação, Entradas e Saídas)
7. Requisitos não funcionais em tabela: ID, Nome do Requisito Não Funcional, Categoria
8. Critérios de Aceitação e Regras de Negócio para os requisitos do item 6
9. Requisitos de rastreabilidade das respostas do sistema
10. Mapeamento dos Riscos
11. Resumo executivo (máximo 12 linhas)
12. Alterações aplicadas em formato de tabela

**Saída dos dados:** Documento Word com o nome `PRD-NovaTech-Assistente-RAG-versao2.docx`

---

### Resposta do Assistente

#### Abordagem

O assistente mapeou cada um dos 15 itens do `feedback.md` para alterações concretas no PRD, preservando todo o conteúdo original da versão 1. As alterações foram organizadas por seção e documentadas na seção 12 (tabela de alterações). Para geração do `.docx`, utilizou HTML intermediário convertido para Word via Microsoft Word COM (Word 16.0), contornando limitações de codificação de caracteres especiais em PowerShell 5.1.

#### Síntese das Alterações Aplicadas (v1 → v2)

| Feedback | Seção Afetada | Tipo | Alteração Aplicada |
|---|---|---|---|
| #01 | Seção 2 | Ambiguidade | Adicionada seção 2.2 com critério testável de "responsável formal" via Active Directory e metadados do Confluence |
| #02 | Seção 2 | Ambiguidade | Adicionada seção 2.3 com critério técnico para "versão mais recente" de planilhas (data de modificação + convenção de nomenclatura) |
| #03 | Seção 3 | Gap Crítico | Removida regra de data 01/12/2023 (dependência de integração CRM não declarada); substituída por alerta genérico de supervisor |
| #04 | Seções 2/3/10 | Inconsistência | Seção 3.1 explicitada como referência de curadoria pré-lançamento; R-01 reclassificado como risco de projeto |
| #05 | Seção 4 / Tabela 4.1 | Inconsistência | Tabela de escalação completada com ramal e e-mail para todos os 7 canais; dados tratados como configuração |
| #06 | Seção 5.1 | Ambiguidade | Prazos de indexação especificados como "horas corridas" explicitamente |
| #07 | Seção 5.2 | Gap Crítico | Alerta de 60 dias complementado com destinatário, canal, prazo de resposta e consequência da não-resposta |
| #08 | Seções 5.1 e 6 (RF-05, RF-10) | Gap Crítico | Mecanismo de detecção especificado por fonte; adicionado RF-10 (Interface de Curadoria) |
| #09 | Seção 7 (RNF-03) e 8.2 | Ambiguidade | Metodologia de medição definida na seção 8.2 com avaliação humana, amostra mínima e critérios objetivos |
| #10 | Seção 5b / RNF-05 | Gap Crítico | Adicionada seção 5b com autenticação SSO/VPN para presencial e home office; RNF-05 atualizado |
| #11 | Seção 6 (RF-09) / Seção 9 | Inconsistência | Adicionado RF-09 (Autenticação de Usuário); seção 9 atualizada |
| #12 | Seção 7 (RNF-08) / Seção 9 / Seção 10 | Gap Crítico | Adicionado RNF-08 (LGPD); seção 9 com base legal e anonimização; risco R-08 adicionado |
| #13 | Seção 8.2 | Ausência de AC | Adicionados critérios de aceite individuais e testáveis para todos os 8 RNFs |
| #14 | Seção 5.3 | Gap | Curadoria mensal estruturada com dono (PM), pauta mínima e critério de decisão em impasse |
| #15 | Seção 5b / Seção 10 (R-09) | Gap Crítico | Adicionada seção 5b (Interface e Integração) com painel lateral no CRM, fluxo de uso e SSO; risco R-09 adicionado |

#### Estrutura Final do Documento v2

| Seção | Conteúdo | Status vs v1 |
|---|---|---|
| 1 | Objetivo do Sistema (3 linhas) | Mantido com ajuste menor no último parágrafo |
| 2 | Escopo das fontes — com seções 2.2 e 2.3 novas | Expandido (#01, #02) |
| 3 | Regras de contradição — separação entre curadoria e runtime | Refinado (#03, #04) |
| 4 | Falta de resposta — tabela 4.1 completa com ramal e e-mail | Refinado (#05) |
| 5 | Atualização — prazos, alerta 60 dias, curadoria mensal estruturada | Refinado (#06, #07, #14) |
| **5b** | **Interface e Integração (seção nova)** | **Novo (#15, #10, #11)** |
| 6 | 10 RFs (RF-09 e RF-10 novos) | Expandido (#08, #11) |
| 7 | 8 RNFs (RNF-08 novo) | Expandido (#12) |
| 8 | Critérios de aceite para todos os RFs e todos os RNFs | Expandido (#13) |
| 9 | Rastreabilidade com base legal LGPD e anonimização | Refinado (#12) |
| 10 | 9 riscos (R-08 e R-09 novos; R-01 reclassificado) | Expandido (#04, #12, #15) |
| 11 | Resumo executivo (4 parágrafos) | Atualizado |
| **12** | **Tabela de alterações aplicadas (nova)** | **Novo** |

---

## Artefatos Gerados

| Artefato | Localização | Tamanho | Descrição |
|---|---|---|---|
| `feedback.md` | `Exercicio1.3/` | ~22 KB | Feedback de revisão com 15 achados estruturados por item, evidência, impacto e sugestão |
| `PRD-NovaTech-Assistente-RAG-versao2.docx` | `Exercicio1.3/` | ~40 KB | PRD refinado com 12 seções, 10 RFs, 8 RNFs, critérios de aceite completos e tabela de alterações |

**Ferramenta usada para geração do .docx:** HTML intermediário + Microsoft Word COM (Word 16.0) via PowerShell
