# Prompt de Entrada — Exercício 2.1 Parte 1
**Fase:** Estruturação do Projeto  
**Papel:** Product Specialist  
**Tarefa:** Recorte de Domínio — Bounded Contexts  
**Data:** 2026-06-11

---

## Contexto fornecido ao assistente

Sou um Product Specialist e estávamos na fase de estruturação do projeto da empresa NovaTech e preciso realizar o recorte de domínio da empresa de logística.

---

## Prompt enviado

**Descrição do projeto:**

Um assistente de IA que irá permitir ao time de atendimento ao cliente realizar perguntas em linguagem natural e receber respostas fundamentadas na documentação oficial da empresa, com indicação da fonte e reduzir o tempo de chamado de 12 minutos para 2 minutos. O assistente será integrado ao ambiente Microsoft da NovaTech (Teams + SharePoint).

---

**Com base nisso, quero que você:**

1. Identifique os possíveis Bounded Contexts do domínio
2. Para cada contexto, descreva:
   - Nome do contexto
   - Responsabilidade principal (o que ele "cuida")
   - Principais conceitos/entidades que pertencem a ele
   - Com quais outros contextos ele se relaciona
3. Aponte onde estão as fronteiras mais críticas (onde contextos se tocam e podem gerar confusão)
4. Sugira um nome em linguagem ubíqua para cada contexto, usando termos do dia a dia da logística
5. Identifique as fronteiras do que o assistente faz e o que não faz, e como se relaciona com os outros, usando termos do dia a dia da logística

Apresente o resultado em formato de tabela + um parágrafo explicando as relações entre os contextos.

---

**Os contextos devem ser extraídos dos documentos:**

- `anexo-a-documentação-simulada-novatech.md`
- `FAQ-atendimento-resumido.md`
- `POL-001-politica-devolucao-resumido.md`
- `PROC-042-frete-especial-v1-resumido.md`
- `PROC-042-v2-frete-especial-revisado1-resumido.md`
- `SLA-2024-tabela-sla-clientes-resumido.md`

---

**Contextos pré-identificados:**

1. Atendimento ao cliente / FAQ
2. Gestão Documental
3. Logística e Frete
4. SLA e Contratos

---

**Entrega:** Gere o conteúdo num arquivo Markdown com o nome: `recorte-dominiov1.md`

---

## Documentos de referência utilizados

| Documento | Tipo | Responsável | Status |
|-----------|------|-------------|--------|
| POL-001 — Política de Devolução de Mercadorias (v3.1) | Normativo | Diretoria de Operações | Vigente |
| PROC-042 — Cálculo de Frete Especial (v1.0) | Normativo | Diretoria Comercial | ⚠️ Coexiste com v2 sem hierarquia clara |
| PROC-042-v2 — Cálculo de Frete Especial Revisado (v2.0) | Normativo | Diretoria Comercial | ⚠️ Coexiste com v1 sem indicação formal de substituição |
| SLA-2024 — Tabela de SLA por Tipo de Cliente (v2024.1) | Contratual | Diretoria Comercial + Operações | Vigente |
| FAQ-Atendimento — Perguntas Frequentes (sem versão controlada) | Informal | Nenhum responsável formal | ⚠️ Não validado pelo Compliance |

---

## Output gerado

Arquivo: `recorte-dominiov1.md`  
Localização: `Pratica 2/recorte-dominiov1.md` → copiado para `Exercicio2.1/recorte-dominiov1.md`

**Estrutura do artefato produzido:**

1. Visão geral do domínio
2. Tabela geral dos 5 Bounded Contexts identificados
3. Detalhamento de cada contexto (responsabilidade, entidades, relacionamentos)
4. Fronteiras críticas — onde os contextos se tocam e podem gerar confusão
5. Fronteiras do assistente: O que FAZ e o que NÃO FAZ
6. Diagrama de relacionamento entre contextos
7. Glossário de linguagem ubíqua (15 termos)

**Bounded Contexts identificados:**

| # | Nome técnico | Nome em linguagem ubíqua |
|---|-------------|--------------------------|
| 1 | Atendimento ao Cliente | Central de Atendimento |
| 2 | Gestão Documental | Base de Conhecimento |
| 3 | Logística e Frete | Mesa de Operações |
| 4 | SLA e Contratos | Gestão de Nível de Serviço |
| 5 | Assistente IA | Copiloto de Atendimento |
