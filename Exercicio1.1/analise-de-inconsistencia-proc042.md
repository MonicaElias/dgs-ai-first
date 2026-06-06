# Análise de Inconsistências — PROC-042 v1 e PROC-042-v2

**Documentos analisados:**
- PROC-042-frete-especial-v1.md (Versão 1.0 — emissão: 03/03/2023)
- PROC-042-v2-frete-especial-revisado.md (Versão 2.0 — emissão: 10/11/2023)

**Elaborado por:** Análise automatizada de inconsistências
**Data de análise:** 06/06/2026

---

## Resumo Executivo

A análise comparativa entre os documentos PROC-042 v1 e PROC-042-v2 identificou **6 inconsistências**, distribuídas entre divergências de valores numéricos, regras contraditórias e ausência de hierarquia formal entre os documentos. Ambos os documentos coexistem sem que nenhum declare formalmente a substituição do outro, criando ambiguidade operacional relevante. As inconsistências afetam diretamente o cálculo do valor do frete, o prazo de entrega prometido ao cliente e a política de concessão de descontos, com potencial impacto financeiro e contratual. A situação exige definição imediata de qual versão prevalece e comunicação formal às equipes operacionais e comerciais.

---

## Divergências Encontradas na Versão 1 e Versão 2

### PROC-042 v1 — Pontos relevantes

| Tema | Conteúdo |
|------|----------|
| Fator de peso (faixa média) | 1.2 para cargas de 1.001kg a 3.000kg |
| Fator de peso (faixa alta) | 1.5 para cargas acima de 3.000kg |
| Multiplicadores regionais | Sul 1.2 / Sudeste 1.0 / Centro-Oeste 1.3 / Nordeste 1.4 / Norte 1.6 |
| Prazo adicional de entrega | +2 dias úteis |
| Desconto de volume | >10 fretes/mês, negociado pelo Comercial, registrado em aditivo contratual |
| Status da PROC-043 | Referenciada sem ressalvas |
| Disposições transitórias | Não previstas |
| Hierarquia documental | Não declara relação com v2 |

### PROC-042-v2 — Pontos relevantes

| Tema | Conteúdo |
|------|----------|
| Fator de peso (faixa média) | 1.15 para cargas de 1.001kg a 3.000kg |
| Fator de peso (faixa alta) | 1.4 para cargas acima de 3.000kg |
| Multiplicadores regionais | Sul 1.3 / Sudeste 1.1 / Centro-Oeste 1.4 / Nordeste 1.5 / Norte 1.8 |
| Prazo adicional de entrega | +3 dias úteis |
| Desconto de volume | ≥8 fretes/mês → 5%; >15 fretes/mês → 10%; maiores com aprovação da Diretoria |
| Status da PROC-043 | Referenciada com alerta: "em processo de revisão pelo Compliance" |
| Disposições transitórias | Previstas: chamados anteriores a 01/12/2023 usam multiplicadores da v1 |
| Hierarquia documental | Não declara formalmente que substitui a v1 |

---

## Análise de Inconsistências

### INC-001 — Valores divergentes nos fatores de peso

**Tipo:** Regras com valores divergentes

Os fatores de peso para as faixas intermediária e superior são distintos entre as versões, sem que nenhum documento explique a descontinuação dos valores anteriores.

- **v1, seção 2:** *"Fator de peso = 1.0 para cargas de 500kg a 1.000kg; **1.2** para cargas de 1.001kg a 3.000kg; **1.5** para cargas acima de 3.000kg."*
- **v2, seção 2:** *"Fator de peso = 1.0 para cargas de 500kg a 1.000kg; **1.15** para cargas de 1.001kg a 3.000kg; **1.4** para cargas acima de 3.000kg."*

Aplicar a v1 em vez da v2 sobre uma carga de 2.000kg gera multiplicação 4,3% maior. Para cargas acima de 3.000kg, a diferença sobe para 7,1%. Sem definição de qual versão prevalece, o operador pode aplicar qualquer um dos dois conjuntos de valores.

---

### INC-002 — Multiplicadores regionais com valores divergentes em todas as regiões

**Tipo:** Regras com valores divergentes

Todos os cinco multiplicadores regionais são diferentes entre as versões, impactando diretamente o valor final do frete calculado.

- **v1, seção 2.1:** Sul 1.2 / Sudeste 1.0 / Centro-Oeste 1.3 / Nordeste 1.4 / Norte 1.6
- **v2, seção 2.1:** *"atualizados em novembro/2023"* — Sul 1.3 / Sudeste 1.1 / Centro-Oeste 1.4 / Nordeste 1.5 / Norte 1.8

A v2 reconhece a atualização (*"Os multiplicadores foram revisados para refletir os custos operacionais atualizados"*), porém não invalida formalmente a v1, que permanece vigente sem marcação de obsolescência. A região Norte apresenta a maior variação: 1.6 (v1) contra 1.8 (v2), diferença de 12,5%.

---

### INC-003 — Prazo de entrega adicional divergente

**Tipo:** Regras com valores divergentes

O número de dias úteis adicionais ao prazo padrão da rota é diferente entre as versões.

- **v1, seção 3:** *"prazo padrão da rota + **2 dias úteis** adicionais para manuseio de carga pesada."*
- **v2, seção 3:** *"prazo padrão da rota + **3 dias úteis** adicionais para manuseio e roteirização de carga pesada (anteriormente era + 2 dias na versão anterior)."*

A v2 reconhece explicitamente a mudança em relação à v1. Entretanto, sem revogação formal da v1, equipes que seguem a v1 podem comprometer junto ao cliente um prazo 1 dia útil menor do que o operacionalmente praticado pela v2.

---

### INC-004 — Política de desconto de volume contraditória

**Tipo:** Regras contraditórias

As duas versões definem critérios incompatíveis de elegibilidade, percentuais e processo de aprovação para descontos de volume, sem que haja referência cruzada entre elas.

- **v1, seção 4:** *"Descontos de volume (mais de **10 fretes especiais/mês** para o mesmo cliente) devem ser **negociados pelo Comercial** e registrados em aditivo contratual."* — Não define percentual.
- **v2, seção 4:** *"a partir de **8 fretes especiais/mês** → desconto de **5%** sobre o multiplicador regional. Acima de **15 fretes/mês** → desconto de **10%**. Descontos maiores requerem aprovação da Diretoria Comercial."*

Os critérios são contraditórios em três dimensões: (a) gatilho de elegibilidade (10 vs. 8 fretes/mês), (b) forma de cálculo (percentual fixo vs. negociação caso a caso) e (c) instância de aprovação (Comercial vs. Diretoria Comercial). Um cliente com 9 fretes/mês, por exemplo, seria elegível pela v2 mas não pela v1.

---

### INC-005 — Ausência de hierarquia formal entre os documentos

**Tipo:** Regras duplicadas / ambiguidade de vigência

Ambos os documentos coexistem sem que nenhum declare formalmente a substituição do outro, criando duplicidade normativa sem hierarquia resolvida.

- **v1, metadados — Status:** *"Este documento não possui indicação formal de vigência ou obsolescência no sistema da NovaTech. Coexiste com a versão PROC-042-v2."*
- **v2, metadados — Status:** *"Este documento não possui indicação formal de que substitui o PROC-042 v1. Ambos coexistem no SharePoint sem hierarquia clara."*

A coexistência de dois documentos com fórmulas e valores diferentes, sem hierarquia formal, permite que diferentes operadores apliquem versões distintas para o mesmo tipo de operação, gerando inconsistência nos valores cobrados.

---

### INC-006 — Estado da PROC-043 referenciada de forma divergente

**Tipo:** Termos iguais com significados diferentes

Ambas as versões referenciam a PROC-043 como documento aplicável a cargas perigosas, porém com status operacional distinto.

- **v1, seção 4:** *"Cargas perigosas com peso acima de 500kg seguem tabela específica (PROC-043: Frete de Cargas Perigosas)."* — Referenciada sem ressalvas.
- **v2, seção 4:** *"Cargas perigosas com peso acima de 500kg seguem tabela específica (PROC-043: Frete de Cargas Perigosas). Nota: a PROC-043 **está em processo de revisão pelo Compliance e pode sofrer alterações**."*

Quem segue apenas a v1 desconhece que a PROC-043 está sob revisão. Decisões operacionais baseadas em uma versão da PROC-043 que pode ser alterada representam risco de não conformidade.

---

## Recomendações

1. **Definir e publicar formalmente a versão vigente:** A Diretoria Comercial deve emitir comunicado oficial declarando que a PROC-042-v2 substitui a PROC-042-v1, com data de vigência clara, e marcar a v1 como obsoleta no SharePoint.

2. **Atualizar o status documental:** Incluir nos metadados de ambos os documentos o campo `Substituído por` (v1) e `Substitui` (v2), evitando nova coexistência ambígua.

3. **Revisar e comunicar a política de descontos:** A inconsistência do INC-004 é a mais crítica do ponto de vista comercial. A Diretoria Comercial deve definir os critérios únicos de elegibilidade, percentuais e alçadas de aprovação, e notificar formalmente o time de atendimento e o Comercial.

4. **Monitorar a revisão da PROC-043:** Incluir a atualização da PROC-043 como dependência no planejamento operacional, comunicando às equipes que o documento referenciado está sujeito a alterações (conforme alerta da v2).

5. **Aplicar as disposições transitórias da v2 como regra oficial:** A seção 5 da v2 define critério de corte em 01/12/2023 para uso dos multiplicadores. Esta regra deve ser formalmente reconhecida e comunicada, pois só é visível para quem leu a v2.

6. **Auditar fretes calculados no período de coexistência:** Verificar operações realizadas entre 10/11/2023 (emissão da v2) e a data de definição formal da versão vigente, identificando possíveis cobranças inconsistentes decorrentes da aplicação de versões distintas.

---

## Matriz Compilada das Inconsistências

| ID | Tipo | Tema | Versão do Documento | Análise de Inconsistência | Recomendações | Fator de Risco |
|----|------|------|---------------------|--------------------------|---------------|----------------|
| INC-001 | Valores divergentes | Fatores de peso (faixas média e alta) | v1: fator 1.2 e 1.5 / v2: fator 1.15 e 1.4 | Diferença de até 7,1% no valor calculado para cargas acima de 3.000kg. Sem hierarquia definida, qualquer versão pode ser aplicada. | Definir oficialmente qual conjunto de fatores prevalece e revogar a v1. | Alto |
| INC-002 | Valores divergentes | Multiplicadores regionais (todas as regiões) | v1: Sul 1.2, SE 1.0, CO 1.3, NE 1.4, N 1.6 / v2: Sul 1.3, SE 1.1, CO 1.4, NE 1.5, N 1.8 | Todas as regiões têm multiplicadores distintos. Norte apresenta variação de 12,5%. Impacto direto no valor do frete cobrado ao cliente. | Publicar formalmente a v2 como vigente e revogar a v1. | Alto |
| INC-003 | Valores divergentes | Prazo de entrega adicional | v1: +2 dias úteis / v2: +3 dias úteis | Promessa de prazo ao cliente pode diferir em 1 dia útil dependendo da versão usada, gerando risco de SLA e insatisfação. | Comunicar formalmente o prazo vigente (+3 dias úteis) às equipes e ao cliente. | Médio |
| INC-004 | Regras contraditórias | Política de desconto de volume | v1: >10 fretes/mês, negociado pelo Comercial / v2: ≥8 fretes→5%; >15→10%; maiores com aprovação da Diretoria | Gatilho, percentual e alçada de aprovação são incompatíveis. Clientes com 8 a 10 fretes/mês têm elegibilidade indefinida. | A Diretoria Comercial deve publicar política única de descontos com critérios e percentuais definidos. | Alto |
| INC-005 | Duplicidade / vigência ambígua | Hierarquia formal entre documentos | v1 e v2 coexistem sem que nenhum revogue o outro formalmente | Operadores podem aplicar versões distintas para a mesma operação, gerando inconsistência de cobrança e risco jurídico. | Emitir comunicado oficial de revogação da v1 e marcar o documento como obsoleto no SharePoint. | Crítico |
| INC-006 | Termos iguais com significados diferentes | Status da PROC-043 referenciada | v1: referenciada sem ressalvas / v2: alerta de revisão pelo Compliance | Equipes que seguem apenas a v1 desconhecem o risco de mudança na PROC-043, podendo operar com base em tabela desatualizada. | Monitorar revisão da PROC-043 e comunicar alerta às equipes operacionais. | Médio |
