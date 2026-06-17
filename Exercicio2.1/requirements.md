# requirements.md — Query Endpoint
**Módulo:** Copiloto de Atendimento — Query Endpoint  
**Versão:** 1.0  
**Data:** 2026-06-13  
**Autor:** Product Specialist  
**Status:** Rascunho — aguarda aprovação do Tech Lead (Gate 1: Spec → Plan)

---

## 1. Outcomes

O que muda na vida do atendente quando este endpoint funciona bem:

1. **O atendente resolve a dúvida sem abrir fontes adicionais.** Hoje são em média 4 fontes por chamado; com o endpoint funcionando, a resposta fundamentada chega em uma única consulta.

2. **O atendente reduz o tempo de chamado de 12 para 2 minutos.** A busca e consolidação da informação deixam de ser manuais — o atendente pergunta, recebe resposta com fonte e orienta o cliente.

3. **O atendente não precisa escalar para o supervisor por falta de informação.** Os 15% de chamados que hoje sobem por não localização da resposta passam a ser tratados pelo próprio atendente — ou recebem indicação explícita de encaminhamento, eliminando a escalação por desorientação.

4. **O atendente sabe em qual documento e seção a resposta está baseada.** A indicação de fonte permite que o atendente confirme a informação ou cite a referência ao cliente final com segurança.

5. **O atendente é protegido de responder com informação desatualizada.** Quando existem duas versões ativas de um mesmo documento (ex.: PROC-042 v1 e v2), o endpoint sinaliza a contradição e indica qual versão deve ser usada para chamados novos.

---

## 2. Scope Boundaries

### Dentro do escopo

Este módulo cobre o contexto **"Copiloto de Atendimento"** — responsável por orquestrar a busca semântica, montar o prompt com os chunks recuperados, acionar o modelo de linguagem e retornar a resposta fundamentada com indicação de fonte.

Este módulo cobre o contexto **"Central de Atendimento"** — responsável por receber a pergunta do atendente em linguagem natural e exibir a resposta com a fonte da informação. O query endpoint é o serviço de backend que viabiliza esse contexto.

Este módulo cobre, como **conteúdo das respostas**, os seguintes contextos de negócio:

- **"Mesa de Operações"** — regras de frete especial, multiplicador regional, fator de peso, prazos de entrega, devolução, coleta reversa, carga perigosa, carga refrigerada.
- **"Gestão de Nível de Serviço"** — tiers de cliente (Gold, Silver, Standard), SLA de resposta, SLA de resolução, incidente crítico, penalidades.

Este módulo cobre, como **fonte de recuperação**, o contexto **"Base de Conhecimento"** — fornece os chunks e metadados de vigência ao Copiloto de Atendimento.

### Fora do escopo

| O que está fora | Motivo | Encaminhamento correto |
|----------------|--------|----------------------|
| Cálculo de frete em tempo real | Depende da tabela mensal `frete-base-AAAAMM.xlsx`, não indexada | Comercial ou sistema de cotação |
| Abertura de chamado no Portal do Cliente | Ação operacional fora do escopo do assistente | Portal do Cliente (portal.novatech.com.br) |
| Autorização de descontos e negociações | Atendente não tem autonomia; regras comerciais não estão nos documentos indexados | Comercial com justificativa |
| Rastreamento de carga em tempo real | Sem integração com sistema de tracking | Portal do Cliente ou sistema de chamados |
| Tratamento de carga danificada em trânsito | Processo passa pelo Jurídico; gap documental (só FAQ informal) | sinistros@novatech.com.br |
| Confirmação de seguro de carga | Apenas FAQ informal; sem documento normativo indexado | Comercial |
| Resposta sobre frete padrão (abaixo de 500 kg) | Gap documental: não existe PROC indexada para essa modalidade | Tabela de fretes padrão com o Comercial |
| Devolução de carga perigosa (classes 1–6 ANTT) pelo processo padrão | POL-001 é explícita: não elegível | Gestão de Riscos — ramal 4500 |
| Exceções fora dos documentos indexados | Guardrail: nunca inventar ou extrapolar | Escalar para supervisor |

### Fronteiras do assistente: o que ele responde vs. o que encaminha para humano

**Responde diretamente** (com fonte):
- Regras de devolução de mercadorias (POL-001)
- Cálculo de frete especial: multiplicador regional e fator de peso (PROC-042 v1 e v2)
- Prazos de entrega para frete especial (PROC-042 v1 e v2)
- Tiers de cliente e SLAs correspondentes (SLA-2024)
- Critérios de incidente crítico e penalidades (SLA-2024)
- Dúvidas frequentes do time de atendimento (FAQ — com ressalva de fonte não normativa)

**Encaminha para humano** (não responde diretamente):
- Qualquer situação que exija ação operacional (abertura de chamado, autorização, rastreamento)
- Respostas com confiança abaixo do limiar definido — inclui aviso de baixa confiança e orienta escalação
- Perguntas cujo conteúdo não existe nos documentos indexados (gap documental)

---

## 3. Constraints

### Técnicas e operacionais

| # | Constraint | Origem |
|---|-----------|--------|
| C-01 | O endpoint nunca retorna informação não presente nos documentos indexados (guardrail absoluto) | ADR-003, Spec RAG |
| C-02 | Toda resposta deve incluir identificador do documento e seção de origem (`source_document`) | ADR-002 |
| C-03 | Tempo de resposta ao atendente: ≤ 30 segundos para 95% das consultas | Discovery (requisito de negócio) |
| C-04 | Context budget: ~4.000 tokens para system prompt + ~8.000 tokens para chunks (5 chunks de ~1.500 tokens cada) | ADR-0002 |
| C-05 | Quando há contradição documental, o endpoint exibe as duas versões sem arbitrar qual é correta, sinalizando a versão mais recente pelo metadado de vigência | ADR-003 |
| C-06 | A base de conhecimento deve ser atualizada em até 24 horas após publicação ou atualização de um documento oficial | Spec RAG |
| C-07 | Respostas são sempre em português formal | Spec RAG |
| C-08 | O endpoint é somente leitura sobre o domínio de negócio: nunca escreve, decide ou executa ações operacionais | Recorte de domínio |

### Regulatórias e de compliance

| # | Constraint | Origem |
|---|-----------|--------|
| C-09 | O assistente nunca deve afirmar que carga perigosa (classes 1–6 ANTT) pode ser devolvida pelo processo padrão | POL-001, seção 3.2 |
| C-10 | Documentos normativos (POL, PROC, SLA) têm prioridade sobre o FAQ. Respostas baseadas exclusivamente no FAQ devem incluir aviso de "fonte não normativa" | Fronteira crítica: Gestão Documental ↔ Atendimento |
| C-11 | O assistente nunca inventa tiers de cliente. Somente existem: Gold, Silver e Standard | SLA-2024, seção 1 |

---

## 4. Functional Requirements

### RF-001 — Recepção de pergunta em linguagem natural

**Bounded context:** Central de Atendimento  
**Descrição:** O endpoint recebe a pergunta do atendente em linguagem natural via POST, valida o input e inicia o pipeline de recuperação.

**Critério de aceitação:**
> Dado que o atendente envia uma pergunta com texto não vazio,  
> quando o endpoint recebe a requisição,  
> então o pipeline de busca semântica é acionado e uma resposta fundamentada é retornada em ≤ 30 segundos.

---

### RF-002 — Busca semântica e recuperação de chunks

**Bounded context:** Copiloto de Atendimento / Base de Conhecimento  
**Descrição:** O endpoint converte a pergunta em embedding, realiza busca vetorial no índice e recupera os chunks mais relevantes, respeitando o context budget.

**Critério de aceitação:**
> Dado que o atendente pergunta "qual o prazo para devolução de mercadoria?",  
> quando o endpoint executa a busca vetorial,  
> então ao menos um chunk do documento POL-001 é recuperado e incluído no contexto de geração.

---

### RF-003 — Geração de resposta fundamentada com indicação de fonte

**Bounded context:** Copiloto de Atendimento  
**Descrição:** O endpoint monta o prompt com os chunks recuperados e retorna a resposta acompanhada de `source_document` (identificador do documento, versão e seção). A resposta reflete apenas o conteúdo presente nos chunks.

**Critério de aceitação:**
> Dado que o atendente pergunta sobre o SLA de resolução de um cliente Gold,  
> quando o endpoint retorna a resposta,  
> então o payload inclui o campo `source_document` preenchido com referência ao SLA-2024 e a seção correspondente, e o valor informado (24h) coincide com o documento.

---

### RF-004 — Aviso de baixa confiança

**Bounded context:** Copiloto de Atendimento  
**Descrição:** Quando o assistente não localiza resposta com grau de confiança suficiente nos documentos indexados, o endpoint retorna resposta com aviso explícito de baixa confiança e orienta o atendente a escalar para o supervisor.

**Critério de aceitação:**
> Dado que o atendente pergunta sobre um assunto sem cobertura documental (ex.: frete padrão abaixo de 500 kg),  
> quando o endpoint processa a pergunta,  
> então a resposta inclui aviso de baixa confiança, não apresenta valores inventados, e indica o encaminhamento correto (Comercial).

---

### RF-005 — Tratamento de contradição documental

**Bounded context:** Base de Conhecimento ↔ Mesa de Operações  
**Descrição:** Quando dois documentos ativos apresentam informações divergentes (ex.: PROC-042 v1 e PROC-042 v2 com multiplicadores regionais diferentes), o endpoint exibe as informações de ambas as versões, sinaliza a contradição e indica que chamados novos (a partir de 01/12/2023) devem usar a versão mais recente (v2).

**Critério de aceitação:**
> Dado que o atendente pergunta sobre o multiplicador regional para a região Norte,  
> quando o endpoint recupera chunks de PROC-042 v1 (fator 1.6) e PROC-042 v2 (fator 1.8),  
> então a resposta apresenta os dois valores, identifica qual versão corresponde a cada um, informa que a v2 é a versão mais recente para chamados a partir de 01/12/2023, e não elege arbitrariamente apenas um valor como correto.

---

### RF-006 — Guardrail: carga perigosa e devolução

**Bounded context:** Mesa de Operações ↔ Central de Atendimento  
**Descrição:** Toda pergunta que relacione carga perigosa (classes 1–6 ANTT) com devolução deve retornar resposta negativa explícita, com base na POL-001, e indicar encaminhamento para Gestão de Riscos (ramal 4500). O assistente nunca afirma que existe exceção pelo processo padrão, independentemente do conteúdo do FAQ.

**Critério de aceitação:**
> Dado que o atendente pergunta "posso devolver uma carga de gás liquefeito?",  
> quando o endpoint processa a pergunta,  
> então a resposta afirma que cargas perigosas (classes 1–6 ANTT) não são elegíveis para devolução pelo processo padrão (citando POL-001 seção 3.2), e orienta o contato com Gestão de Riscos no ramal 4500. A resposta não menciona a possibilidade de "exceção" baseada no FAQ.

---

### RF-007 — Prioridade de fonte normativa sobre FAQ

**Bounded context:** Base de Conhecimento ↔ Central de Atendimento  
**Descrição:** Quando chunks de documentos normativos (POL, PROC, SLA) e chunks do FAQ cobrem o mesmo assunto com informações divergentes, o endpoint prioriza os documentos normativos. Respostas baseadas exclusivamente no FAQ incluem aviso de "fonte não normativa".

**Critério de aceitação:**
> Dado que o atendente pergunta sobre devolução de carga perigosa e o FAQ sugere que pode haver exceções,  
> quando o endpoint recupera chunks de POL-001 e do FAQ,  
> então a resposta é fundamentada na POL-001 (normativa) e não reproduz a sugestão de exceção do FAQ. Quando o único chunk disponível for do FAQ, a resposta inclui o aviso "informação baseada em fonte não normativa — confirmar com a área responsável".

---

### RF-008 — Encaminhamento para fora do escopo

**Bounded context:** Central de Atendimento  
**Descrição:** Para perguntas cujo assunto está fora do escopo do assistente (gap documental, ação operacional requerida, ou cálculo em tempo real), o endpoint retorna mensagem padrão indicando que a informação não está disponível na base de conhecimento e orienta o encaminhamento correto.

**Critério de aceitação:**
> Dado que o atendente pergunta "qual é o valor do frete para uma carga de 200 kg para São Paulo?",  
> quando o endpoint processa a pergunta,  
> então a resposta informa que frete padrão (abaixo de 500 kg) não está coberto pelos documentos indexados e orienta a consultar o Comercial ou o sistema de cotação. Nenhum valor de frete é apresentado.

---

### RF-009 — Informação sobre tiers de cliente

**Bounded context:** Gestão de Nível de Serviço ↔ Central de Atendimento  
**Descrição:** O assistente responde perguntas sobre tiers de cliente usando exclusivamente os tiers definidos no SLA-2024: Gold, Silver e Standard. Quando o atendente mencionar tier inexistente (ex.: "Platinum"), o assistente corrige e informa os tiers válidos.

**Critério de aceitação:**
> Dado que o atendente pergunta "meu cliente diz que é Platinum, qual o SLA dele?",  
> quando o endpoint processa a pergunta,  
> então a resposta informa que o tier "Platinum" não existe na NovaTech, que os tiers são Gold, Silver e Standard, e orienta o atendente a verificar o contrato do cliente para identificar o tier correto.

---

## 5. Non-Functional Requirements

### RNF-001 — Tempo de resposta

O endpoint deve retornar a resposta completa (texto + `source_document`) em **≤ 30 segundos** para 95% das consultas em condições normais de operação.

Threshold de alerta: consultas acima de 20 segundos devem ser registradas para análise de degradação.

### RNF-002 — Disponibilidade

[A DEFINIR] — percentual de disponibilidade e janela de manutenção não foram definidos nesta fase. Impacto: ausência deste SLA impede que o time de infraestrutura configure monitoramento e alertas adequados.

### RNF-003 — Rastreabilidade de fonte

Toda resposta retornada pelo endpoint deve incluir o campo `source_document` no payload, contendo:
- `document_id`: identificador único do documento (ex.: `POL-001`, `PROC-042-v2`, `SLA-2024`)
- `section`: seção ou título do trecho citado (ex.: `seção 3.2`)
- `version`: versão do documento (ex.: `v2`, `2024.1`)
- `is_normative`: booleano indicando se o documento é normativo (POL, PROC, SLA) ou informal (FAQ)

O campo `source_document` é **obrigatório** — mesmo em respostas de baixa confiança ou fora do escopo, deve estar presente (pode indicar ausência de fonte com valor explícito `null` e campo `low_confidence: true`).

### RNF-004 — Atualização da base de conhecimento

Um documento publicado ou atualizado na fonte oficial (SharePoint) deve estar disponível para consulta pelo endpoint em **até 24 horas** após a publicação.

Documentos marcados como obsoletos devem deixar de ser retornados como fonte primária, mas continuam indexados para suporte a chamados históricos com metadado de vigência inativo.

### RNF-005 — Context budget

O prompt enviado ao modelo de linguagem deve respeitar o budget de tokens definido na ADR-0002:
- System prompt: ≤ 4.000 tokens
- Chunks recuperados: ≤ 8.000 tokens (aproximadamente 5 chunks de ~1.500 tokens)
- Pergunta do atendente + histórico: limitado a 3 turnos anteriores

Consultas que não caibam no budget devem priorizar os chunks com maior score de relevância semântica, descartando os menos relevantes.

### RNF-006 — Idioma e registro

Todas as respostas retornadas ao atendente devem estar em **português formal**. O endpoint não retorna respostas em outros idiomas, mesmo que a pergunta seja submetida em outro idioma — nesse caso, retorna aviso de que o assistente opera exclusivamente em português.

---

## 6. Prior Decisions (ADRs)

### ADR-001 — RAG como estratégia de recuperação

**Decisão:** O assistente utiliza Retrieval-Augmented Generation (RAG) como estratégia de recuperação de informações, com Azure AI Search como índice vetorial e Azure OpenAI (GPT-4o) como modelo de linguagem.

**Motivo:** Baixo custo de atualização — a base documental pode ser re-indexada sem retreinamento do modelo. Validado pelo protótipo open-source (ChromaDB + sentence-transformers) na fase anterior.

**Impacto direto neste endpoint:**
- A base documental deve estar indexada e versionada antes de cada consulta.
- O endpoint depende da disponibilidade do índice Azure AI Search; sem o índice, nenhuma resposta pode ser gerada.
- O pipeline de ingestão é uma dependência de infraestrutura deste endpoint.

---

### ADR-002 — Toda resposta com citação de fonte (rastreabilidade obrigatória)

**Decisão:** O endpoint retorna obrigatoriamente os metadados de fonte (`source_document`) junto à resposta, em todo e qualquer cenário.

**Motivo:** Rastreabilidade obrigatória — o atendente precisa citar a base normativa ao orientar o cliente; respostas sem fonte não podem ser usadas com segurança operacional.

**Impacto direto neste endpoint:**
- O contrato do endpoint (schema de resposta) inclui `source_document` como campo obrigatório.
- A ausência de fonte é representada explicitamente (`null` + `low_confidence: true`), nunca por omissão silenciosa.
- O QA deve verificar 100% das respostas para presença do campo.

---

### ADR-003 — Fontes contraditórias exibidas sem arbitragem

**Decisão:** Quando dois documentos ativos apresentam informações divergentes, o endpoint exibe ambas as versões. O modelo não elege uma versão como correta — o metadado de vigência indica qual é a mais recente, e a regra de priorização é informada ao atendente (chamados novos → v2).

**Motivo:** Evitar risco regulatório — eleger automaticamente uma versão como correta e descartar a outra é uma decisão que cabe ao Compliance da NovaTech, não ao assistente.

**Impacto direto neste endpoint:**
- O endpoint deve detectar, via metadado de vigência, quando dois chunks de versões diferentes do mesmo documento são recuperados.
- A resposta deve apresentar os valores divergentes identificados por versão e aplicar a regra de transição do PROC-042-v2 (chamados abertos antes de 01/12/2023 → v1; chamados novos → v2).
- O sistema de prompt deve instruir o modelo a nunca omitir a contradição.

---

### ADR-0001 — Azure OpenAI (GPT-4o) como modelo de linguagem

**Decisão:** O modelo de linguagem adotado é o GPT-4o via Azure OpenAI.

**Motivo:** Integração nativa com o ecossistema Microsoft (Teams + SharePoint) da NovaTech; janela de contexto de 128K tokens garante margem para futuras expansões do context budget.

**Impacto direto neste endpoint:**
- O endpoint chama exclusivamente a API Azure OpenAI; não há fallback para outros provedores nesta versão.
- A autenticação com Azure OpenAI deve usar credenciais gerenciadas (Managed Identity), não chaves hardcoded.

---

### ADR-0002 — Context budget por consulta

**Decisão:** Cada consulta ao endpoint usa o seguinte budget de tokens: ~4.000 para system prompt + ~8.000 para chunks (5 chunks de ~1.500 tokens) + pergunta + histórico limitado a 3 turnos.

**Motivo:** Equilíbrio entre profundidade de contexto e custo por consulta; validado no protótipo.

**Impacto direto neste endpoint:**
- O seletor de chunks deve aplicar ranking por relevância semântica e truncar ao budget definido.
- O system prompt é versionado em `/prompts/system-prompt.md` e seu tamanho não pode exceder 4.000 tokens.

---

### ADR-0003 — Metadado de vigência para documentos contraditórios

**Decisão:** Documentos obsoletos são marcados com metadado de vigência inativo, não excluídos do índice. O prompt instrui o modelo a priorizar a versão mais recente.

**Motivo:** Chamados históricos (abertos antes de 01/12/2023) ainda precisam ser resolvidos com os multiplicadores da v1; a exclusão do índice quebraria esses casos.

**Impacto direto neste endpoint:**
- O pipeline de ingestão deve persistir o campo `is_active_version` em todos os chunks.
- O endpoint deve filtrar e rotular chunks por versão ao montar o contexto, não misturá-los sem identificação.

---

## 7. Verification Criteria

Os critérios abaixo devem ser verificados pelo QA antes da aprovação do endpoint para produção.

### VC-01 — Resposta com fonte

> Dado que o atendente pergunta "qual o prazo para coleta reversa após aprovação de devolução?",  
> quando o endpoint retorna a resposta,  
> então o payload contém o campo `source_document` preenchido com `document_id: "POL-001"`, `section: "seção 3.3"`, e o prazo informado (2 dias úteis) coincide com o documento.

---

### VC-02 — Fonte contraditória (PROC-042 v1 vs. v2)

> Dado que o atendente pergunta "qual o multiplicador regional para frete especial com destino ao Norte?",  
> quando o endpoint processa a pergunta,  
> então a resposta apresenta os dois valores (1.6 da v1 e 1.8 da v2), identifica cada um pela versão correspondente, informa que chamados abertos a partir de 01/12/2023 devem usar a v2, e não apresenta apenas um valor como se não houvesse contradição.

---

### VC-03 — Pergunta fora do escopo (gap documental)

> Dado que o atendente pergunta "qual o valor do frete para uma carga de 300 kg para Recife?",  
> quando o endpoint processa a pergunta,  
> então a resposta informa que frete padrão (abaixo de 500 kg) não está coberto pelos documentos indexados, não apresenta nenhum valor numérico de frete, e orienta a consultar o Comercial. O campo `low_confidence: true` está presente no payload.

---

### VC-04 — Tempo de resposta

> Dado que 100 consultas simultâneas são enviadas ao endpoint em horário de pico,  
> quando o endpoint processa as consultas,  
> então 95 ou mais retornam resposta completa em ≤ 30 segundos, sem erros de timeout.

---

### VC-05 — Guardrail: carga perigosa e devolução

> Dado que o atendente pergunta "cliente tem carga de substância tóxica e quer devolver, como procedo?",  
> quando o endpoint processa a pergunta,  
> então a resposta afirma explicitamente que carga perigosa (classes 1–6 ANTT) não é elegível para devolução pelo processo padrão, cita POL-001 seção 3.2, orienta o contato com Gestão de Riscos (ramal 4500), e não menciona a possibilidade de exceções.

---

### VC-06 — Aviso de baixa confiança

> Dado que o atendente pergunta sobre um assunto sem cobertura documental (ex.: seguro de carga),  
> quando o endpoint não localiza chunks normativos relevantes,  
> então a resposta inclui aviso explícito de baixa confiança, não apresenta valores percentuais inventados, e orienta o atendente a confirmar com o Comercial. O campo `low_confidence: true` está presente no payload.

---

### VC-07 — Tier inexistente

> Dado que o atendente menciona que o cliente alega ser "Platinum",  
> quando o endpoint processa a pergunta,  
> então a resposta informa que o tier "Platinum" não existe na NovaTech, lista os tiers válidos (Gold, Silver, Standard), e orienta a verificar o número de contrato do cliente para identificar o tier correto. O campo `source_document` referencia o SLA-2024.

---

### VC-08 — Fonte não normativa (FAQ)

> Dado que o atendente pergunta sobre o processo de carga danificada em trânsito (assunto coberto apenas pelo FAQ),  
> quando o endpoint recupera chunks do FAQ,  
> então a resposta inclui o aviso "informação baseada em fonte não normativa — confirmar com a área responsável", orienta o envio para sinistros@novatech.com.br, e o campo `is_normative: false` está presente no `source_document`.

---

## 8. Open Questions

As questões abaixo não foram resolvidas nesta fase e bloqueiam ou arriscam a implementação se não forem decididas antes do início do desenvolvimento.

| # | Questão | Impacto se não decidido | Prazo sugerido |
|---|---------|------------------------|---------------|
| OQ-01 | Qual o limiar numérico de score de confiança que dispara o aviso de baixa confiança? (ex.: score < 0.75 no ranker) | Sem limiar definido, o comportamento de baixa confiança não pode ser implementado deterministicamente nem testado pelo QA | Antes do início do desenvolvimento do RF-004 |
| OQ-02 | Qual o percentual de disponibilidade do endpoint? (RNF-002 marcado como [A DEFINIR]) | Sem SLA de disponibilidade, o time de infra não consegue configurar alertas, redundância ou janela de manutenção | Gate 1: aprovação da spec |
| OQ-03 | Como o atendente é autenticado ao chamar o endpoint? (identidade do Teams, token Azure AD, ou outro mecanismo?) | Sem definição de autenticação, o endpoint não pode ser implementado com segurança e auditabilidade de quem fez cada consulta | Antes do início do desenvolvimento do RF-001 |
| OQ-04 | O histórico de consultas do atendente deve ser persistido? Se sim, por quanto tempo e em qual sistema? | Impacta o design do payload, a política de retenção de dados e eventuais requisitos de privacidade (LGPD) | [A DEFINIR] |
| OQ-05 | O endpoint deve suportar perguntas que cruzam dois contextos simultaneamente? (15% dos casos no discovery) | Se sim, o ranker e o montador de prompt precisam de lógica específica para multi-contexto; se não, a resposta deve orientar o atendente a fazer duas perguntas separadas | Gate 1: aprovação da spec |
| OQ-06 | Qual é o processo de atualização quando o Compliance arquivar formalmente o PROC-042 v1? O metadado de vigência é atualizado manualmente ou via automação? | Sem esse processo definido, a contradição PROC-042 v1/v2 persistirá indefinidamente e o guardrail de VC-02 continuará ativo mesmo após a resolução | [A DEFINIR pelo Compliance] |
