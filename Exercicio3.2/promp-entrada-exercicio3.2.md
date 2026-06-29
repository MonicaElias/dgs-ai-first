# Prompt de Entrada — Exercício 3.2 NovaTech

**Sessão:** Design de Harness de Produto para o Assistente NovaTech AI  
**Data:** 27/06/2026  
**Participante:** Monica Elias (Product Specialist)  
**Modelo utilizado:** Claude Sonnet 4.6 (Claude Code)

---

## Contexto da Sessão

O exercício consistiu em projetar um harness de produto completo para o Assistente de IA da NovaTech, com foco em governança pós-go-live. O assistente é um bot corporativo de atendimento ao cliente acessado via Microsoft Teams por atendentes humanos, com pipeline de RAG indexado no Azure AI Search (847 documentos). O harness deveria ser baseado nos guardrails formais definidos no Cenário 2 (GR-001 a GR-017).

---

## Prompt do Usuário

```
Você é um consultor especializado em governança de produtos de IA para atendimento corporativo.

Preciso que você projete um harness de produto para o assistente de IA da NovaTech, 
que entrará em go-live em breve e precisará evoluir de forma controlada após o lançamento.

---

CONTEXTO DO SISTEMA:
- Assistente de atendimento corporativo com pipeline de RAG
- Base de 847 documentos indexados no Azure AI Search
- Bot no Teams acessível por atendentes humanos
- 5 atendentes-piloto em staging, expansão planejada após go-live
- 12% de respostas incorretas identificadas em testes internos (alucinação, 
  documento desatualizado, chunk incorreto)

---

GUARDRAILS DO PRODUTO (cenário 2 — devem ser preservados ao longo da evolução):
[Arquivo referenciado: @Exercicio2.2/guardrails-completo.md]

Guardrails formais carregados no contexto:
- GR-001 a GR-006: Seção DEVE (comportamentos obrigatórios)
- GR-007 a GR-012: Seção NÃO DEVE (comportamentos proibidos)
- GR-013 a GR-017: Seção QUANDO EM DÚVIDA (comportamentos de fallback)

---

CONCEITO DE HARNESS DE PRODUTO:
"Define quais métricas de qualidade são monitoradas, como o feedback de usuários 
é processado, e como mudanças no assistente são validadas antes de ir a produção 
(regression testing de produto)."

---

INSTRUÇÕES:

Projete um harness de produto completo cobrindo obrigatoriamente as 3 seções abaixo:

1. PROCESSO DE FEEDBACK
   Descreva o fluxo completo desde o momento em que um atendente reporta um problema 
   até a melhoria efetiva no assistente. Inclua:
   - Como o atendente registra o feedback (canal, formato)
   - Quem recebe e triaga o feedback
   - Como o feedback é classificado (ex: erro de documento, erro de prompt, 
     gap de cobertura)
   - Qual ação de melhoria é disparada para cada tipo de classificação
     (novo documento? ajuste de prompt? reindexação? ajuste de guardrail?)
   - Como o atendente é informado de que seu feedback gerou melhoria

2. REGRESSION TESTING DE PRODUTO
   Descreva como validar que uma mudança no assistente não degradou 
   respostas que já funcionavam e não violou os guardrails. Inclua:
   - Um conjunto mínimo de casos de teste de regressão (perguntas + 
     resposta esperada) cobrindo os cenários críticos
   - Como verificar especificamente que cada guardrail (DEVE / NÃO DEVE / 
     QUANDO EM DÚVIDA) continua sendo respeitado após a mudança
   - Critério de aprovação: o que precisa passar para a mudança seguir adiante
   - O que acontece quando um teste de regressão falha

3. HUMAN-IN-THE-LOOP (HITL)
   Defina concretamente quais mudanças exigem aprovação humana antes de ir 
   a produção e quem aprova cada tipo. Inclua:
   - Tabela de tipos de mudança x nível de aprovação necessário
     (ex: mudança de prompt, adição de documento, reindexação, 
     alteração de guardrail, mudança de modelo)
   - Quem são os aprovadores para cada nível 
     (Product Specialist, Tech Lead, Delivery Manager, Supervisor?)
   - Prazo máximo para aprovação antes de escalar
   - Como a aprovação é registrada (rastreabilidade)

---

FORMATO DE SAÍDA:
Gere um documento formal chamado "Harness de Produto — NovaTech AI Assistant".
Estruture em seções claras com títulos, subtítulos e use tabelas onde ajudar 
a clareza (especialmente no HITL).
O documento deve ser acionável — qualquer membro do time deve conseguir 
seguir o processo sem ambiguidade.
Seja específico e concreto. Evite generalidades como "monitorar a qualidade" 
sem definir como e quem.
```

---

## Documento de Contexto Carregado

**Arquivo:** `Exercicio2.2/guardrails-completo.md`  
**Versão:** 1.0 — 13/06/2026  
**Conteúdo:** 17 guardrails formais (GR-001 a GR-017) organizados em três seções (DEVE / NÃO DEVE / QUANDO EM DÚVIDA), com triggers, exemplos corretos, exemplos incorretos e referência aos três incidentes internos (INCIDENTE-01, INCIDENTE-02, INCIDENTE-03) que motivaram sua criação.

---

## Output Gerado

**Arquivo:** `Pratica 3/harness-produto-novatech.md`  
**Título:** Harness de Produto — NovaTech AI Assistant  
**Versão:** 1.0 — 27/06/2026

### Estrutura do documento produzido

**Seção 1 — Processo de Feedback**
- Canal de registro: comando `/feedback` no bot do Teams com formulário estruturado (7 campos, ID da conversa capturado automaticamente)
- Triagem diária pelo Product Specialist até as 10h, com SLA por severidade (P1=24h úteis, P2=5 dias úteis, P3=15 dias úteis)
- 5 classificações de causa com ação específica por tipo:
  - `ERRO-DOC` → reindexação do documento correto (Tech Lead + Product Specialist)
  - `ERRO-CHUNK` → revisão de parâmetros de chunking (Tech Lead)
  - `ERRO-PROMPT` → ajuste de prompt com regression testing obrigatório (Product Specialist + Tech Lead)
  - `GAP-COBERTURA` → levantamento e adição de novo documento (Product Specialist)
  - `AJUSTE-GUARDRAIL` → revisão via HITL nível 2 (Product Specialist + Delivery Manager)
- Notificação padronizada ao atendente via Teams com template estruturado quando a melhoria entra em produção

**Seção 2 — Regression Testing de Produto**
- 17 casos de regressão (RT-001 a RT-017) mapeados 1:1 com GR-001 a GR-017
- Cada caso especifica: pergunta de entrada, resposta esperada concreta e critério de falha objetivo
- Critério de aprovação: falha em guardrails críticos (GR-002, GR-007, GR-008, GR-011) = bloqueio imediato; demais = análise de risco
- Falha reinicia o ciclo do zero; nenhuma notificação ao atendente antes da aprovação completa
- Registro em planilha versionada: `\\novatech-fs\ai-governance\regression-testing\RT-YYYYMMDD-*.xlsx`

**Seção 3 — Human-in-the-Loop (HITL)**
- 3 níveis de aprovação em tabela com 10 tipos de mudança mapeados
  - Nível 1: adição de doc, reindexação, ajuste de prompt não-guardrail (Product Specialist ou Tech Lead)
  - Nível 2: remoção/substituição de doc, alteração ou adição de guardrail (Product Specialist + Delivery Manager)
  - Nível 3: mudança de modelo, expansão de usuários, mudança de arquitetura, desativação de guardrail (Tech Lead + Delivery Manager + CTO)
- Prazos máximos por tipo (1 a 5 dias úteis) com escalação automática via Teams se vencido
- Rastreabilidade dupla: ticket no Azure DevOps + CHANGELOG versionado no repositório Git

---

## Decisões de Design Tomadas

| Decisão | Justificativa |
|---|---|
| Canal `/feedback` no Teams (não e-mail como canal primário) | Captura automática do ID da conversa e da resposta do assistente, eliminando transcrição manual e erro de cópia |
| Triagem diária com horário fixo (10h) | Garante cadência previsível sem depender de disponibilidade ad hoc do Product Specialist |
| 17 casos de regressão (1 por guardrail) | Cobertura completa sem redundância; cada caso tem gatilho e critério de falha objetivos para avaliação sem subjetividade |
| Bloqueio imediato apenas para guardrails críticos (GR-002/007/008/011) | Proporcionalidade: erros com impacto regulatório/operacional direto bloqueiam; demais passam por análise de risco |
| Desativação de guardrail como Nível 3 não-escalável (só CTO) | Guardrails foram criados a partir de incidentes reais; remoção exige responsabilidade máxima e não pode ser delegada |
| CHANGELOG em arquivo versionado no repositório | Auditabilidade permanente integrada ao ciclo de desenvolvimento, não dependente de ferramentas externas |

---

*Prompt registrado para rastreabilidade do exercício 3.2 — Fase de Governança e Validação (Cenário 3, DGS AI First).*
