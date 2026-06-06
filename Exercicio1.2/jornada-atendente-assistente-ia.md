# Jornada do Atendente com Assistente de IA

**Contexto:** NovaTech — Operação de Atendimento ao Cliente
**Elaborado por:** Discovery inicial — Fase de Intenção e Descoberta
**Data:** 06/06/2026

---

## 1. Resumo do Papel do Atendente Usando a IA

O atendente da NovaTech utiliza o assistente de IA como primeira fonte de consulta durante o atendimento ao cliente. Em vez de abrir manualmente até quatro fontes distintas — SharePoint, Confluence, planilhas de rede e o FAQ informal — o atendente faz uma consulta única em linguagem natural e recebe uma resposta com indicação da fonte documental de origem. Essa mudança reduz o tempo médio de busca de 12 minutos para menos de 2 minutos por chamado.

O assistente opera sobre a base documental oficial da NovaTech (políticas, procedimentos, tabelas de SLA e contratos) e responde com base apenas no que está documentado. Ele não inventa respostas, não arbitra conflitos entre versões de documentos sem sinalizar a ambiguidade, e não substitui decisões que exigem validação humana.

O papel do atendente permanece central: ele interpreta a resposta do assistente, aplica o contexto do cliente e assume a responsabilidade pelo que comunica. O assistente é uma ferramenta de suporte à decisão — não um decisor autônomo.

---

## 2. Fluxo Principal

> **Caminho feliz:** O assistente encontra resposta confiável, com fonte clara e sem conflito entre documentos.

**Passo 1 — Recebimento da dúvida**
O atendente recebe a dúvida do cliente via canal de atendimento (telefone, chat ou e-mail). A dúvida se enquadra em uma das categorias mais comuns: prazo de entrega, regra de frete, política de devolução ou procedimento de reclamação.

**Passo 2 — Consulta ao assistente de IA**
O atendente digita a dúvida no assistente em linguagem natural. Exemplos de consulta:
- *"Qual o prazo para solicitação de devolução de carga padrão?"*
- *"Como calcular o frete especial para carga de 2.000 kg na região Norte?"*
- *"Quais são os SLAs de resposta para clientes Gold com incidente crítico?"*

**Passo 3 — Resposta com fonte**
O assistente retorna a resposta com:
- O conteúdo da informação solicitada
- O documento de origem (ex.: *POL-001, seção 3.1 — versão 3.1, 15/01/2024*)
- A versão e data do documento consultado
- Alerta caso o tema esteja coberto apenas por documento informal (ex.: FAQ não validado)

**Passo 4 — Avaliação pelo atendente**
O atendente lê a resposta e verifica se ela é aplicável ao contexto específico do cliente (tier, tipo de carga, histórico contratual). Se a resposta estiver clara, completa e sem conflito sinalizado, segue para o próximo passo.

**Passo 5 — Uso da resposta no atendimento**
O atendente repassa a informação ao cliente com segurança, citando a base normativa quando relevante (*"De acordo com nossa política de devolução..."*). O chamado é encerrado com registro da consulta realizada.

**Passo 6 — Encerramento do chamado**
O atendente registra o chamado como resolvido. O sistema associa automaticamente a consulta ao assistente ao ticket, criando rastreabilidade para análise futura.

---

## 3. Fluxo de Fallback

> **Quando o assistente não encontra resposta confiável, o atendente discorda da resposta, ou o caso exige validação humana.**

### Situação 3.1 — O assistente não encontra resposta confiável

O assistente sinaliza: *"Não encontrei informação documentada suficiente para responder com segurança. Recomendo escalar para validação."*

Isso ocorre em temas identificados como gaps no discovery, como:
- Procedimento de solicitação de crédito por violação de SLA (Gap G7 — não coberto por nenhum documento)
- Frete expresso para cargas perigosas (Gap G8 — coberto apenas pelo FAQ informal)
- Critérios exatos de classificação de tier (Gap G6 — SLA-2024 não especifica limiares)

**Ação do atendente:**
1. Informar ao cliente que a questão requer verificação interna
2. Abrir ticket de escalonamento para o supervisor ou área responsável
3. Registrar o tema como "sem cobertura documental confirmada" para alimentar o fluxo de feedback

### Situação 3.2 — O assistente apresenta resposta com conflito entre versões

O assistente sinaliza: *"Encontrei informações divergentes entre documentos. Veja as versões disponíveis antes de responder ao cliente."*

Isso ocorre nos conflitos mapeados no discovery, como:
- Multiplicadores regionais da PROC-042 v1 × v2 (INC-002 — Norte: 1,6 vs. 1,8)
- Prazo adicional de entrega: +2 dias úteis (v1) × +3 dias úteis (v2)
- Política de desconto de volume: gatilhos e percentuais incompatíveis entre v1 e v2 (INC-004)

**Ação do atendente:**
1. Não repassar ao cliente nenhum dos valores em conflito sem validação
2. Escalar para o supervisor com o registro do conflito identificado
3. O supervisor consulta a área responsável (Diretoria Comercial ou Operações) para definir qual versão prevalece no caso específico
4. Registrar a decisão no chamado para criar precedente documentado

### Situação 3.3 — O atendente discorda da resposta apresentada

O atendente percebe que a resposta do assistente contradiz a prática operacional que ele conhece, ou que o documento fonte está desatualizado na prática.

**Ação do atendente:**
1. Não usar a resposta no atendimento ao cliente naquele momento
2. Informar ao cliente que verificará a informação
3. Registrar a discordância via fluxo de feedback (ver Seção 4)
4. Escalar para supervisor se o caso exigir resposta imediata

### Situação 3.4 — O caso exige validação humana obrigatória

Alguns temas identificados no discovery exigem, por natureza, decisão humana antes de qualquer resposta ao cliente:

| Situação | Área responsável |
|----------|-----------------|
| Devolução de carga perigosa (classes 1–6 ANTT) | Gestão de Riscos (ramal 4500) — conforme POL-001 |
| Desconto de volume acima dos limiares automáticos | Diretoria Comercial — conforme PROC-042 v2 |
| Solicitação de crédito por violação de SLA | Supervisor + área contratual — Gap G7 sem procedimento formal |
| Carga danificada em trânsito com valor relevante | Equipe de Sinistros — procedimento formal ainda não disponível (Gap G4) |
| Frete expresso para carga perigosa | Compliance + documentação ANTT obrigatória — Gap G8 |

**Ação do atendente:**
1. Identificar que o tema pertence à lista acima
2. Não tentar responder com base no FAQ ou no assistente para esses casos
3. Direcionar imediatamente para a área responsável e registrar o encaminhamento no chamado

---

## 4. Fluxo de Feedback

> **Como o atendente informa que a resposta estava incorreta, desatualizada, incompleta, sem fonte suficiente ou em conflito com a prática operacional.**

### Tipos de feedback e como acionar

| Tipo de feedback | Descrição | Ação do atendente |
|-----------------|-----------|-------------------|
| **Errada** | A informação contradiz o que foi confirmado pela área responsável | Registrar o feedback com o número do chamado, o documento citado pelo assistente e a correção recebida |
| **Desatualizada** | O documento existe, mas os valores ou regras não refletem a prática vigente | Registrar indicando a versão do documento e a informação que está na prática atual |
| **Incompleta** | O assistente respondeu parcialmente, omitindo condicionantes importantes | Registrar o que faltou na resposta e qual seria a informação completa |
| **Sem fonte suficiente** | O assistente respondeu apenas com base no FAQ informal, sem documento normativo | Registrar o tema como coberto apenas por fonte informal — candidato a lacuna documental |
| **Conflito com prática operacional** | A resposta está documentada, mas a operação pratica diferente | Registrar a divergência entre o documento e a prática real, identificando qual área deveria alinhar os dois |

### Como o feedback alimenta melhoria contínua

**Ciclo de melhoria em três etapas:**

**Etapa 1 — Coleta e triagem (contínua)**
Todos os feedbacks registrados pelos atendentes são agregados por tema, documento e tipo de feedback. Feedbacks do mesmo tema em chamados diferentes indicam padrão — não exceção.

**Etapa 2 — Priorização mensal (Operações + Compliance + Comercial)**
A cada ciclo de atualização documental (mensal, conforme o processo atual da NovaTech), os feedbacks acumulados são revisados pelas três áreas responsáveis. Os temas com maior volume de feedbacks "errada" ou "conflito com prática" são priorizados para atualização documental antes de qualquer outra revisão.

**Etapa 3 — Atualização da base e re-indexação pelo assistente**
Quando um documento é atualizado ou um gap é coberto por novo procedimento normativo, a base do assistente é re-indexada. O atendente que gerou o feedback original recebe notificação de que o tema foi tratado.

> **Nota operacional:** Os gaps identificados no discovery (G1 a G8) e as divergências DIV-001 a DIV-007 do cruzamento FAQ × documentos formais devem ser tratados como backlog prioritário de qualidade da base documental antes do go-live do assistente. Responder a partir de uma base com conflitos não resolvidos cria risco de reforçar os mesmos problemas atuais.

---

## 5. Guardrails Obrigatórios do Assistente

> **Restrições específicas ao domínio de logística e atendimento da NovaTech, baseadas nos riscos identificados no discovery.**

---

### Guardrail 1 — Nunca informar valor de frete calculado com versão de documento em conflito

**Regra:** O assistente não pode calcular ou informar valores de frete especial quando identificar que os parâmetros necessários (multiplicadores regionais, fatores de peso, prazo adicional) existem em mais de uma versão sem hierarquia formal declarada.

**Por quê:** A PROC-042 v1 e v2 coexistem com valores divergentes em todas as regiões (ex.: Norte: 1,6 vs. 1,8) e sem revogação formal de nenhuma delas. Informar um valor calculado com qualquer uma das versões, sem deixar isso explícito, pode gerar cobrança divergente, contestação contratual e perda de confiança do cliente.

**Comportamento esperado:** Ao identificar conflito entre versões, o assistente sinaliza o conflito com as duas versões disponíveis e instrui o atendente a escalar para o supervisor antes de informar qualquer valor ao cliente.

---

### Guardrail 2 — Nunca sugerir que devolução de carga perigosa é possível sem aprovação formal de Gestão de Riscos

**Regra:** O assistente não pode orientar o atendente a "deixar em aberto" ou "sugerir tratamento especial" para devoluções de cargas classificadas nas classes 1 a 6 da ANTT. A resposta deve ser diretiva: o processo padrão de devolução não se aplica, e o encaminhamento obrigatório é para Gestão de Riscos (ramal 4500).

**Por quê:** O FAQ informal instrui o atendente a não dizer que é "impossível" e a insinuar que exceções são viáveis (DIV-001). A POL-001, norma vigente, é explícita ao excluir cargas perigosas do processo padrão. O assistente não pode reproduzir a ambiguidade do FAQ — o risco jurídico e contratual de criar expectativa não autorizada é alto.

**Comportamento esperado:** O assistente responde com a regra da POL-001 e o encaminhamento para Gestão de Riscos. Não oferece alternativa que sugira viabilidade de devolução fora desse fluxo.

---

### Guardrail 3 — Nunca informar prazo de entrega sem identificar a versão do documento utilizada

**Regra:** Toda resposta sobre prazo de entrega de frete especial deve indicar explicitamente qual versão da PROC-042 foi usada como base (+2 dias úteis = v1 / +3 dias úteis = v2), enquanto a hierarquia entre as versões não for formalmente resolvida.

**Por quê:** A diferença de 1 dia útil entre as versões (INC-003) pode gerar promessa errada ao cliente. Um atendente que usa a v1 e outro que usa a v2 comunicam prazos diferentes para o mesmo tipo de carga, gerando inconsistência perceptível ao cliente e risco de SLA.

**Comportamento esperado:** O assistente inclui na resposta a marcação explícita da versão consultada e, se a versão vigente não tiver sido formalizada, alerta que há divergência e recomenda confirmar com o supervisor.

---

### Guardrail 4 — Nunca informar percentuais de seguro de carga como definitivos sem indicar que não há respaldo normativo disponível

**Regra:** O assistente não pode informar os percentuais de seguro de carga (0,3% padrão / 0,8% perigosas) como se fossem valores oficiais da NovaTech sem indicar que esses valores constam apenas no FAQ informal e não foram confirmados por nenhum documento normativo disponível.

**Por quê:** O FAQ menciona esses percentuais e uma condicionante por data de contrato (a partir de 2023), mas nenhum documento oficial — POL-001, PROC-042, SLA-2024 — trata do tema (DIV-004, Gap G2). Informar esses valores como definitivos expõe a NovaTech a conflito com apólices reais e com contratos anteriores a 2023.

**Comportamento esperado:** O assistente sinaliza que o tema de seguro de carga não possui cobertura normativa disponível e orienta o atendente a confirmar os valores com o Comercial antes de informar ao cliente.

---

### Guardrail 5 — Nunca arbitrar qual versão de documento prevalece sem comunicado oficial

**Regra:** O assistente não pode declarar, por conta própria, que a PROC-042 v2 substitui a v1, mesmo que isso pareça logicamente correto. Enquanto não houver comunicado oficial da Diretoria Comercial formalizando a hierarquia, o assistente apresenta as duas versões e direciona a decisão ao supervisor.

**Por quê:** O FAQ já tenta fazer essa arbitragem ao orientar "na dúvida, use a mais recente (v2)", mas não tem autoridade formal para isso (DIV-003, INC-005). O assistente reproduzir essa arbitragem informal — mesmo que bem-intencionada — pode resultar em cobrança com versão não acordada contratualmente com clientes que assinaram com base nos multiplicadores da v1.

**Comportamento esperado:** O assistente apresenta as informações das duas versões lado a lado, sinaliza a ausência de hierarquia formal e instrui o atendente a não comunicar ao cliente nenhum valor calculado sem validação do supervisor.

---

## Visão Consolidada dos Fluxos

```
ATENDENTE RECEBE DÚVIDA DO CLIENTE
           │
           ▼
  CONSULTA O ASSISTENTE DE IA
           │
    ┌──────┴──────┐
    │             │
RESPOSTA      RESPOSTA COM
CONFIÁVEL     ALERTA / SEM RESPOSTA
    │             │
    ▼             ├──► Conflito entre versões → Escalar supervisor
AVALIA CONTEXTO  ├──► Sem cobertura normativa → Registrar gap + escalar
DO CLIENTE       ├──► Tema com validação obrigatória → Área responsável
    │             └──► Atendente discorda → Não usar + acionar feedback
    ▼
USA RESPOSTA NO ATENDIMENTO
    │
    ▼
ENCERRA CHAMADO + REGISTRA CONSULTA
    │
    ├──► Resposta OK → Nenhuma ação adicional
    └──► Qualquer problema → FLUXO DE FEEDBACK
              │
              ▼
    Tipo: errada / desatualizada /
    incompleta / sem fonte / conflito
              │
              ▼
    Triagem contínua → Revisão mensal
    pelas 3 áreas → Atualização documental
    → Re-indexação do assistente
```

---

*Este documento foi produzido na fase de discovery do projeto e deve ser revisado após validação com a equipe de atendimento e as áreas de Operações, Compliance e Comercial da NovaTech.*
