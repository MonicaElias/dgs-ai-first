# Comparação de Avaliações — Assistente IA NovaTech (Staging)

**Avaliadores:** Monica Elias (Product Specialist) · Claude Sonnet 4.6 (segundo avaliador)  
**Data:** 27/06/2026  
**Fonte de verdade:** Anexo A — Documentação Simulada NovaTech  

---

## 1. Tabela de Concordâncias e Divergências

| # | Pergunta | Veredicto Monica | Veredicto Claude | Resultado |
|---|----------|-----------------|-----------------|-----------|
| 1 | Prazo de devolução para produtos standard? | Parcialmente correta | Parcialmente correta | **Concordância total** |
| 2 | Prazo de resolução para cliente Silver? | **Correta** | **Parcialmente correta** | **Divergência** |
| 3 | Posso devolver carga perigosa classe 3? | **Correta** | **Parcialmente correta** | **Divergência** |
| 4 | Política para carga danificada durante transporte? | Incorreta | Incorreta | **Concordância parcial** |
| 5 | Qual o SLA do cliente Enterprise? | Correta | Correta | **Concordância total** |
| 6 | Posso enviar carga perigosa com frete expresso? | Parcialmente correta | Parcialmente correta | **Concordância parcial** |

---

## 2. Análise das Divergências e Concordâncias Parciais

### Resposta #2 — Prazo de resolução para cliente Silver · DIVERGÊNCIA

**Monica:** Classificou como **Correta**. O prazo de 48h consta no SLA-2024 e a resposta do assistente está factualmente alinhada com o documento.

**Claude:** Classificou como **Parcialmente correta**, identificando dois problemas:
1. A resposta omite o qualificador **"úteis"** (horas úteis vs. horas corridas), o que altera o compromisso contratual real.
2. Não distingue chamados gerais (48h úteis) de **incidentes críticos** (8h úteis), que possuem SLA completamente diferente.

**Avaliação mais fundamentada:** A análise do Claude parece mais robusta. A omissão do qualificador "úteis" não é um detalhe cosmético — em um contexto de SLA contratual, 48 horas corridas e 48 horas úteis representam compromissos diferentes. Mais grave, a ausência da distinção para incidentes críticos (SLA de 8h) pode levar atendentes a não priorizar chamados urgentes de clientes Silver, gerando descumprimento contratual silencioso. Monica provavelmente avaliou o número (48h) como correto e considerou a resposta satisfatória, sem checar os qualificadores e exceções documentados.

---

### Resposta #3 — Devolução de carga perigosa classe 3 · DIVERGÊNCIA

**Monica:** Classificou como **Correta**. O conteúdo principal estava alinhado com a POL-001, seção 3.2: cargas perigosas classes 1–6 não são elegíveis para devolução padrão, e a recomendação de escalar ao supervisor foi aceita como adequada.

**Claude:** Classificou como **Parcialmente correta**, apontando que o encaminhamento é incorreto: a política determina contato com o setor de **Gestão de Riscos (ramal 4500)**, não escalada genérica para "supervisor".

**Avaliação mais fundamentada:** A análise do Claude é mais precisa. A escalada para "supervisor" é um procedimento vago que depende do supervisor conhecer o fluxo correto — um passo extra sujeito a falha. A política já define o contato específico com ramal, o que indica que a NovaTech optou deliberadamente por centralizar esse tipo de caso em Gestão de Riscos. Monica aceitou a escalada como válida na prática, mas o desvio do fluxo documentado é um erro real de procedimento. Este é um caso em que a familiaridade com o cotidiano operacional pode ter levado Monica a aceitar uma aproximação que a política não endossa.

---

### Resposta #4 — Política para carga danificada durante transporte · CONCORDÂNCIA PARCIAL

**Monica:** Classificou como **Incorreta**, com justificativa sucinta: "Não existe política. Deve ser feita uma investigação e processo jurídico." Identificou corretamente o problema central — o assistente fabricou uma política formal inexistente.

**Claude:** Também classificou como **Incorreta**, mas foi consideravelmente mais detalhado. Além de identificar a ausência de documento formal (POL ou PROC), apontou os elementos críticos omitidos que constam no FAQ Item 38: prazo de **48h para registrar a ocorrência**, encaminhamento obrigatório ao **Jurídico via sinistros@novatech.com.br**, e que o laudo técnico é opcional ("se possível"). Também destacou o padrão de confiança Alta sem nenhuma fonte citada como o risco mais grave.

**O que cada avaliador viu:** Ambas chegaram ao mesmo veredicto e ao mesmo diagnóstico central (política inexistente), mas Monica parou na identificação do problema, enquanto Claude mapeou o delta entre o que foi respondido e o que o único documento existente (FAQ Item 38) realmente diz. A diferença é especialmente relevante no prazo de 48h: um atendente orientado pela resposta do assistente pode deixar o cliente perder o direito ao reembolso por desconhecer esse prazo.

---

### Resposta #6 — Carga perigosa com frete expresso · CONCORDÂNCIA PARCIAL

**Monica:** Classificou como **Parcialmente correta**, com a justificativa de que o processo precisa ser "inserido na política como uma regra" — ou seja, o problema identificado foi a ausência de formalização.

**Claude:** Também classificou como **Parcialmente correta**, mas por uma razão mais técnica: a fonte usada pelo assistente (FAQ-Atendimento, item 32) é um **documento informal, explicitamente não validado pelo Compliance**. Isso significa que orientar um cliente sobre envio de carga perigosa — uma operação sujeita à regulação ANTT — com base em um FAQ não homologado cria risco regulatório independentemente do conteúdo estar ou não correto. Adicionalmente, o FAQ menciona que na prática o processo leva ~2 dias para obter autorização, informação operacionalmente relevante que foi omitida.

**O que cada avaliador viu:** Monica identificou uma lacuna normativa (ausência de regra formal); Claude identificou um risco de conformidade imediato (FAQ não validado sendo usado como base para orientação sobre carga perigosa). As perspectivas são complementares, mas a de Claude implica risco mais urgente: não é apenas que a regra precisa ser formalizada — é que usar uma fonte não validada para esse tipo de operação já é um problema em si.

---

## 3. Síntese Final

### Padrões nas concordâncias

Nos dois casos de concordância total (#1 e #5), ambas as avaliações identificaram corretamente: um erro de citação de fonte na resposta #1 (seção 3.2 no lugar de 3.1), e um acerto legítimo na resposta #5, onde o assistente admitiu corretamente o desconhecimento sobre um tier inexistente. Esses casos representam os extremos mais claros do conjunto — erro evidente e acerto inequívoco — o que explica a convergência independente de profundidade analítica.

Nas concordâncias parciais (#4 e #6), ambas chegaram ao mesmo veredicto, mas com granularidade diferente: Monica identificou o problema central em cada caso, Claude mapeou os detalhes adicionais e os riscos derivados. A divergência não é de diagnóstico, mas de profundidade.

### O que as divergências revelam sobre os limites do assistente como avaliador

As divergências nas respostas #2 e #3 revelam uma característica importante do Claude como avaliador: **ele aplica verificação exaustiva contra a documentação literal**, sem concessão ao que seria "razoável na prática". Isso é uma vantagem em termos de precisão normativa, mas pode gerar avaliações mais severas do que o contexto operacional exige.

Dito isso, nos dois casos de divergência, a análise do Claude parece mais fundamentada do ponto de vista dos riscos documentados. O que Monica aceitou como correto (#2 e #3) é parcialmente correto na prática cotidiana, mas insuficiente diante do que a documentação formal especifica. Isso não indica erro de Monica — indica que sua avaliação priorizou o resultado prático, enquanto o Claude priorizou a aderência estrita ao documento.

### Reflexão sobre o valor do humano no loop

Esta comparação evidencia dois modos complementares de avaliação:

**O avaliador humano (Monica)** traz contexto operacional: sabe o que funciona na prática, reconhece quando uma resposta é "boa o suficiente" e evita falsos positivos gerados por excesso de literalismo. Sua avaliação da resposta #2, por exemplo, reflete o fato de que "48h" sem o qualificador "úteis" provavelmente não causaria problema no atendimento do dia a dia.

**O avaliador IA (Claude)** traz cobertura sistemática: lê a documentação em detalhe, identifica omissões específicas, mapeia consequências de segunda ordem e não se deixa influenciar pela plausibilidade superficial de uma resposta. Suas análises mais detalhadas nas respostas #4 e #6 — especialmente o prazo de 48h e o risco de fonte não validada — são contribuições que o avaliador humano não capturou de forma completa.

O maior valor da combinação está exatamente nos casos de risco alto: quando Monica e Claude concordam que algo é incorreto ou perigoso, como nas respostas #4 e #6, a convergência independente aumenta a confiança no diagnóstico. Quando divergem, como em #2 e #3, o ponto de tensão entre "funciona na prática" e "não está em conformidade com a política" é precisamente o que a gestão precisa decidir — e nenhum dos dois avaliadores, sozinho, tornaria essa tensão visível.

Isso confirma que a avaliação mais robusta de sistemas de IA em produção exige as duas perspectivas: o humano para calibrar relevância operacional, a IA para garantir cobertura documental. Nenhum substitui o outro.

---

*Documento gerado para fins de avaliação interna do assistente em staging — NovaTech / Prática 1.*
