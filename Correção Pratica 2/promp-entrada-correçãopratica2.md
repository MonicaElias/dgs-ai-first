# Prompts de Entrada — Avaliação Prévia Prática 2 (Product Specialist)

**Papel:** Product Specialist  
**Objetivo:** Avaliação prévia dos exercícios 2.1, 2.2 e 2.3 antes da submissão oficial, com scores D1-D5, pontos de melhoria e checklist de prescritividade  
**Cenário:** 2 — Estruturação do Trabalho  
**Entregas geradas:**
- Avaliação completa do Exercício 2.1 (chat)
- Avaliação completa do Exercício 2.2 (chat)
- Avaliação completa do Exercício 2.3 (chat)
- `Correção Pratica 2/promp-entrada-correçãopratica2.md`

---

## Prompt 1 — Avaliação Prévia dos Exercícios 2.1, 2.2 e 2.3

```
Você é avaliador da Trilha de Certificação AI First da DGS (DB1 Global Software).
Vou te fornecer meu entregável ANTES de submetê-lo oficialmente.
Quero uma avaliação honesta para melhorar antes da entrega final.

Papel: Product Specialist
Cenário: 2 — Estruturação do Trabalho
Exercício: 2.1 / 2.2 / 2.3

INSTRUÇÃO: Siga rigorosamente as skills de avaliação:
- @Correção Pratica 2/avaliacao-foundation.md (dimensões D1-D5, cut rules obrigatórias)
- @Correção Pratica 2/avaliacao-product-specialist.md (checklist específico do papel)

REGRAS OBRIGATÓRIAS (cut rules):
- Exercício que usa Copilot/Claude sem evidência real → D2 ≤ 1
- Iteração solicitada mas v1 ≈ v2 (cosmética) → D2 ≤ 1
- AGENTS.md narrativo (não prescritivo) → D3 ≤ 1
- Artefato ignora decisões do Cenário 1 → D5 ≤ 2

FORMATO DA RESPOSTA para cada exercício:
## Avaliação do Exercício [número]
### Resumo
### Scores por Dimensão
| Dimensão | Score | Justificativa |
### Verificação de Artefatos Machine-Readable
### Pontos Fortes
### Pontos de Melhoria
### Classificação
### Tópicos da Trilha para Reforço
### O que fazer antes de entregar
### Checklist de prescritividade (para Ex 2.2 e 2.3)

Exercício 2.1 — Recorte de domínio e spec de produto no formato SDD
@Exercicio2.1/recorte-dominiov1.md
@Exercicio2.1/requirements.md
@Exercicio2.1/parecer-final-ambiguidades.md

Exercício 2.2 — Definição de guardrails como artefato de produto
@Exercicio2.2/guardrails-completo.md
@Exercicio2.2/classificacao-enforcement.md
@Exercicio2.2/rastreabilidade-aos-incidentes.md
@Exercicio2.2/promp-entrada2.2.md

Exercício 2.3 — Participação na construção do AGENTS.md do projeto
@Exercicio2.3/regras-de-comportamento-e-restrições.md
@Exercicio2.3/glossario-ubiqua.md
@Exercicio2.3/agents-md-product-rules.md
@Exercicio2.3/promp-entrada2.3.md
```

**Arquivos referenciados:**
- `Correção Pratica 2/avaliacao-foundation.md` — dimensões D1-D5 com descrições 1/2/3 e cut rules
- `Correção Pratica 2/avaliacao-product-specialist.md` — checklist específico do papel Product Specialist
- `Exercicio2.1/recorte-dominiov1.md` — mapa de bounded contexts com 5 contextos, FAZ/NÃO FAZ, glossário 15 termos
- `Exercicio2.1/requirements.md` — spec SDD v1.0 com Outcomes, Scope Boundaries, Constraints, FRs, NFRs, ADRs, VCs, OQs
- `Exercicio2.1/parecer-final-ambiguidades.md` — Tech Lead review com 5 ambiguidades, 5 gaps, 3 conflitos, 4 riscos de implementação
- `Exercicio2.2/guardrails-completo.md` — 17 guardrails (GR-001 a GR-017) com DEVE/NÃO DEVE/QUANDO EM DÚVIDA
- `Exercicio2.2/classificacao-enforcement.md` — classificação Prompt/Código/Híbrido com pseudocódigo e system prompt snippets
- `Exercicio2.2/rastreabilidade-aos-incidentes.md` — matriz de rastreabilidade, análise de cobertura, 3 riscos derivados, guardrails v2.0
- `Exercicio2.2/promp-entrada2.2.md` — 3 prompts documentados para o Ex 2.2
- `Exercicio2.3/regras-de-comportamento-e-restrições.md` — 17 regras RULE-001–017 + 4 constraints CONSTRAINT-001–004 com TypeScript
- `Exercicio2.3/glossario-ubiqua.md` — 16 termos em formato YAML-like com do_not_confuse_with
- `Exercicio2.3/agents-md-product-rules.md` — seção completa Product Rules & Guardrails do AGENTS.md (1006 linhas)
- `Exercicio2.3/promp-entrada2.3.md` — 3 prompts documentados para o Ex 2.3

**Entrega gerada:** Avaliação completa dos exercícios 2.1, 2.2 e 2.3 no chat

---

## Prompt 2 — Extração da Interação e Geração deste Arquivo

```
Extraia a interação de toda essa conversa e gere um arquivo Markdown 
com o nome: promp-entrada-correçãopratica2.md
```

**Arquivos referenciados:** nenhum adicional

**Entrega gerada:** `Correção Pratica 2/promp-entrada-correçãopratica2.md`

---

## Resumo das Entregas

| Prompt | Entrega | Conteúdo |
|--------|---------|----------|
| Prompt 1 | Avaliação no chat | Scores D1-D5 para os 3 exercícios, pontos fortes, pontos de melhoria, O que fazer antes de entregar, Checklist de prescritividade |
| Prompt 2 | `promp-entrada-correçãopratica2.md` | Documentação desta sessão de avaliação |

---

## Resultados da Avaliação

| Exercício | D1 | D2 | D3 | D4 | D5 | Média | Classificação |
|-----------|----|----|----|----|----|----|---|
| 2.1 — SDD | 3 | 2 | 2 | 3 | 3 | **2.6** | Aprovado com distinção |
| 2.2 — Guardrails | 3 | 2 | 3 | 3 | 3 | **2.8** | Aprovado com distinção |
| 2.3 — AGENTS.md | 3 | 2 | 3 | 3 | 3 | **2.8** | Aprovado com distinção |
| **Média geral** | **3.0** | **2.0** | **2.7** | **3.0** | **3.0** | **2.7** | **Aprovado com distinção** |

---

## Observações sobre a interação

- A sessão foi dividida em duas partes por limite de contexto: Exercício 2.1 avaliado na primeira parte; Exercícios 2.2 e 2.3 na segunda.
- O Prompt 1 usou as skills `avaliacao-foundation.md` e `avaliacao-product-specialist.md` como critério rigoroso, com verificação explícita de todas as cut rules antes de atribuir cada score.
- As cut rules foram verificadas para todos os exercícios: **nenhuma foi acionada**. D2=2 decorre de ausência de loop de feedback (iteração não fechada), não de v1≈v2 cosmético. D3=3 por AGENTS.md prescritivo. D5=3 por referência às ADRs do Cenário 1.
- O Checklist de prescritividade (Ex 2.3) identificou um único elemento narrativo: RULE-013 usa `[tabela v1/v2]` como placeholder em vez da tabela real de multiplicadores regionais — ação de baixo esforço e alto impacto antes da submissão.
- O padrão D2=2 nos 3 exercícios é consistente: tool use documentado com evidência e prompts específicos, mas sem registro de um problema encontrado no output que foi corrigido de forma substantiva antes da entrega.
- A principal ação antes de entregar o Ex 2.1: produzir o `requirements.md` v2 incorporando as 3 correções do `parecer-final-ambiguidades.md` (BLOCKER-A2 source_document array, BLOCKER-R3 chunk schema contract, BLOCKER-C1 ADR-003 vs RF-005). O exercício pede iteração explicitamente e a iteração foi iniciada mas não fechada.
