# Cruzamento de Inconsistências vs. FAQ — Análise Integrada

**Fontes utilizadas:**
- `entrega-final.md` — Mapa de temas, conflitos e hipóteses de gaps
- `analise-de-inconsistencia-proc042.md` — Análise comparativa PROC-042 v1 × v2
- `FAQ-atendimento.md` — Documento colaborativo informal do time de atendimento

**Data de análise:** 06/06/2026

---

## Resumo Executivo

A análise cruzada entre o FAQ de atendimento, o mapa de gaps (entrega-final.md) e a análise de inconsistências da PROC-042 revela que o documento informal orienta os atendentes com base em práticas que, em pelo menos quatro pontos, contradizem ou carecem de respaldo nos documentos normativos vigentes. O caso mais crítico é a orientação do FAQ sobre devolução de cargas perigosas: o documento instrui o atendente a não dizer que é "impossível" e a sugerir tratamento especial, enquanto a POL-001 classifica explicitamente essas cargas como inelegíveis pelo processo padrão. O segundo conflito mais grave envolve a política de descontos: o FAQ combina o gatilho da v1 (>10 fretes/mês) com a lógica da v2 (desconto automático), criando uma orientação híbrida que não corresponde a nenhuma das duas versões formais. Há ainda quatro temas — seguro de carga, carga danificada em trânsito, frete expresso para cargas perigosas e SLA de tracking por rota — para os quais o FAQ fornece orientações práticas detalhadas sem qualquer contrapartida em documento normativo. Por fim, o FAQ orienta o uso da PROC-042-v2 como padrão "na dúvida", mas não tem autoridade formal para isso, pois nenhum dos dois documentos declara hierarquia sobre o outro. O conjunto dessas divergências cria um cenário de risco operacional, comercial e jurídico, onde o atendente age de boa-fé com base em orientações informais que podem não ter validade contratual ou regulatória.

---

## Lista de Divergências Analisadas e Contextualizadas

### DIV-001 — Devolução de carga perigosa: FAQ contradiz POL-001

**Documentos envolvidos:** POL-001-politica-devolucao.md × FAQ-atendimento.md (Item 3)

A POL-001 é explícita ao excluir cargas perigosas classificadas nas classes 1 a 6 da ANTT do processo padrão de devolução: *"Cargas perigosas classificadas nas classes 1 a 6 da ANTT [...] NÃO são elegíveis para devolução pelo processo padrão."* O procedimento definido é: *"o cliente deve entrar em contato com o setor de Gestão de Riscos (ramal 4500) para tratamento individual."*

O FAQ, por sua vez, instrui o atendente: *"Oficialmente não pode pelo processo padrão, mas já tiveram casos em que o pessoal de Riscos autorizou exceção. Então não diga que é impossível — diga que precisa de tratamento especial."*

O FAQ reconhece a proibição normativa, mas orienta o atendente a suavizar a negativa e insinuar que exceções são viáveis, algo que a POL-001 não autoriza nem menciona. O risco é que o cliente interprete a fala do atendente como uma promessa informal de devolução, criando passivo contratual e expectativa não respaldada por documento oficial.

---

### DIV-002 — Desconto de volume: FAQ cria orientação híbrida inexistente nos documentos formais

**Documentos envolvidos:** PROC-042-v1 × PROC-042-v2 × FAQ-atendimento.md (Item 45)

A PROC-042 v1 define: *"Descontos de volume (mais de 10 fretes especiais/mês para o mesmo cliente) devem ser negociados pelo Comercial e registrados em aditivo contratual."* — sem percentual fixo, sem automatismo.

A PROC-042 v2 define: *"a partir de 8 fretes especiais/mês [...] desconto de 5% sobre o multiplicador regional. Acima de 15 fretes/mês, desconto de 10%."* — com percentual fixo e sem mencionar negociação pelo Comercial abaixo dos limiares.

O FAQ orienta: *"Para clientes com mais de 10 fretes especiais por mês, existe desconto automático na tabela (veja PROC-042)."*

O FAQ adota o gatilho numérico da v1 (>10 fretes/mês) e a lógica de automatismo da v2 (desconto automático), criando uma terceira versão da regra que não existe em nenhum dos dois documentos formais. Isso é confirmado pelo mapeamento da entrega-final.md (Conflito 3 e Gap G3). Um cliente com 9 fretes/mês seria elegível pela v2 (≥8), não elegível pela v1 (>10) e estaria em zona cinzenta pelo FAQ.

---

### DIV-003 — Hierarquia entre versões da PROC-042: FAQ orienta sem autoridade formal

**Documentos envolvidos:** PROC-042-v1 × PROC-042-v2 × FAQ-atendimento.md (Item 8)

Ambas as versões da PROC-042 declaram explicitamente que não há hierarquia formal entre elas. A v1 registra: *"Coexiste com a versão PROC-042-v2."* A v2 registra: *"Ambos coexistem no SharePoint sem hierarquia clara."*

O FAQ instrui o atendente: *"Na dúvida, use a mais recente (v2)."*

O FAQ assume uma posição de arbitragem documental que nenhum documento normativo lhe confere. A orientação pode estar correta na prática, mas não tem respaldo formal. A entrega-final.md classifica isso como inconsistência crítica (INC-005) e gap prioritário (G3): *"Qual versão está vigente oficialmente? Existe despacho ou comunicado formalizando a transição?"*

---

### DIV-004 — Seguro de carga: FAQ define percentuais sem respaldo normativo

**Documentos envolvidos:** FAQ-atendimento.md (Item 22) — sem contrapartida em nenhum documento formal disponível

O FAQ fornece orientação detalhada: *"O valor é 0,3% do valor declarado da mercadoria para cargas padrão e 0,8% para cargas perigosas. Detalhe: isso vale para contratos a partir de 2023. Contratos mais antigos podem ter percentuais diferentes — confirme com o Comercial."*

Nenhum dos documentos normativos disponíveis (POL-001, PROC-042 v1 e v2, SLA-2024) menciona seguro de carga, percentuais ou condições contratuais relacionadas. A entrega-final.md classifica isso como Gap G2: *"Atendentes podem informar valores desatualizados ou incorretos; risco comercial e jurídico."* Os percentuais informados pelo atendente com base no FAQ não têm respaldo formal verificável.

---

### DIV-005 — Carga danificada em trânsito: FAQ define processo sem documento normativo

**Documentos envolvidos:** FAQ-atendimento.md (Item 38) — sem contrapartida em nenhum documento formal disponível

O FAQ define procedimento completo: *"O cliente precisa registrar a ocorrência em até 48h após o recebimento, com fotos e laudo se possível. [...] encaminhe para o e-mail sinistros@novatech.com.br."*

Nenhum documento normativo disponível define prazo, documentação exigida, canal de contato ou processo de reembolso para cargas danificadas em trânsito. A entrega-final.md classifica como Gap G4: *"Atendentes seguem procedimento informal não validado; risco de inconsistência e passivo jurídico."* O prazo de 48h e o e-mail sinistros@ são orientações sem validação formal, podendo não ter validade contratual.

---

### DIV-006 — SLA de tracking por rota: FAQ define critérios sem respaldo no SLA-2024

**Documentos envolvidos:** SLA-2024-tabela-sla-clientes.md × FAQ-atendimento.md (Item 27)

O FAQ define parâmetros práticos por região: *"Rotas para o Norte podem levar até 10 dias úteis. Para Sul/Sudeste, mais de 3 dias parado é estranho."* E orienta escalonamento: *"Abra um chamado de rastreamento e classifique como prioridade alta se for Gold ou se o valor da carga for acima de R$ 50.000."*

O SLA-2024 não cobre prazos de tracking por rota. A entrega-final.md classifica como Gap G5: *"Atendentes usam critério informal divergente; clientes Gold podem exigir escalada por SLA não documentado."* Os critérios do FAQ podem gerar expectativa de SLA de tracking que não está contratualmente definida.

---

### DIV-007 — Frete expresso para carga perigosa: FAQ descreve fluxo sem documento normativo

**Documentos envolvidos:** FAQ-atendimento.md (Item 32) — sem contrapartida em nenhum documento formal disponível

O FAQ orienta: *"Sim, mas precisa de autorização do Compliance e a documentação ANTT tem que estar atualizada. Na prática, demora uns 2 dias para conseguir a autorização."*

Nenhum documento normativo disponível define o processo de autorização de frete expresso para cargas perigosas, os critérios do Compliance, a documentação ANTT exigida ou os prazos de aprovação. A entrega-final.md classifica como Gap G8: *"Atendente pode subestimar o prazo ou omitir a necessidade de documentação ANTT; risco regulatório."*

---

## Tabela Comparativa das Divergências

| ID | Tema | Evidência nos Documentos Formais | Evidência FAQ | Impacto |
|----|------|----------------------------------|---------------|---------|
| DIV-001 | Devolução de carga perigosa | POL-001 (seção 3.2): *"NÃO são elegíveis para devolução pelo processo padrão"* | Item 3: *"não diga que é impossível — diga que precisa de tratamento especial"* | Alto — atendente sugere possibilidade não autorizada formalmente, criando expectativa não respaldada e risco jurídico |
| DIV-002 | Desconto de volume | v1 (seção 4): >10 fretes/mês, negociado pelo Comercial, sem percentual / v2 (seção 4): ≥8 fretes → 5%; >15 → 10%, automático | Item 45: *">10 fretes especiais por mês, existe desconto automático na tabela"* | Alto — orientação híbrida sem base formal; clientes com 8–10 fretes/mês ficam em zona cinzenta |
| DIV-003 | Versão vigente da PROC-042 | v1 e v2: ambas declaram coexistência sem hierarquia formal (metadados de ambos os documentos) | Item 8: *"Na dúvida, use a mais recente (v2)"* | Médio — FAQ arbitra sem autoridade; pode gerar cobrança com versão não contratualmente acordada com o cliente |
| DIV-004 | Seguro de carga — percentuais | Nenhum documento normativo disponível trata do tema | Item 22: *"0,3% [...] para cargas padrão e 0,8% para cargas perigosas"* | Alto — valores informados sem respaldo normativo; risco de informação desatualizada ou incorreta ao cliente |
| DIV-005 | Carga danificada em trânsito | Nenhum documento normativo disponível define procedimento | Item 38: *"registrar a ocorrência em até 48h [...] fotos e laudo [...] sinistros@novatech.com.br"* | Alto — processo informal aplicado a casos com implicação jurídica e financeira, sem validação formal |
| DIV-006 | SLA de tracking por rota | SLA-2024: não cobre prazos de tracking por rota ou região | Item 27: *"Norte podem levar até 10 dias úteis. Sul/Sudeste, mais de 3 dias parado é estranho"* | Médio — critérios de escalonamento sem contrato; cliente Gold pode contestar SLA de rastreamento |
| DIV-007 | Frete expresso — carga perigosa | Nenhum documento normativo disponível cobre o tema | Item 32: *"precisa de autorização do Compliance e documentação ANTT [...] demora uns 2 dias"* | Médio — risco regulatório se documentação ANTT não for devidamente exigida; prazo estimado sem formalização |

---

## Principais Riscos Identificados

**Risco 1 — Jurídico/Contratual (DIV-001 e DIV-005)**
O atendente, ao seguir o FAQ, pode comprometer a NovaTech com expectativas não cobertas pelos documentos formais. No caso de devolução de cargas perigosas, a POL-001 é explícita sobre a inelegibilidade, mas o FAQ sugere ao atendente que contorne a negativa. Em caso de carga danificada, o FAQ define prazos e processos que, por não terem respaldo normativo, podem não ser reconhecidos como compromisso formal da empresa.

**Risco 2 — Financeiro/Comercial (DIV-002 e DIV-004)**
A orientação híbrida do FAQ sobre descontos pode levar a concessão de benefícios não previstos em contrato (aplicação automática com critério errado) ou à negação de descontos a clientes que, pela v2 vigente, já seriam elegíveis (8–10 fretes/mês). Os percentuais de seguro informados sem respaldo normativo podem gerar conflito com apólices reais ou contratos anteriores a 2023.

**Risco 3 — Operacional/Reputacional (DIV-003 e DIV-006)**
A orientação do FAQ de usar a v2 "na dúvida" sem formalização pode resultar em cobranças divergentes para clientes cujos contratos foram firmados com base nos multiplicadores da v1. O uso de critérios informais de SLA de tracking pode gerar escalada indevida (custo operacional) ou não escalada quando devida (risco de SLA para clientes Gold com carga de alto valor).

**Risco 4 — Regulatório (DIV-007)**
O frete expresso para cargas perigosas envolve obrigações perante a ANTT. O FAQ oferece estimativa de prazo e processo sem base em documento normativo — se a documentação ANTT não for exigida corretamente, a NovaTech pode incorrer em irregularidade regulatória.

---

## Dúvidas a Serem Levantadas com o Cliente

| # | Dúvida | Contexto |
|---|--------|----------|
| 1 | Existe algum processo formal documentado que autorize exceções à POL-001 para devolução de cargas perigosas? Quem pode autorizar e em quais condições? | DIV-001 — FAQ sugere que Gestão de Riscos pode autorizar, mas POL-001 não prevê essa exceção. |
| 2 | Qual versão da PROC-042 está oficialmente vigente? Existe comunicado, despacho ou e-mail formal declarando a v2 como substituta da v1? A regra de corte de 01/12/2023 (seção 5 da v2) foi comunicada ao time e aos clientes afetados? | DIV-003 e INC-005 — coexistência sem hierarquia formal. |
| 3 | Qual é a política formal de descontos de volume em vigor hoje? Os percentuais e gatilhos da v2 são os oficialmente praticados? O processo é automático no sistema ou ainda passa pelo Comercial? | DIV-002 — FAQ, v1 e v2 têm regras incompatíveis. |
| 4 | Existe documento normativo que formalize o seguro de carga adicional — percentuais, coberturas, condições por tipo de contrato? O FAQ pode informar ao cliente valores (0,3% / 0,8%) que não sejam os praticados na apólice atual? | DIV-004 — percentuais sem respaldo normativo. |
| 5 | Existe política ou procedimento formal para sinistros de carga danificada em trânsito? O prazo de 48h e o e-mail sinistros@novatech.com.br são canais oficiais? Quem no Jurídico ou Operações é responsável por esse fluxo? | DIV-005 — processo informal aplicado a casos com impacto financeiro e jurídico. |
| 6 | O SLA-2024 cobre ou pretende cobrir prazos de tracking por rota e região? Os critérios do FAQ (Norte até 10 dias, Sul/Sudeste >3 dias = anomalia) foram validados por Operações? | DIV-006 — critérios de escalonamento sem respaldo contratual. |
| 7 | Existe procedimento escrito para frete expresso de carga perigosa? Quem no Compliance autoriza? Qual é a documentação ANTT exigida e o prazo real de aprovação? | DIV-007 — fluxo regulatório sem documento formal. |
| 8 | A PROC-043 (Frete de Cargas Perigosas) está formalmente vigente? Quem é o responsável? Quando a revisão mencionada pela v2 será concluída? | Gap G1 da entrega-final.md — documento referenciado por ambas as versões da PROC-042, mas não disponível. |
