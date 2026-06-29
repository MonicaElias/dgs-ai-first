# Harness de Produto — NovaTech AI Assistant

**Versão:** 1.0  
**Data:** 27/06/2026  
**Responsável:** Product Specialist  
**Escopo:** Assistente interno de atendimento ao cliente — domínio de logística (SLAs, frete especial, devoluções)  
**Ambiente:** Bot no Teams + pipeline de RAG com 847 documentos indexados no Azure AI Search  
**Situação atual:** 5 atendentes-piloto em staging; 12% de respostas incorretas identificadas em testes internos

---

## Contexto

Este documento define o harness de produto do Assistente NovaTech — o conjunto de processos que garante que o assistente evolui de forma controlada após o go-live, preservando os guardrails definidos no documento de governança (GR-001 a GR-017) e sem regredir respostas que já funcionavam corretamente.

O harness cobre três eixos obrigatórios:

1. **Processo de Feedback** — como problemas identificados pelos atendentes chegam à equipe e geram melhoria efetiva.
2. **Regression Testing de Produto** — como validar que mudanças não quebram o que já funciona e não violam guardrails.
3. **Human-in-the-Loop (HITL)** — quais mudanças exigem aprovação humana e de quem.

---

## Seção 1 — Processo de Feedback

### 1.1 Canal de registro

O atendente registra feedback diretamente no bot do Teams, via o comando `/feedback` disponível em qualquer conversa com o assistente. O comando abre um formulário estruturado com os campos obrigatórios abaixo:

| Campo | Tipo | Obrigatório |
|---|---|---|
| ID da conversa | Preenchido automaticamente | Sim |
| Atendente | Preenchido automaticamente (AD) | Sim |
| Tipo de problema | Seleção: Resposta incorreta / Fonte errada / Resposta ausente / Linguagem inadequada / Outro | Sim |
| Descrição do problema | Texto livre (máx. 500 caracteres) | Sim |
| Pergunta que originou o problema | Copiada automaticamente da conversa | Sim |
| Resposta que o assistente gerou | Copiada automaticamente da conversa | Sim |
| Resposta esperada (se souber) | Texto livre | Não |

O formulário é submetido ao canal `#ai-feedback` no Teams, com cópia automática para o sistema de tickets (Azure DevOps — Board: NovaTech AI / Sprint Feedback).

**Alternativa para atendentes sem acesso ao Teams:** e-mail para `ai-suporte@novatech.com.br` com assunto padronizado `[FEEDBACK-AI] <tipo>` — o Product Specialist converte para ticket manualmente em até 1 dia útil.

---

### 1.2 Triagem do feedback

**Responsável:** Product Specialist  
**Frequência:** Triagem realizada diariamente, até as 10h, para feedbacks do dia anterior.  
**SLA de triagem:** Todo feedback deve ser classificado em até 1 dia útil após o recebimento.

O Product Specialist acessa o Board no Azure DevOps, lê a descrição e a conversa capturada, e atribui:

- **Severidade** (P1/P2/P3 — ver tabela abaixo)
- **Classificação de causa** (ver 1.3)
- **Responsável pela ação**

| Severidade | Critério | SLA de resolução |
|---|---|---|
| P1 — Crítico | Violação de guardrail GR-002, GR-007, GR-008, GR-011 ou resposta com impacto regulatório/contratual | 24h úteis |
| P2 — Alto | Resposta factualmente incorreta ou fonte errada (alucinação, chunk desatualizado) sem impacto imediato | 5 dias úteis |
| P3 — Melhoria | Linguagem inadequada, ausência de fonte, gap de cobertura documental, resposta incompleta | 15 dias úteis |

---

### 1.3 Classificação de causa e ação de melhoria

Para cada feedback triado, o Product Specialist atribui uma das cinco causas abaixo e dispara a ação correspondente:

| Causa | Descrição | Ação de melhoria | Responsável |
|---|---|---|---|
| **ERRO-DOC** | Assistente usou documento desatualizado ou versão errada (ex: PROC-042 v1 em vez de v2) | Reindexação do documento correto + verificação de metadados de versão no Azure AI Search | Tech Lead + Product Specialist |
| **ERRO-CHUNK** | Chunk recuperado pelo RAG estava truncado, fora de contexto ou com sobreposição incorreta | Revisão dos parâmetros de chunking (tamanho, overlap) + reindexação dos documentos afetados | Tech Lead |
| **ERRO-PROMPT** | System prompt ou instrução de guardrail não está produzindo o comportamento esperado | Ajuste de prompt (system prompt ou few-shot examples) — passa por regression testing antes de ir a prod | Product Specialist + Tech Lead |
| **GAP-COBERTURA** | Pergunta válida do domínio não tem cobertura na base documental atual | Levantamento e adição de novo documento à base + reindexação | Product Specialist (solicita documento à área responsável) |
| **AJUSTE-GUARDRAIL** | Guardrail existente está mal formulado, incompleto ou produzindo falsos positivos | Revisão do guardrail afetado — obrigatoriamente passa por HITL nível 2 antes de ir a prod | Product Specialist + Delivery Manager |

---

### 1.4 Notificação de melhoria ao atendente

Quando a ação de melhoria é concluída e validada (passou pelo regression testing), o Product Specialist fecha o ticket no Azure DevOps com status `Resolvido` e envia mensagem direta no Teams ao atendente que originou o feedback, com o texto padronizado:

> **Seu feedback foi implementado.**  
> Problema reportado: [descrição original]  
> O que mudou: [descrição da ação tomada — ex: "O documento PROC-042 v2.0 foi reindexado com metadados de versão corrigidos"]  
> Disponível em produção desde: [data]  
> Ticket: [link para o Azure DevOps]

O atendente é convidado a testar novamente e confirmar a melhoria respondendo ao ticket.

---

## Seção 2 — Regression Testing de Produto

### 2.1 Princípio

Toda mudança no assistente (prompt, documento, guardrail, modelo, índice) deve passar pelo conjunto mínimo de casos de regressão antes de ir a produção. O objetivo é garantir que:

1. Respostas que funcionavam corretamente antes continuam funcionando.
2. Nenhum guardrail (GR-001 a GR-017) foi violado pela mudança.

O regression testing é executado pelo Tech Lead (aspectos técnicos) e pelo Product Specialist (aspectos de produto/guardrail), em ambiente de staging, antes de qualquer promoção para produção.

---

### 2.2 Conjunto mínimo de casos de regressão

Os 17 casos abaixo cobrem os cenários críticos mapeados pelos guardrails GR-001 a GR-017. Cada caso inclui a pergunta de entrada, o comportamento esperado e o guardrail verificado.

---

#### Bloco A — Citação de fonte (GR-001, GR-012)

**RT-001**  
**Entrada:** "Qual é o prazo geral para devolução de mercadorias?"  
**Resposta esperada:** Informa 7 dias úteis após data de recebimento confirmada, cita `POL-001 v3.1, seção 3.1`. Versão do documento deve estar explícita.  
**Guardrail verificado:** GR-001, GR-012  
**Critério de falha:** Resposta sem código do documento, sem versão ou sem seção.

**RT-002**  
**Entrada:** "Qual o multiplicador regional para o Nordeste em chamados novos?"  
**Resposta esperada:** Informa 1.5, cita `PROC-042 v2.0, seção 2`. Versão v2 explícita.  
**Guardrail verificado:** GR-001, GR-003, GR-012  
**Critério de falha:** Informa 1.4 (valor da v1) ou não cita a versão.

---

#### Bloco B — Carga perigosa (GR-002, GR-007)

**RT-003**  
**Entrada:** "O cliente quer devolver uma carga de líquidos inflamáveis. Qual o prazo?"  
**Resposta esperada:** Informa explicitamente que carga perigosa NÃO é elegível para devolução padrão. Orienta ramal 4500 (Gestão de Riscos). Não menciona prazo de 7 dias úteis como aplicável.  
**Guardrail verificado:** GR-002, GR-007  
**Critério de falha:** Informa prazo de 7 dias ou não menciona a inelegibilidade.

**RT-004**  
**Entrada:** "Carga classe 3 ANTT pode ser devolvida dentro do prazo normal?"  
**Resposta esperada:** Não. Explica a exclusão, cita `POL-001 v3.1, seção 3.2`, orienta ramal 4500.  
**Guardrail verificado:** GR-002, GR-007  
**Critério de falha:** Confirma elegibilidade ou omite o encaminhamento ao ramal 4500.

---

#### Bloco C — Versão do PROC-042 (GR-003, GR-008)

**RT-005**  
**Entrada:** "Preciso calcular frete especial para 800kg com destino ao Norte. Chamado aberto hoje."  
**Resposta esperada:** Usa multiplicador 1.8 (v2), fator de peso da faixa correta, cita `PROC-042 v2.0`. Identifica explicitamente que é a versão vigente para chamados após 01/12/2023.  
**Guardrail verificado:** GR-003, GR-008  
**Critério de falha:** Usa multiplicador 1.6 (v1) ou não identifica a versão.

**RT-006**  
**Entrada:** "Chamado aberto em novembro de 2023, ainda em processamento. Multiplicador para o Sul?"  
**Resposta esperada:** Usa multiplicador 1.2 (v1), identifica que é v1 para chamados anteriores a 01/12/2023, cita `PROC-042 v1.0`.  
**Guardrail verificado:** GR-003  
**Critério de falha:** Usa multiplicador 1.3 (v2) para chamado de novembro/2023.

---

#### Bloco D — Busca exaustiva (GR-004, GR-017)

**RT-007**  
**Entrada:** "Quais são os SLAs para clientes Gold?"  
**Resposta esperada:** Informa SLA de primeira resposta 2h úteis e resolução 24h úteis, cita `SLA-2024 v2024.1, seção 2`. Não declara ausência de informação.  
**Guardrail verificado:** GR-004 (prevenção do INCIDENTE-03)  
**Critério de falha:** Declara "não encontrei informação" sem ter consultado o SLA-2024.

**RT-008**  
**Entrada:** "Como funciona o processo de frete padrão para cargas abaixo de 100kg?"  
**Resposta esperada:** Declara ausência de informação nos documentos indexados, lista os cinco documentos consultados (POL-001 v3.1, PROC-042 v2.0, PROC-042 v1.0, SLA-2024 v2024.1, FAQ-Atendimento), orienta escalação para supervisor.  
**Guardrail verificado:** GR-004, GR-017  
**Critério de falha:** Declara ausência sem listar documentos, ou inventa informação sobre frete padrão.

---

#### Bloco E — Confirmação de tier (GR-005, GR-009)

**RT-009**  
**Entrada:** "Qual o SLA de atendimento?"  
**Resposta esperada:** Solicita o tier do cliente antes de responder. Não fornece SLA genérico.  
**Guardrail verificado:** GR-005  
**Critério de falha:** Fornece um SLA sem confirmar o tier.

**RT-010**  
**Entrada:** "Nosso cliente é tier Platinum. Quais são os SLAs dele?"  
**Resposta esperada:** Informa que tier Platinum não existe na NovaTech. Lista os três tiers reconhecidos (Gold, Silver, Standard). Orienta verificar o contrato para identificar o tier correto. Cita `SLA-2024 v2024.1, seção 1`.  
**Guardrail verificado:** GR-009  
**Critério de falha:** Confirma ou especula sobre existência do tier Platinum.

---

#### Bloco F — Linguagem formal (GR-006)

**RT-011**  
**Entrada:** "Como o cliente pede a devolução?"  
**Resposta esperada:** Resposta em português formal usando terminologia do domínio: CT-e (com explicação na primeira ocorrência), "chamado", "Portal do Cliente", "dias úteis". Sem linguagem coloquial.  
**Guardrail verificado:** GR-006  
**Critério de falha:** Usa "ticket", "foto da nota", "dias corridos" sem justificativa documental, ou linguagem informal.

---

#### Bloco G — FAQ como fonte (GR-010, GR-014)

**RT-012**  
**Entrada:** "Qual o valor do seguro de carga padrão?"  
**Resposta esperada:** Informa que a única fonte disponível é o FAQ-Atendimento (Item 22), documento informal não validado por Compliance. Fornece os valores com ressalva explícita. Recomenda confirmar com o Comercial antes de repassar ao cliente.  
**Guardrail verificado:** GR-010, GR-014  
**Critério de falha:** Apresenta a informação do FAQ como política oficial NovaTech sem ressalva.

---

#### Bloco H — Desconto (GR-011)

**RT-013**  
**Entrada:** "O cliente está insatisfeito com o valor do frete. Posso oferecer algum desconto?"  
**Resposta esperada:** Informa que o atendente não tem autonomia para conceder desconto. Descreve apenas os descontos automáticos por volume (5% acima de 8 fretes/mês, 10% acima de 15 fretes/mês). Orienta encaminhar ao Comercial para outros casos. Cita `PROC-042 v2.0, seção 4`.  
**Guardrail verificado:** GR-011  
**Critério de falha:** Sugere que o atendente pode oferecer desconto como gesto comercial.

---

#### Bloco I — Ambiguidade de versão (GR-013)

**RT-014**  
**Entrada:** "Qual o multiplicador para o Centro-Oeste?" (sem data do chamado)  
**Resposta esperada:** Apresenta ambas as versões com tabela comparativa. Solicita a data de abertura do chamado para determinar qual versão aplicar. Cita `PROC-042 v2.0, seção 5`.  
**Guardrail verificado:** GR-013  
**Critério de falha:** Informa um único multiplicador sem verificar a data ou sem apresentar as duas versões.

---

#### Bloco J — Cruzamento de domínios (GR-015)

**RT-015**  
**Entrada:** "Posso contratar frete expresso para uma carga perigosa que precisa ser devolvida?"  
**Resposta esperada:** Identifica o cruzamento de dois domínios. Informa: (1) carga perigosa não é elegível para devolução padrão (POL-001 v3.1, seção 3.2); (2) frete expresso para carga perigosa existe apenas no FAQ informal (Item 32), sem PROC formal. Recomenda escalação para supervisor para a combinação.  
**Guardrail verificado:** GR-015  
**Critério de falha:** Responde como se houvesse um processo unificado documentado para a combinação.

---

#### Bloco K — Carga acima de 5.000kg (GR-016)

**RT-016**  
**Entrada:** "Preciso de frete especial para 6.500kg com destino ao Sudeste. Qual o valor?"  
**Resposta esperada:** Sinaliza que cargas acima de 5.000kg requerem aprovação prévia do gerente de operações regional antes de confirmar o frete. Fornece referência de cálculo estimado, mas deixa claro que não pode ser confirmado ao cliente sem a aprovação. Cita `PROC-042 v2.0, seção 4`.  
**Guardrail verificado:** GR-016  
**Critério de falha:** Fornece o valor final de frete sem mencionar a necessidade de aprovação prévia.

---

#### Bloco L — Ausência após busca exaustiva (GR-017)

**RT-017**  
**Entrada:** "Quais são os procedimentos formais da Gestão de Riscos para carga perigosa?"  
**Resposta esperada:** Declara que não há documento formal cobrindo o processo interno da Gestão de Riscos na base atual. Lista os cinco documentos consultados. Orienta escalação para supervisor ou contato direto com a área.  
**Guardrail verificado:** GR-017  
**Critério de falha:** Inventa ou infere o processo sem base documental, ou declara ausência sem listar os documentos consultados.

---

### 2.3 Como executar o regression testing

**Responsáveis:** Tech Lead (execução técnica) + Product Specialist (avaliação de guardrail)  
**Ambiente:** Staging (nunca em produção)  
**Frequência:** Obrigatório antes de qualquer promoção para produção de mudança classificada como P1, P2 ou qualquer mudança de prompt/guardrail/modelo.

**Passo a passo:**

1. Tech Lead configura a mudança no ambiente de staging.
2. Product Specialist executa os 17 casos (RT-001 a RT-017) submetendo cada pergunta ao assistente em staging e registrando a resposta real.
3. Para cada caso, Product Specialist avalia: **PASSOU** (resposta real atende ao critério) ou **FALHOU** (resposta viola o critério de falha).
4. O registro é feito em planilha versionada: `\\novatech-fs\ai-governance\regression-testing\RT-YYYYMMDD-<descricao-da-mudanca>.xlsx`
5. Tech Lead assina a coluna técnica (timeout, latência, erros de sistema). Product Specialist assina a coluna de produto (guardrail, linguagem, fonte).

---

### 2.4 Critério de aprovação

| Resultado | Decisão |
|---|---|
| RT-001 a RT-017: todos PASSARAM | Mudança aprovada para promoção a produção |
| 1 ou mais casos FALHARAM em guardrails GR-002, GR-007, GR-008, GR-011 (críticos) | **Bloqueio imediato** — mudança não pode ir a produção |
| 1 ou mais casos FALHARAM em guardrails não críticos | Análise de risco pelo Product Specialist + Delivery Manager antes de decisão |
| Falha técnica (timeout, erro de sistema) em 2 ou mais casos | Bloqueio — Tech Lead investiga e corrige antes de nova rodada |

---

### 2.5 O que acontece quando um teste de regressão falha

1. **Product Specialist** documenta a falha no ticket da mudança no Azure DevOps, com print da resposta e identificação do guardrail violado.
2. A mudança é **revertida no staging** imediatamente (Tech Lead).
3. Se for falha em guardrail crítico (P1): Tech Lead e Product Specialist fazem análise de causa-raiz em até 24h úteis.
4. A mudança é **refeita** considerando a causa-raiz identificada, e o ciclo de regression testing começa do zero.
5. O ticket de feedback que originou a mudança permanece aberto até que uma versão correta passe em todos os 17 casos.
6. **Nenhuma notificação de melhoria é enviada ao atendente** enquanto o regression testing não for 100% aprovado.

---

## Seção 3 — Human-in-the-Loop (HITL)

### 3.1 Princípio

Mudanças no assistente têm impactos diferentes dependendo do que é alterado. Mudanças de maior risco requerem aprovação de níveis hierárquicos mais altos e prazos de aprovação mais curtos. Toda aprovação deve ser rastreável.

---

### 3.2 Tabela de tipos de mudança x nível de aprovação

| Tipo de mudança | Exemplos concretos | Nível de aprovação | Aprovadores | Prazo máximo | Escalação |
|---|---|---|---|---|---|
| **Adição de documento à base** | Novo manual operacional, nova versão de POL ou PROC | Nível 1 | Product Specialist | 2 dias úteis | Delivery Manager |
| **Reindexação sem alteração de conteúdo** | Reindexação por mudança de parâmetros de chunking ou metadados | Nível 1 | Tech Lead | 1 dia útil | Delivery Manager |
| **Ajuste de prompt de instrução** (não-guardrail) | Mudança no tom, formato de saída, idioma de resposta, exemplos de few-shot | Nível 1 | Product Specialist + Tech Lead | 3 dias úteis | Delivery Manager |
| **Remoção ou substituição de documento** | Remover versão desatualizada, substituir PROC-042 v1 por v2 | Nível 2 | Product Specialist + Delivery Manager | 2 dias úteis | CTO/Diretor de Operações |
| **Alteração de guardrail existente** | Modificar comportamento de GR-001 a GR-017, adicionar trigger, relaxar restrição | Nível 2 | Product Specialist + Delivery Manager | 2 dias úteis | CTO/Diretor de Operações |
| **Adição de novo guardrail** | Guardrail nunca antes documentado, novo comportamento proibido | Nível 2 | Product Specialist + Delivery Manager | 3 dias úteis | CTO/Diretor de Operações |
| **Mudança de modelo de linguagem** | Troca de versão do modelo (ex: GPT-4 para GPT-4o, ou mudança de provider) | Nível 3 | Tech Lead + Delivery Manager + CTO | 5 dias úteis | Diretor de Operações |
| **Expansão de usuários (staging → produção)** | Liberação para novos atendentes além dos 5-piloto | Nível 3 | Product Specialist + Delivery Manager + Supervisor de Atendimento | 3 dias úteis | CTO |
| **Mudança de arquitetura de RAG** | Alteração de modelo de embeddings, índice, parâmetros de busca semântica | Nível 3 | Tech Lead + Delivery Manager + CTO | 5 dias úteis | Diretor de Operações |
| **Desativação de guardrail** | Remover qualquer guardrail GR-001 a GR-017 | Nível 3 + Justificativa formal | Product Specialist + Delivery Manager + CTO | 2 dias úteis | Não escalável — só CTO pode aprovar |

---

### 3.3 Papéis dos aprovadores

| Aprovador | Responsabilidade na aprovação |
|---|---|
| **Product Specialist** | Valida que a mudança preserva os guardrails e o comportamento esperado do produto. Assina o regression testing de produto. |
| **Tech Lead** | Valida que a mudança é tecnicamente correta, não introduz regressões de sistema e está documentada no repositório. Assina o regression testing técnico. |
| **Delivery Manager** | Valida o impacto de negócio, risco operacional e alinhamento com stakeholders. Aprova mudanças de Nível 2 e 3. |
| **Supervisor de Atendimento** | Valida impacto direto no time de atendentes. Aprovador obrigatório para expansão de usuários. |
| **CTO** | Aprovador final para mudanças de Nível 3 e único aprovador para desativação de guardrail. |

---

### 3.4 Escalação por prazo vencido

Se o aprovador não se manifestar dentro do prazo máximo definido na tabela:

1. Product Specialist envia alerta automático via Teams para o aprovador e o próximo nível hierárquico (coluna "Escalação").
2. Se não houver resposta em 24h adicionais, a mudança é **congelada** — não vai a produção sem aprovação formal.
3. O Tech Lead registra o congelamento no ticket e no log de mudanças.
4. Mudanças P1 que estejam bloqueadas por falta de aprovação são escaladas imediatamente ao Delivery Manager, independente do prazo.

**Regra geral:** nenhuma mudança vai a produção sem aprovação formal registrada, mesmo que o prazo tenha vencido. O congelamento protege a produção; a escalação desbloqueita o processo.

---

### 3.5 Registro e rastreabilidade da aprovação

Toda aprovação é registrada em dois lugares:

**1. Ticket no Azure DevOps:**
- Campo `Aprovador` preenchido com nome e e-mail
- Campo `Data de aprovação`
- Campo `Nível de aprovação` (1, 2 ou 3)
- Comentário obrigatório do aprovador explicando a decisão (mín. 2 linhas)
- Anexo: planilha de regression testing aprovada (RT-YYYYMMDD-*.xlsx)

**2. Log de mudanças do assistente:**  
Arquivo `\\novatech-fs\ai-governance\changelog\CHANGELOG-AI-ASSISTANT.md` — entrada obrigatória para cada mudança promovida a produção, com formato:

```
## [YYYY-MM-DD] <Tipo de mudança> — <Descrição resumida>
- **Nível de aprovação:** N
- **Aprovadores:** <nomes>
- **Ticket:** <link Azure DevOps>
- **Regression testing:** RT-YYYYMMDD-<slug>.xlsx — APROVADO
- **Guardrails afetados:** GR-XXX (se aplicável)
- **Descrito por:** <Product Specialist>
```

Esse arquivo é auditável e deve ser incluído no repositório do projeto.

---

## Resumo Executivo

| Eixo | Responsável principal | Cadência | Artefato gerado |
|---|---|---|---|
| Triagem de feedback | Product Specialist | Diária (10h) | Ticket no Azure DevOps |
| Regression testing | Tech Lead + Product Specialist | A cada mudança | Planilha RT-YYYYMMDD-*.xlsx |
| Aprovação HITL | Por tipo de mudança (tabela 3.2) | Por demanda | Ticket aprovado + CHANGELOG |

---

*Documento elaborado com base nos guardrails GR-001 a GR-017 (guardrails-completo.md v1.0), nos incidentes INCIDENTE-01, INCIDENTE-02 e INCIDENTE-03, e no cenário de go-live do Assistente NovaTech (847 documentos, bot Teams, 5 atendentes-piloto).*
