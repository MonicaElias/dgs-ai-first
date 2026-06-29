# Avaliação de Qualidade — Assistente IA NovaTech (Staging)

**Avaliador:** Claude Sonnet 4.6  
**Data:** 24/06/2026  
**Fonte de verdade:** Anexo A — Documentação Simulada NovaTech  
**Critério:** Respostas avaliadas de forma independente; confiança declarada pelo sistema **não foi considerada**.

---

## Tabela de Avaliação

| # | Pergunta | Veredicto | Justificativa | Tipo de Erro | Risco Operacional |
|---|----------|-----------|---------------|--------------|-------------------|
| 1 | Prazo de devolução para produtos standard? | **PARCIALMENTE CORRETA** | O prazo de 7 dias úteis está correto (POL-001, **seção 3.1**). Porém a fonte citada é **errada**: a seção 3.2 trata das *exceções* ao prazo, não do prazo em si. Além disso, o procedimento está incompleto: exige também o número do CT-e e mínimo de 3 fotos específicas (embalagem, etiqueta e conteúdo), não apenas "fotos". | Fonte incorreta + Informação incompleta | **Médio** — atendente pode aceitar chamado sem CT-e ou com fotos insuficientes, gerando retrabalho e atrasos na devolução. |
| 2 | Prazo de resolução para cliente Silver? | **PARCIALMENTE CORRETA** | O prazo de 48h consta no SLA-2024, mas a resposta omite o qualificador **"úteis"** (horas úteis), o que muda significativamente o compromisso contratual. Também não distingue chamados gerais (48h úteis) de **incidentes críticos** (8h), que têm SLA completamente diferente. | Informação incompleta | **Médio** — em incidentes críticos, o SLA real é 8h, não 48h. Um atendente pode deixar de priorizar corretamente um chamado crítico de cliente Silver. |
| 3 | Posso devolver carga perigosa classe 3? | **PARCIALMENTE CORRETA** | Está correto que cargas perigosas classes 1–6 da ANTT não são elegíveis para devolução padrão (POL-001, seção 3.2). Porém o encaminhamento indicado está **errado**: a política determina contato com o setor de **Gestão de Riscos (ramal 4500)**, não escalada para "supervisor". | Informação incompleta | **Médio** — escalada para supervisor gera perda de tempo; o contato correto já está definido na política com ramal específico. |
| 4 | Política para carga danificada durante transporte? | **INCORRETA** | **Não existe documento formal (POL ou PROC)** sobre carga danificada em trânsito — o próprio Anexo A identifica isso como gap. A única referência é o FAQ Item 38, que é informal e não validado pelo Compliance. A resposta omite informações críticas presentes no FAQ: prazo de **48h para registrar a ocorrência**, escalada obrigatória para o **Jurídico** via sinistros@novatech.com.br, e que o laudo técnico é opcional ("se possível"). A resposta é apresentada com confiança **Alta** sem citar fonte alguma. | Alucinação (política formal inexistente) + Informação incompleta | **Alto** — atendente pode informar cliente sem mencionar o prazo de 48h (gerando perda do direito ao reembolso) e encaminhar pelo fluxo errado em vez de sinistros@novatech.com.br. |
| 5 | SLA do cliente Enterprise? | **CORRETA** | O assistente identificou corretamente que o tier "Enterprise" não existe na documentação, listou os três tiers reais (Gold, Silver, Standard) e sugeriu verificação ou escalada. O SLA-2024 confirma explicitamente: "Não existem outros tiers além dos três listados acima." A confiança declarada como **Baixa** é apropriada. | — (sem erros) | **Baixo** |
| 6 | Posso enviar carga perigosa com frete expresso? | **PARCIALMENTE CORRETA** | O conteúdo da resposta espelha o FAQ Item 32, que de fato menciona a possibilidade com autorização do Compliance e documentação ANTT. Porém: (1) o FAQ é **documento informal, não validado** por Compliance ou Operações; (2) o Anexo A registra explicitamente que **não existe documento formal (PROC ou POL) que defina esse processo**; (3) a confiança declarada é **Alta** para uma fonte não confiável sobre operação envolvendo carga perigosa; (4) o FAQ menciona que na prática leva ~2 dias para obter autorização — informação relevante omitida. | Fonte não confiável | **Alto** — orientar um cliente sobre envio de carga perigosa com base em FAQ informal, sem respaldo normativo, expõe a NovaTech a risco regulatório (ANTT) e de segurança. |

---

## Síntese e Casos Críticos

Das seis respostas avaliadas, apenas **uma foi considerada Correta** (resposta #5), duas foram **Incorreta ou de alto risco** (#4 e #6), e três apresentaram **erros parciais** de fonte ou incompletude.

**Os casos mais críticos são:**

- **Resposta #4 (carga danificada)** é o maior risco operacional do conjunto. O assistente afirmou com confiança Alta uma política formal que simplesmente não existe na documentação. A omissão do prazo de 48h para registro da ocorrência pode fazer o cliente perder o direito ao reembolso sem que o atendente perceba o erro. A ausência de qualquer fonte citada, combinada com confiança Alta, é o padrão mais perigoso que um assistente de IA pode exibir em ambiente corporativo.

- **Resposta #6 (carga perigosa + frete expresso)** apresenta risco equivalente por razões distintas: a autorização concedida se baseia exclusivamente em um documento informal, explicitamente não validado pelo Compliance, sobre uma operação que envolve carga perigosa e regulação ANTT. Uma orientação equivocada aqui pode gerar não conformidade regulatória, multas e risco de segurança.

- **Resposta #1** merece atenção por citar a seção errada da mesma política — um erro sutil que pode passar despercebido, mas que indica que o modelo está indexando incorretamente os documentos internos.

**Padrão sistêmico identificado:** O assistente tende a inflar a confiança declarada em respostas baseadas em fontes frágeis (FAQ informal) ou em ausência de fontes. A confiança declarada é **inversamente proporcional à qualidade real** nas respostas #4 e #6 — exatamente onde o risco é mais alto. Isso sugere que o mecanismo de calibração de confiança do sistema precisa ser revisado antes de homologação.

---

*Documento gerado para fins de avaliação interna do assistente em staging — NovaTech / Prática 1.*
