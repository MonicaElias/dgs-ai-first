# Parecer Final — Revisão Técnica do requirements.md
**Revisor:** Tech Lead  
**Data:** 2026-06-13  
**Artefatos revisados:** `recorte-dominiov1.md` v1.0, `requirements.md` v1.0  
**Veredito:** NÃO está pronto para desenvolvimento — nova iteração necessária

---

## Veredito

Este requirements.md **NÃO está pronto para desenvolvimento**. É necessária uma nova iteração.

A qualidade geral é acima da média para um primeiro rascunho — o uso consistente da linguagem ubíqua, a separação de bounded contexts e a honestidade das Open Questions são pontos fortes reais. Mas existem **3 bloqueadores que impediriam a implementação de funcionar corretamente**:

| Bloqueador | Por que bloqueia |
|---|---|
| **C1** — ADR-003 contradiz RF-005 | Dev implementa "sem arbitragem" e quebra o caso PROC-042 v1/v2, ou implementa "usa v2 sempre" e quebra chamados históricos |
| **C4 / A2** — `source_document` singular vs. multi-source | O schema quebra na primeira implementação de RF-005; a correção impacta RF-003, VC-01, VC-02 e VC-08 simultaneamente |
| **R3** — ausência de contrato de schema com ingestão | O endpoint falha silenciosamente em runtime se os campos `is_active_version` e `is_normative` não existirem nos chunks |

Os itens A4, L1 (OQ-05), L2 e R2 são críticos o suficiente para entrar na próxima iteração — não são blockers de implementação, mas são riscos de comportamento incorreto em produção.

---

## Resumo de Todos os Problemas Encontrados

### Ambiguidades

| ID | Trecho com problema | Risco | Correção |
|----|-------------------|-------|----------|
| **A1** | RF-004: `"grau de confiança suficiente"` | Dois devs implementarão limiares distintos; o comportamento de baixa confiança muda completamente | Marcar RF-004 como bloqueado por OQ-01; substituir por `"score inferior ao limiar definido em OQ-01 [A DEFINIR]"` |
| **A2** ⚠️ | RNF-003: `source_document` como objeto singular + RF-005 exige duas fontes simultâneas | Dev que implementar RF-005 estende o schema para array, quebrando VC-01 | Definir schema polimórfico desde o início: `source_documents: []` com cardinalidade 0/1/2+ |
| **A3** | RNF-005: `"histórico limitado a 3 turnos anteriores"` | Dev A conta 3 mensagens; Dev B conta 3 pares — o context budget pode dobrar | `"3 pares pergunta/resposta anteriores (3 mensagens do atendente + 3 respostas do assistente)"` |
| **A4** | RF-007 descreve texto na resposta; VC-08 testa campo estruturado `is_normative: false` | Dev implementa apenas um dos dois canais e passa em um critério, falha no outro | Especificar dois canais obrigatórios: campo `is_normative: false` no payload E texto fixo inserido na resposta |
| **A5** | RF-005: `"o endpoint exibe as informações de ambas as versões, sinaliza a contradição"` sem definir como detectar | Três implementações possíveis (mapa estático, metadado pós-retrieval, LLM) com comportamentos radicalmente diferentes | Adicionar: `"detecção ocorre pós-retrieval: chunks com mesmo prefixo document_id e version distintos"` |

---

### Lacunas

| ID | Cenário não coberto | Risco | Correção |
|----|-------------------|-------|----------|
| **L1** ⚠️ | 15% de perguntas cruzadas (discovery) — OQ-05 sem comportamento de fallback | Ranker retorna chunks de um contexto só ou divide budget pela metade sem critério | Adicionar RF-010 provisório com comportamento explícito para perguntas multi-contexto |
| **L2** | Carga acima de 5.000 kg requer aprovação do gerente (PROC-042 seção 4) | Atendente recebe multiplicadores corretos sem o aviso de aprovação obrigatória | Adicionar aviso obrigatório no RF-003 para esse caso específico |
| **L3** | Interceptação de carga em trânsito (PROC-088 não indexada) | Assistente aplica regras de devolução para cargo em trânsito — processos distintos | Adicionar ao Scope Boundaries e ao glossário: `interceptação` ≠ `devolução` |
| **L4** | Classificação proativa de incidente crítico ausente (SLA-2024 seção 3) | Atendente não é alertado quando o cenário descrito qualifica como incidente crítico | Adicionar RF para identificação proativa de incidente crítico com base nos 4 critérios do SLA-2024 |
| **L5** | Devoluções parciais (POL-001 seção 3.4) | Atendente recebe resposta genérica de devolução sem a regra de reembolso proporcional por volume | Adicionar cenário de devolução parcial como caso coberto em RF-003 ou VC |

---

### Conflitos

| ID | Requisitos em conflito | Risco | Correção |
|----|----------------------|-------|----------|
| **C1** ⚠️ BLOQUEADOR | ADR-003 `"modelo não elege uma versão como correta"` vs. RF-005 `"chamados novos devem usar v2"` | Dev implementa "sem arbitragem total" (quebra RF-005) ou "usa v2 sempre" (quebra chamados históricos) | Reescrever ADR-003: `"sem arbitragem autônoma — a priorização segue a regra de vigência do próprio documento (PROC-042-v2 seção 5)"` |
| **C2** | RF-007 `"normativo > FAQ"` vs. ADR-003 `"sem arbitragem"` — regra incompleta para edge cases | Dev aplica "sem arbitragem" a conflito normativo/FAQ incorretamente | Adicionar constraint C-12 com hierarquia explícita de 3 casos: normativo vs. normativo / normativo vs. FAQ / FAQ vs. FAQ |
| **C3** | RNF-001 `"≤ 30s"` nunca foi validado com GPT-4o + Azure AI Search | SLA de latência pode ser inalcançável com o stack de produção sob carga concorrente | Adicionar OQ-07: benchmark de latência obrigatório antes da implementação com o stack Azure |

---

### Riscos de Implementação

| ID | Risco | Impacto | Correção |
|----|-------|---------|----------|
| **R1** | "Copiloto de Atendimento" concentra embedding, retrieval, detecção de contradição, prompt, guardrails e formatação em um único bounded context | Handler de 300+ linhas impossível de testar em isolamento; falhas em produção não são isoláveis | Adicionar nota ao Scope Boundaries indicando os 4 passos lógicos separáveis; plan.md deve decompô-los em units testáveis |
| **R2** ⚠️ | RF-006 (guardrail carga perigosa) sem modo de enforcement especificado — prompt vs. código | Enforcement via prompt é probabilístico; um edge case ou variação de temperatura pode fazer o LLM afirmar que a exceção existe — risco regulatório | Adicionar constraint C-13: enforcement determinístico obrigatório para RF-006 via verificação de output antes do retorno |
| **R3** ⚠️ BLOQUEADOR | Campos `is_active_version`, `is_normative`, `version`, `document_id` especificados no query endpoint, mas pipeline de ingestão tem spec separado sem contrato compartilhado | Query endpoint falha silenciosamente em runtime ao tentar ler campos inexistentes nos chunks | Adicionar OQ-08: schema de metadados de chunks como contrato compartilhado ingestão↔query, responsabilidade do Tech Lead, bloqueador para RF-002, RF-005 e RF-007 |
| **R4** | Mecanismo de detecção de contradição não especificado (coberto em A5) | Duas implementações incompatíveis se ingestão e query forem desenvolvidos por devs diferentes | Ver correção de A5 |

---

### Critérios Não Testáveis pelo QA

| ID | Critério problemático | Problema | Correção |
|----|----------------------|----------|----------|
| **T1** | VC-02: `"a resposta apresenta os dois valores (1.6 da v1 e 1.8 da v2)"` via free-text | Dois QAs chegam a conclusões diferentes para respostas que não citam os valores numericamente mas estão corretas | Vincular a campo estruturado `source_documents[]` com `has_contradiction: true` na raiz do payload |
| **T2** | VC-04: `"100 consultas simultâneas em horário de pico"` | "Horário de pico" indefinido; sem ambiente, ferramenta ou baseline especificados — QA não consegue executar sozinho | Mover para RNF como requisito de performance; executar somente após OQ-07 respondida |
| **T3** | VC-06 e VC-08: pré-condições dependem do estado do índice de produção | Se documento normativo sobre seguro de carga ou carga danificada for indexado, os testes mudam de resultado sem alteração de código | Vincular a índice de teste fixo (fixture) com conjunto de documentos controlado |
| **T4** | VC-05: `"a resposta não menciona a possibilidade de exceções"` | Verificação de ausência de texto livre é subjetiva — "tratamento especial" pode ou não ser interpretado como exceção | Substituir por: `guardrail_triggered: 'dangerous_goods_return'` no payload + string match negativo com termos específicos (`"exceção"`, `"possível"`, `"possibilidade"`) |

---

## Prioridade de Resolução para a Próxima Iteração

### Bloqueadores — resolver antes do Gate 1

1. **C1** — Reescrever ADR-003 para distinguir arbitragem autônoma de aplicação de regra documental
2. **A2 + C4** — Redefinir `source_document` como `source_documents[]` com schema polimórfico
3. **R3** — Criar OQ-08 e definir schema de metadados de chunks como contrato compartilhado

### Críticos — resolver na mesma iteração

4. **R2** — Adicionar constraint C-13 de enforcement determinístico para RF-006
5. **L1** — Adicionar comportamento explícito para perguntas multi-contexto (RF-010 ou decisão em OQ-05)
6. **T1, T3, T4** — Vincular VC-02, VC-06, VC-08 e VC-05 a campos estruturados e fixtures de teste

### Próxima iteração (não bloqueiam Gate 1)

7. **L2, L3, L4** — Lacunas de carga acima de 5.000 kg, interceptação e incidente crítico
8. **A1, A3, A4** — Clarificações de threshold de confiança, turno e formato de aviso
9. **C3** — OQ-07 para benchmark de latência com stack Azure antes de implementar RNF-001
