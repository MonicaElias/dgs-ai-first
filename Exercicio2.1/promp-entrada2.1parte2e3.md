# Exercício 2.1 — Registro de Interação: Partes 2 e 3
**Papel:** Product Specialist  
**Data:** 2026-06-13  
**Ferramenta:** Claude (chat)  
**Artefatos gerados:** `requirements.md`, `parecer-final-ambiguidades.md`

---

## Parte 2 — Escrita do requirements.md no formato SDD

### Prompt de entrada

```
Sou um Product Specialist e estamos na fase de estruturação do projeto da empresa NovaTech
e preciso realizar a documentação técnica para o projeto da empresa de logística.

Meu Projeto:
Construir um assistente de IA que irá permitir ao time de atendimento ao cliente realizar
perguntas em linguagem natural e receber respostas fundamentadas na documentação oficial
da empresa, com indicação da fonte e reduzir o tempo de chamado de 12 minutos para 2 minutos.
O assistente será integrado ao ambiente Microsoft da NovaTech (Teams + SharePoint).

Dados do Discovery da jornada do atendente:
- Os atendentes hoje abrem em média 4 fontes diferentes por chamado.
- As dúvidas mais comuns são sobre prazos de entrega (35%), regras de frete (25%),
  política de devolução (20%) e outros (20%).
- Em 15% dos casos, o atendente não encontra resposta e escala para o supervisor.

Inputs:
A etapa de mapeamento de Bounded Contexts e da linguagem ubíqua do assistente NovaTech
já foi concluída. Abaixo está o recorte do domínio:
[mapa de bounded contexts, fronteiras críticas, fronteiras do assistente, glossário de
linguagem ubíqua]

Spec RAG (fase anterior):
- Responde perguntas sobre SLAs, frete e devoluções
- Fontes contraditórias mostram ambas as versões
- Nunca inventa informações
- Toda resposta cita a fonte
- Atualização da base em até 24h

Dados do discovery:
- Categorias: prazos de entrega, regras de frete, política de devolução e SLAs
- 15% das perguntas cruzam duas categorias
- Tempo máximo de resposta: 30 segundos

ADRs da fase anterior (simuladas):
- ADR-001: RAG como estratégia de recuperação —
  Motivo: baixo custo de atualização —
  Impacto: base deve ser indexada e versionada
- ADR-002: Toda resposta com citação de fonte —
  Motivo: rastreabilidade obrigatória —
  Impacto: endpoint retorna metadados da fonte junto à resposta
- ADR-003: Fontes contraditórias exibidas sem arbitragem —
  Motivo: evitar risco regulatório —
  Impacto: modelo não elege uma versão como correta

Etapa 2 — Escrita do requirements.md (estrutura SDD)

Com base nos bounded contexts e na linguagem ubíqua definidos acima, na SPEC de requisitos
RAG, escreva o requirements.md do query endpoint do assistente NovaTech.

Siga obrigatoriamente esta estrutura SDD:

1. Outcomes
   O que muda na vida do usuário quando este endpoint funciona bem.
   Escreva orientado a resultado do usuário — não a features técnicas.

2. Scope Boundaries
   - Dentro do escopo: derive explicitamente dos bounded contexts.
   - Fora do escopo: seja explícito sobre o que o assistente recusa.
   - Fronteiras do assistente: o que ele responde vs. o que encaminha para humano.

3. Constraints
   Restrições técnicas, regulatórias e operacionais não negociáveis.

4. Functional Requirements
   Formato: RF-001, RF-002...
   Para cada um: Descrição, Bounded context relacionado,
   Critério de aceitação verificável pelo QA (formato: "Dado X, quando Y, então Z")

5. Non-Functional Requirements
   Inclua obrigatoriamente: tempo de resposta, disponibilidade,
   rastreabilidade de fonte e política de atualização da base.

6. Prior Decisions (ADRs)
   Referencie as ADRs acima. Para cada uma: decisão tomada e impacto direto no endpoint.

7. Verification Criteria
   Critérios testáveis pelo QA no formato:
   "Dado [contexto], quando [ação], então [resultado esperado]"
   Cubra pelo menos: resposta com fonte, fonte contraditória,
   pergunta fora do escopo, tempo de resposta.

8. Open Questions
   Pontos não decididos que bloqueiam ou arriscam a implementação.

Restrições de saída:
- Use exclusivamente os termos da linguagem ubíqua — nunca sinônimos
- Onde faltar informação, marque com [A DEFINIR]
- Não invente requisitos sem base nos inputs fornecidos

@Exercicio1.3/PRD-NovaTech-Assistente-RAG-versao2.docx
@Exercicio2.1/recorte-dominiov1.md
@Prática 1/anexo-a-documentacao-simulada-novatech.md
```

---

### Output gerado — requirements.md

O Claude leu os artefatos referenciados (recorte-dominiov1.md e anexo-a-documentacao-simulada-novatech.md) e gerou o arquivo `requirements.md` com a seguinte estrutura:

**1. Outcomes** — 5 resultados orientados ao atendente:
- Resolver dúvida sem abrir fontes adicionais (hoje: 4 fontes por chamado)
- Reduzir tempo de chamado de 12 para 2 minutos
- Eliminar 15% de escalações por falta de informação
- Citar fonte com segurança para embasar a orientação ao cliente
- Ser protegido de responder com informação desatualizada (PROC-042 v1/v2)

**2. Scope Boundaries** — 4 bounded contexts cobertos; 9 itens fora do escopo com encaminhamento; tabela "responde vs. encaminha".

**3. Constraints** — 11 restrições (C-01 a C-11), divididas entre técnicas/operacionais e regulatórias, todas rastreadas à origem.

**4. Functional Requirements** — RF-001 a RF-009:
- RF-001: Recepção de pergunta em linguagem natural
- RF-002: Busca semântica e recuperação de chunks
- RF-003: Geração de resposta fundamentada com indicação de fonte
- RF-004: Aviso de baixa confiança
- RF-005: Tratamento de contradição documental (PROC-042 v1 vs. v2)
- RF-006: Guardrail — carga perigosa e devolução
- RF-007: Prioridade de fonte normativa sobre FAQ
- RF-008: Encaminhamento para fora do escopo
- RF-009: Informação sobre tiers de cliente

**5. Non-Functional Requirements** — RNF-001 a RNF-006: tempo de resposta (≤ 30s / P95), disponibilidade [A DEFINIR], rastreabilidade com schema de `source_document`, atualização ≤ 24h, context budget (~4K + ~8K tokens), idioma português formal.

**6. Prior Decisions** — 5 ADRs com decisão e impacto direto: ADR-001 (RAG), ADR-002 (citação obrigatória), ADR-003 (sem arbitragem), ADR-0001 (GPT-4o), ADR-0002 (context budget), ADR-0003 (metadado de vigência).

**7. Verification Criteria** — VC-01 a VC-08: resposta com fonte, contradição documental, gap de escopo, tempo de resposta, guardrail de carga perigosa, baixa confiança, tier inexistente, fonte não normativa.

**8. Open Questions** — OQ-01 a OQ-06: limiar de confiança, disponibilidade, autenticação, persistência de histórico, perguntas multi-contexto, arquivamento do PROC-042 v1.

> **Arquivo gerado:** `Exercicio2.1/requirements.md`

---

## Parte 3 — Revisão crítica como Tech Lead

### Prompt de entrada

```
Você é um Tech Lead sênior com experiência em sistemas RAG
e domínios de logística regulada. Seu trabalho é encontrar
problemas antes que virem bugs ou retrabalho.

## O que revisar
Leia os artefatos abaixo e atue como revisor crítico:
- Bounded contexts
- Linguagem ubíqua
- requirements.md

## Tarefa
Aponte obrigatoriamente:

1. Ambiguidades — termos ou requisitos que dois desenvolvedores
   interpretariam de formas diferentes
2. Lacunas — cenários do discovery que não estão cobertos
   pelos requisitos (especialmente os 15% de perguntas cruzadas)
3. Conflitos — requisitos que se contradizem entre si
   ou com as ADRs
4. Riscos de implementação — bounded contexts mal delimitados
   que vão gerar acoplamento indevido
5. Critérios não testáveis — verification criteria que o QA
   não consegue executar objetivamente

Para cada problema encontrado:
- Cite o trecho exato com problema
- Explique o risco em uma linha
- Sugira a correção

Ao final, dê um parecer: este requirements.md está pronto
para desenvolvimento ou precisa de nova iteração?
```

---

### Output gerado — Revisão técnica completa

O Claude releu `requirements.md` e `recorte-dominiov1.md` e produziu revisão estruturada em 5 eixos:

#### Ambiguidades (5 encontradas)

| ID | Problema | Correção |
|----|----------|----------|
| A1 | RF-004: "grau de confiança suficiente" sem limiar numérico | Marcar RF-004 como bloqueado por OQ-01; usar `"score inferior ao limiar definido em OQ-01"` |
| A2 ⚠️ | `source_document` definido como objeto singular; RF-005 exige duas fontes simultâneas | Redefinir como `source_documents: []` com cardinalidade 0/1/2+ desde o início |
| A3 | "Histórico limitado a 3 turnos" — turno = 1 mensagem ou 1 par? | `"3 pares pergunta/resposta anteriores"` |
| A4 | RF-007 descreve texto na resposta; VC-08 testa campo `is_normative: false` — canais diferentes | Especificar dois canais obrigatórios: campo no payload E texto fixo na resposta |
| A5 | RF-005 não define como detectar contradição: mapa estático, metadado ou LLM? | Definir: detecção pós-retrieval por chunks com mesmo prefixo `document_id` e `version` distintos |

#### Lacunas (5 encontradas)

| ID | Cenário não coberto | Correção |
|----|-------------------|----------|
| L1 ⚠️ | 15% de perguntas cruzadas sem comportamento de fallback (OQ-05 sem RF) | Adicionar RF-010 com comportamento explícito para perguntas multi-contexto |
| L2 | Carga acima de 5.000 kg requer aprovação de gerente (PROC-042 seção 4) — não coberto | Adicionar aviso obrigatório no RF-003 para esse caso |
| L3 | Interceptação de carga em trânsito (PROC-088 não indexada) — confunde com devolução | Adicionar ao Scope Boundaries e ao glossário: `interceptação` ≠ `devolução` |
| L4 | Classificação proativa de incidente crítico ausente (SLA-2024 seção 3) | Adicionar RF para identificação proativa com base nos 4 critérios do SLA-2024 |
| L5 | Devoluções parciais (POL-001 seção 3.4) | Adicionar cenário em RF-003 ou VC |

#### Conflitos (3 encontrados)

| ID | Requisitos em conflito | Correção |
|----|----------------------|----------|
| C1 ⚠️ BLOQUEADOR | ADR-003 "modelo não elege uma versão como correta" vs. RF-005 "chamados novos devem usar v2" | Reescrever ADR-003: "sem arbitragem autônoma — priorização segue regra de vigência do próprio documento" |
| C2 | RF-007 "normativo > FAQ" vs. ADR-003 "sem arbitragem" — regra incompleta | Adicionar hierarquia explícita C-12 para 3 casos: normativo/normativo, normativo/FAQ, FAQ/FAQ |
| C3 | RNF-001 ≤ 30s nunca foi validado com GPT-4o + Azure AI Search | Adicionar OQ-07: benchmark de latência obrigatório antes da implementação |

#### Riscos de implementação (4 encontrados)

| ID | Risco | Correção |
|----|-------|----------|
| R1 | "Copiloto de Atendimento" concentra 7 responsabilidades — handler de 300+ linhas | Adicionar nota de decomposição em 4 passos lógicos separáveis no Scope Boundaries |
| R2 ⚠️ | RF-006 (guardrail carga perigosa) sem modo de enforcement — prompt vs. código | Adicionar constraint C-13: enforcement determinístico obrigatório para RF-006 |
| R3 ⚠️ BLOQUEADOR | Sem contrato de schema compartilhado entre ingestão e query endpoint | Adicionar OQ-08: schema de metadados como contrato compartilhado, bloqueador para RF-002/005/007 |
| R4 | Mecanismo de detecção de contradição não especificado (coberto em A5) | Ver correção de A5 |

#### Critérios não testáveis (4 encontrados)

| ID | Critério problemático | Correção |
|----|----------------------|----------|
| T1 | VC-02: "apresenta os dois valores" via free-text | Vincular a campo estruturado `source_documents[]` com `has_contradiction: true` |
| T2 | VC-04: "100 consultas simultâneas em horário de pico" sem ambiente/ferramenta/baseline | Mover para RNF; executar somente após OQ-07 |
| T3 | VC-06 e VC-08: pré-condições dependem do estado do índice de produção | Vincular a índice de teste fixo (fixture) com documentos controlados |
| T4 | VC-05: "não menciona a possibilidade de exceções" — verificação subjetiva | Substituir por `guardrail_triggered` no payload + string match negativo com termos específicos |

#### Parecer final

> **NÃO está pronto para desenvolvimento. Nova iteração necessária.**
>
> Três bloqueadores impedem a implementação: (C1) ADR-003 contradiz RF-005 na regra de arbitragem; (A2/C4) schema `source_document` quebra ao implementar RF-005; (R3) ausência de contrato de schema entre ingestão e query endpoint causa falha silenciosa em runtime.

---

## Parte 3b — Extração do Parecer Final

### Prompt de entrada

```
Extraia o resultado do Parecer final e gere um arquivo Markdown
com o nome: parecer-final-ambiguidades.md
```

### Output gerado

O Claude extraiu todos os problemas encontrados na revisão e gerou o arquivo consolidado com tabelas organizadas por eixo e seção de prioridade de resolução.

> **Arquivo gerado:** `Exercicio2.1/parecer-final-ambiguidades.md`

---

## Artefatos produzidos nesta sessão

| Arquivo | Descrição | Status |
|---------|-----------|--------|
| `Exercicio2.1/requirements.md` | Spec do query endpoint no formato SDD — 8 seções, RF-001 a RF-009, VC-01 a VC-08, 6 Open Questions | Gerado — aguarda iteração |
| `Exercicio2.1/parecer-final-ambiguidades.md` | Revisão técnica completa — 5 ambiguidades, 5 lacunas, 3 conflitos, 4 riscos, 4 critérios não testáveis, parecer e priorização | Gerado |
| `Exercicio2.1/promp-entrada2.1parte2e3.md` | Este arquivo — registro completo da interação | Gerado |

---

## Aprendizados sobre o uso do Claude nesta tarefa

### O que funcionou bem

- **Uso dos termos da linguagem ubíqua:** O Claude respeitou rigorosamente os termos do `recorte-dominiov1.md` — nunca usou "sinônimos" como "cliente prioritário" no lugar de "cliente Gold", ou "retorno" no lugar de "escalação".
- **Derivação de bounded contexts no Scope Boundaries:** O formato "Este módulo cobre o contexto X — responsável por Y" ficou diretamente rastreável ao recorte de domínio.
- **Identificação de gaps documentais:** O Claude cruzou os inputs com o Anexo A e identificou corretamente os gaps (frete padrão, seguro de carga, PROC-088) sem inventar requisitos.
- **Marcação explícita de [A DEFINIR]:** RNF-002 (disponibilidade) foi corretamente marcado em vez de inventar um valor.

### O que precisou de iteração

- **Schema `source_document`:** A spec inicial definiu um objeto singular que quebra o caso de contradição documental — detectado pela revisão do Tech Lead (A2/C4).
- **ADR-003 muito ampla:** "Sem arbitragem" foi escrito de forma genérica demais, criando conflito com RF-005 que instrui aplicar a regra de transição do PROC-042-v2 — detectado em C1.
- **Enforcement de guardrail não especificado:** RF-006 não diferenciou prompt vs. código — risco regulatório identificado em R2.
- **VC-04 como teste de carga disfarçado:** O critério de tempo de resposta foi escrito como verification criterion mas requer infraestrutura de load testing — identificado em T2.

### Padrão de uso recomendado para este tipo de tarefa

1. **Prompt de geração:** fornecer todos os inputs estruturados (bounded contexts, ADRs, discovery data, restrições de saída) em um único prompt — o Claude usa todos na geração sem precisar de rodadas adicionais.
2. **Prompt de revisão:** instruir o Claude a atuar como um papel diferente (Tech Lead) e fornecer critérios explícitos do que revisar — produz revisão mais objetiva do que pedir "o que está errado?".
3. **Extração de artefatos:** o Claude gera arquivos markdown diretamente, eliminando a necessidade de copiar e colar manualmente.
