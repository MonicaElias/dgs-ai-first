# Proposta de Ajustes — Assistente IA NovaTech (Staging)

**Origem:** Avaliação consolidada (Monica Elias + Claude Sonnet 4.6)  
**Data:** 27/06/2026  
**Escopo:** Respostas #1, #2, #3, #4 e #6 (incorretas ou parcialmente corretas)

---

## Resposta #1 — Prazo de devolução para produtos standard

### 1. Identificação do Problema

| Campo | Detalhe |
|-------|---------|
| **Tipo de erro** | Fonte incorreta + Informação incompleta |
| **Risco operacional** | Médio |
| **O que falhou** | O assistente citou POL-001, **seção 3.2** (exceções ao prazo), quando o prazo padrão consta na **seção 3.1**. Além disso, o procedimento de abertura de chamado foi simplificado incorretamente: a política exige também o número do **CT-e** e **mínimo de 3 fotos específicas** (embalagem, etiqueta e conteúdo), não apenas "fotos". |

### 2. Proposta de Ajuste

**PROMPT**
> Instrução a adicionar no system prompt:
> _"Ao citar seções de documentos internos (POL, PROC, SLA), sempre confirme que a seção citada é aquela que define a regra, não a que lista exceções ou condições especiais. Ao descrever procedimentos de abertura de chamado, inclua todos os requisitos listados na política, sem omitir campos obrigatórios."_

**INTERFACE**
> Exibir ao atendente um **checklist de requisitos** embutido na resposta sobre devoluções, gerado dinamicamente a partir da POL-001, seção 3.1. O atendente vê: ☐ CT-e · ☐ Foto da embalagem · ☐ Foto da etiqueta · ☐ Foto do conteúdo. Isso remove a dependência de o assistente listar corretamente todos os campos.

**PIPELINE**
> Adicionar validação pós-geração: quando a resposta citar um número de seção de documento, verificar se o texto recuperado pelo RAG pertence de fato àquela seção. Se houver discrepância, rebaixar a confiança para Baixa e sinalizar revisão.

### 3. Critério de Validação

**Teste:** Fazer a mesma pergunta ("Qual o prazo de devolução para produtos standard?") em ambiente de staging após o ajuste.  
**Esperado:** A resposta cita POL-001, **seção 3.1**; lista explicitamente CT-e e as 3 categorias de foto; não menciona seção 3.2 como fonte do prazo.  
**Falha se:** A seção citada for 3.2 ou os requisitos de CT-e/fotos específicas estiverem ausentes.

---

## Resposta #2 — Prazo de resolução para cliente Silver

### 1. Identificação do Problema

| Campo | Detalhe |
|-------|---------|
| **Tipo de erro** | Informação incompleta |
| **Risco operacional** | Médio |
| **O que falhou** | A resposta informou "48h" sem o qualificador **"úteis"**, alterando o compromisso contratual. Também não distinguiu o SLA de chamados gerais (48h úteis) do SLA de **incidentes críticos (8h úteis)**, que tem tratamento completamente diferente e exige priorização imediata. |

### 2. Proposta de Ajuste

**PROMPT**
> Instrução a adicionar no system prompt:
> _"Ao informar prazos de SLA, sempre inclua o qualificador temporal exato conforme o documento (ex: 'horas úteis', 'dias corridos'). Quando o documento define SLAs distintos para tipos diferentes de chamado (ex: geral vs. incidente crítico), apresente ambos — nunca omita exceções com prazo diferente do padrão."_

**INTERFACE**
> Quando a resposta envolver SLA de um tier de cliente, exibir automaticamente uma **tabela de SLA resumida** do tier (recuperada do SLA-2024), com os campos: Chamado geral | Incidente crítico | Qualificador. Isso garante que o atendente veja o quadro completo independentemente do que o assistente textualizar.

**PIPELINE**
> Criar uma regra de validação semântica: sempre que a resposta contiver um número de prazo em horas seguido de referência a SLA, verificar se o texto do chunk recuperado contém o qualificador "úteis" ou "corridas". Se o qualificador existir no documento mas não na resposta gerada, acionar revisão automática antes da exibição.

### 3. Critério de Validação

**Teste:** Perguntar "Qual o SLA para cliente Silver com incidente crítico?" e "Qual o SLA padrão para cliente Silver?" em sequência.  
**Esperado:** Primeira pergunta → "8h úteis"; segunda → "48h úteis". Qualificador "úteis" presente em ambas.  
**Falha se:** Qualquer resposta omitir "úteis" ou apresentar 48h como prazo único sem mencionar a categoria de incidente crítico.

---

## Resposta #3 — Devolução de carga perigosa classe 3

### 1. Identificação do Problema

| Campo | Detalhe |
|-------|---------|
| **Tipo de erro** | Informação incompleta |
| **Risco operacional** | Médio |
| **O que falhou** | O assistente recomendou escalar para "supervisor" como encaminhamento para carga perigosa. A POL-001, seção 3.2, define explicitamente o contato correto: setor de **Gestão de Riscos, ramal 4500**. A escalada genérica ao supervisor cria um passo adicional de mediação desnecessário e sujeito a falha, dado que o fluxo já está documentado. |

### 2. Proposta de Ajuste

**PROMPT**
> Instrução a adicionar no system prompt:
> _"Quando a política definir um contato ou canal específico para um tipo de situação (ex: ramal, e-mail, setor), cite esse contato diretamente. Nunca substitua um encaminhamento específico documentado por uma escalada genérica ao supervisor, a menos que a política indique explicitamente que o supervisor é o ponto de contato."_

**INTERFACE**
> Para categorias de carga com restrição (classes ANTT 1–6), exibir um **banner de encaminhamento fixo** na interface do atendente com o contato correto: "Gestão de Riscos — Ramal 4500". Esse dado deve ser estático, não gerado pelo LLM, evitando variação na exibição.

**PIPELINE**
> Criar um dicionário de encaminhamentos obrigatórios mapeado por tipo de situação (ex: `carga_perigosa → Gestão de Riscos, ramal 4500`). No pós-processamento, verificar se a resposta gerada menciona o encaminhamento correto. Se mencionar apenas "supervisor" ou similar, substituir automaticamente pelo encaminhamento documentado antes da exibição.

### 3. Critério de Validação

**Teste:** Perguntar "Como proceder com uma devolução de carga perigosa classe 2?"  
**Esperado:** Resposta menciona "Gestão de Riscos" e "ramal 4500" como encaminhamento.  
**Falha se:** A resposta sugerir escalada ao supervisor sem citar o contato específico, ou omitir o ramal 4500.

---

## Resposta #4 — Política para carga danificada durante transporte

### 1. Identificação do Problema

| Campo | Detalhe |
|-------|---------|
| **Tipo de erro** | Alucinação + Informação incompleta |
| **Risco operacional** | **Alto** |
| **O que falhou** | O assistente afirmou com confiança Alta a existência de uma política formal de reembolso por danos que **não existe** (nenhum POL ou PROC cobre esse cenário). Nenhuma fonte foi citada. Além disso, omitiu as informações críticas do único documento existente (FAQ Item 38): prazo de **48h para registrar a ocorrência**, encaminhamento ao **Jurídico via sinistros@novatech.com.br**, e que o laudo técnico é opcional ("se possível"). A combinação de confiança Alta + ausência de fonte + informações fabricadas representa o padrão de risco mais alto do conjunto. |

### 2. Proposta de Ajuste

**PROMPT**
> Instrução a adicionar no system prompt:
> _"Nunca afirme a existência de uma política, procedimento ou regra formal sem citar o documento que a sustenta (POL-XXXX, PROC-XXXX, SLA-XXXX). Se a única referência disponível for um FAQ ou documento informal, declare isso explicitamente e rebaixe a confiança para Baixa. Se não houver nenhuma fonte, responda que o processo não está documentado formalmente e oriente o atendente a escalar para a área responsável."_

**INTERFACE**
> Implementar **bloqueio de exibição** para respostas que declarem confiança Alta sem nenhuma fonte formal citada. A resposta não é exibida ao atendente — em seu lugar, aparece: _"Informação pendente de verificação. Consulte o supervisor ou a área de Compliance antes de responder ao cliente."_

**PIPELINE**
> Adicionar uma regra de guarda crítica no pós-processamento:
> - Se `confiança = Alta` **E** `fonte_citada = ausente` → bloquear resposta e acionar revisão humana.
> - Se `confiança = Alta` **E** `fonte_citada = FAQ` (documento informal) → rebaixar automaticamente para `confiança = Baixa` e adicionar aviso de fonte não validada.
> - Registrar o evento em log de auditoria para análise de padrão de alucinação.

### 3. Critério de Validação

**Teste:** Perguntar "Qual a política para carga danificada durante transporte?" após os ajustes.  
**Esperado:** O assistente informa que não há documento formal (POL/PROC), cita o FAQ Item 38 como fonte informal, menciona o prazo de 48h e o e-mail sinistros@novatech.com.br, e declara confiança Baixa.  
**Falha se:** A resposta afirmar qualquer política formal sem citar documento, declarar confiança Alta sem fonte, ou omitir o prazo de 48h.

---

## Resposta #6 — Envio de carga perigosa com frete expresso

### 1. Identificação do Problema

| Campo | Detalhe |
|-------|---------|
| **Tipo de erro** | Fonte não confiável |
| **Risco operacional** | **Alto** |
| **O que falhou** | A resposta autorizou o envio de carga perigosa com frete expresso com confiança Alta, baseando-se exclusivamente no **FAQ-Atendimento, item 32** — documento explicitamente não validado pelo Compliance. Não existe PROC ou POL que formalize esse processo. A operação envolve regulação ANTT, e uma orientação equivocada pode gerar não conformidade regulatória, multas e risco de segurança. Também foi omitida a informação de que na prática o processo leva ~2 dias para obter autorização. |

### 2. Proposta de Ajuste

**PROMPT**
> Instrução a adicionar no system prompt:
> _"Para qualquer operação envolvendo carga perigosa, material regulado pela ANTT, ou qualquer situação com implicação regulatória, a resposta deve ser baseada exclusivamente em documentos formais (POL, PROC). Documentos do tipo FAQ não são aceitos como fonte suficiente para autorizar ou orientar esse tipo de operação. Se não houver documento formal, informe que o processo não está formalmente definido e oriente o atendente a contatar o Compliance antes de prosseguir."_

**INTERFACE**
> Implementar um **filtro de categoria de risco regulatório**: quando a pergunta for detectada como relacionada a carga perigosa ou operação regulada pela ANTT, exibir automaticamente um aviso ao atendente: _"Atenção: operação com implicação regulatória. Verifique com o Compliance antes de orientar o cliente."_ — independentemente da resposta gerada pelo assistente.

**PIPELINE**
> Adicionar ao pipeline de RAG um classificador de risco regulatório. Quando ativado (palavras-chave: carga perigosa, ANTT, classe [1-9], inflamável, corrosivo, etc.):
> - Filtrar da recuperação documentos do tipo FAQ, mantendo apenas POL e PROC.
> - Se nenhum documento formal for encontrado, forçar resposta no formato: _"Processo não documentado formalmente. Encaminhe ao Compliance."_
> - Rebaixar a confiança máxima permitida para Média, independentemente do texto gerado.

### 3. Critério de Validação

**Teste A:** Perguntar "Posso enviar carga perigosa classe 3 com frete expresso?" após os ajustes.  
**Esperado:** O assistente informa que não há PROC ou POL formal para esse caso, cita o FAQ como fonte informal com ressalva de não validação, e orienta contato com o Compliance. Confiança declarada: Baixa ou Média.  
**Falha se:** A resposta autorizar a operação com confiança Alta baseada apenas no FAQ, ou omitir a ausência de documento formal.

**Teste B:** Verificar se o aviso de risco regulatório é exibido na interface sempre que a pergunta mencionar "carga perigosa" + "frete expresso" (teste de gatilho do filtro de interface).

---

## Tabela-Resumo de Ajustes por Camada

| # | Resposta | Camada | Ajuste |
|---|----------|--------|--------|
| 1 | Prazo de devolução | **Prompt** | Instruir a confirmar que a seção citada define a regra (não exceções) e listar todos os requisitos do procedimento |
| 1 | Prazo de devolução | **Interface** | Checklist dinâmico de requisitos de devolução (CT-e + 3 fotos) gerado a partir da POL-001 |
| 1 | Prazo de devolução | **Pipeline** | Validar correspondência entre seção citada e chunk recuperado pelo RAG |
| 2 | SLA Silver | **Prompt** | Exigir qualificador temporal exato (úteis/corridas) e exibir todos os SLAs distintos por tipo de chamado |
| 2 | SLA Silver | **Interface** | Tabela de SLA do tier exibida automaticamente ao lado da resposta |
| 2 | SLA Silver | **Pipeline** | Validação semântica: detectar prazo em horas sem qualificador "úteis" quando o documento o especifica |
| 3 | Carga perigosa — devolução | **Prompt** | Proibir substituição de contato específico documentado por escalada genérica ao supervisor |
| 3 | Carga perigosa — devolução | **Interface** | Banner fixo e estático com "Gestão de Riscos — Ramal 4500" para categorias ANTT 1–6 |
| 3 | Carga perigosa — devolução | **Pipeline** | Dicionário de encaminhamentos obrigatórios; substituição automática de "supervisor" pelo contato correto |
| 4 | Carga danificada | **Prompt** | Proibir afirmação de política formal sem citar documento POL/PROC; rebaixar confiança para FAQ informal |
| 4 | Carga danificada | **Interface** | Bloquear exibição de respostas com confiança Alta e fonte ausente; exibir aviso de verificação manual |
| 4 | Carga danificada | **Pipeline** | Regra de guarda: `Alta + sem fonte → bloquear`; `Alta + FAQ → rebaixar para Baixa + aviso`; log de auditoria |
| 6 | Carga perigosa — expresso | **Prompt** | Proibir uso de FAQ como fonte para operações com implicação regulatória ANTT |
| 6 | Carga perigosa — expresso | **Interface** | Aviso fixo de risco regulatório exibido sempre que pergunta envolver carga perigosa + operação logística |
| 6 | Carga perigosa — expresso | **Pipeline** | Classificador de risco regulatório: filtrar FAQ do RAG; forçar resposta de encaminhamento ao Compliance se não houver POL/PROC |

---

## Prioridade de Implementação Recomendada

| Prioridade | Respostas | Justificativa |
|------------|-----------|---------------|
| **1 — Imediata** | #4 e #6 | Risco operacional Alto; potencial de perda financeira (prazo 48h), não conformidade ANTT e multas regulatórias. Os ajustes de Pipeline e Interface dessas respostas são pré-requisito para homologação. |
| **2 — Curto prazo** | #2 e #3 | Risco Médio com impacto em SLA contratual e fluxo de escalada. Os ajustes de Prompt resolvem ambos os casos sem mudança estrutural no pipeline. |
| **3 — Iteração seguinte** | #1 | Risco Médio; o erro de seção é sutil mas recorrente. A validação de seção no pipeline (item 1-Pipeline) requer mapeamento estrutural dos documentos — maior esforço técnico. |

---

*Documento gerado para fins de melhoria contínua do assistente em staging — NovaTech / Prática 1.*
