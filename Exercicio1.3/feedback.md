# Feedback de Revisão — PRD-NovaTech-Assistente-RAG-versao1

**Revisor:** Claude (Papel: Revisor de Produto)
**Data da revisão:** 09/06/2026
**Documento analisado:** PRD-NovaTech-Assistente-RAG-versao1 (síntese em `promp-entrada1.3versao1.md`)
**Versão do PRD:** 1.0

---

## Sumário Executivo da Revisão

O PRD demonstra maturidade acima da média para um documento de versão 1: a curadoria de fontes é bem estruturada, o tratamento de contradições tem lógica clara e o mapeamento de riscos é honesto. Os problemas identificados abaixo são majoritariamente **gaps de especificação** (o que falta) e **ambiguidades de critério** (o que está impreciso demais para ser testável), não falhas conceituais.

Total de achados: **15 itens** — 5 gaps críticos, 6 ambiguidades, 3 inconsistências, 1 ausência de critério de aceite para RNFs.

---

## Achados Detalhados

---

### #01 — Escopo: Critério de elegibilidade no Confluence não é testável

| Campo | Detalhe |
|---|---|
| **Seção** | 2. Escopo das Fontes de Conhecimento |
| **Tipo** | Ambiguidade |
| **Severidade** | Alta |

**Evidência do problema**
> *"Wiki Confluence — somente páginas com responsável formal, versão declarada e data de revisão"*

O PRD não define o que é um "responsável formal". Qualquer editor do Confluence pode inserir um nome de campo. Não há critério de quem valida essa informação antes da indexação.

**Impactos e Riscos**
- O QA não consegue criar um teste de aceite objetivo para o filtro do Confluence.
- Páginas informais podem ser indexadas se um usuário qualquer preencher os campos, gerando ruído na base.
- Divergência de interpretação entre equipe técnica e curadoria documental no go-live.

**Feedback sobre a especificação**
Definir formalmente: "responsável formal" = colaborador com cargo mínimo de Especialista, registrado no diretório ativo da empresa; página deve conter metadado de aprovação preenchido por um dos três gestores das áreas autorizadas (Operações, Compliance, Comercial). Acrescentar critério de aceite específico para este filtro.

---

### #02 — Escopo: Critério para "versão mais recente" de planilhas é indefinido

| Campo | Detalhe |
|---|---|
| **Seção** | 2. Escopo das Fontes de Conhecimento |
| **Tipo** | Ambiguidade |
| **Severidade** | Alta |

**Evidência do problema**
> *"Planilhas de referência — pasta de rede, somente a versão mais recente de cada arquivo"*

Não está especificado o que determina "mais recente": data de modificação do arquivo, data no nome do arquivo, uma célula de controle dentro da planilha ou outro critério. Planilhas frequentemente têm datas no nome com formatos inconsistentes (ex: `SLA_jan24.xlsx`, `SLA_2024-01.xlsx`).

**Impactos e Riscos**
- Risco de indexar uma versão incorreta se o critério for ambíguo.
- Impossibilidade de validação automatizada sem critério técnico explícito.

**Feedback sobre a especificação**
Especificar: "versão mais recente é determinada pela data de modificação do arquivo no sistema de arquivos da pasta de rede; em caso de ambiguidade, prevalece o arquivo com convenção de nomenclatura homologada pela área de Operações." Listar a convenção de nomenclatura esperada como anexo ou apêndice.

---

### #03 — Regras de Contradição: Dependência não especificada de integração com sistema de chamados

| Campo | Detalhe |
|---|---|
| **Seção** | 3. Regras para Informações Contraditórias |
| **Tipo** | Gap Crítico |
| **Severidade** | Alta |

**Evidência do problema**
> *"Para chamados abertos antes de 01/12/2023 ainda em processamento, o assistente alerta para confirmação com o supervisor."*

Para aplicar essa regra, o assistente precisaria saber a data de abertura do chamado corrente. Não existe nenhum requisito funcional no PRD que especifique integração com o sistema de gestão de chamados (CRM/helpdesk). Sem essa integração, a regra é inaplicável tecnicamente.

**Impactos e Riscos**
- Requisito não implementável sem dependência não declarada.
- Pode gerar retrabalho técnico significativo se descoberto só na fase de desenvolvimento.
- Inconsistência entre a promessa do produto e a realidade técnica entregável.

**Feedback sobre a especificação**
Ou (a) criar RF-09 para integração com o sistema de chamados, incluindo a data de abertura como dado de contexto disponível para o assistente; ou (b) remover essa regra granular e substituir por: "Para temas com histórico de contradição documental, o assistente sempre exibe alerta genérico de confirmação com supervisor, independente da data do chamado."

---

### #04 — Regras de Contradição: Inconsistência entre exclusão de PROC-042 v1 e regra de runtime

| Campo | Detalhe |
|---|---|
| **Seção** | 2 e 3 (conflito entre seções) |
| **Tipo** | Inconsistência |
| **Severidade** | Alta |

**Evidência do problema**
- Seção 2: *"PROC-042 v1.0 — versão desatualizada, deve ser arquivada formalmente antes do go-live"* → **excluída da base**
- Seção 3: tabela comparativa de campos em conflito entre PROC-042 v1 e v2 com regra de alerta de contradição
- Risco R-01: *"PROC-042 v1 e v2 coexistem sem resolução formal — Alta probabilidade / Alto impacto"*

Se a PROC-042 v1 é excluída da base (seção 2) e seu arquivamento formal é pré-requisito de lançamento, a regra de contradição da seção 3 e o risco R-01 deixam de ser aplicáveis ao sistema em produção. A tabela comparativa é informativa para curadoria, mas não deveria gerar comportamento de runtime.

**Impactos e Riscos**
- Desenvolvimento de lógica de alerta de contradição que nunca será acionada em produção.
- Confusão para a equipe técnica sobre qual comportamento implementar.
- Risco R-01 deveria ser reclassificado após o go-live.

**Feedback sobre a especificação**
Separar claramente: (a) regra de curadoria pré-lançamento (arquivar v1 antes do go-live); (b) regra de runtime de contradição (aplicável para situações em que dois documentos vigentes conflitam). Reformular R-01 como risco de projeto (pré-lançamento), não de produto em produção.

---

### #05 — Escalação: Inconsistência nas informações de contato dos canais

| Campo | Detalhe |
|---|---|
| **Seção** | 4. Regras para Falta de Resposta |
| **Tipo** | Inconsistência |
| **Severidade** | Média |

**Evidência do problema**
> - *"Cargas perigosas → Gestão de Riscos (ramal 4500)"* ✓ tem contato específico
> - *"Negociação/descontos → Diretoria Comercial"* — sem ramal, e-mail ou canal
> - *"Questões regulatórias → Compliance"* — sem ramal, e-mail ou canal
> - *"Sem canal identificado → Supervisor imediato"* — sem critério de quem é o supervisor

**Impactos e Riscos**
- Atendente recebe escalação incompleta e perde tempo buscando o contato.
- Inconsistência de tratamento entre temas reduz a confiabilidade percebida do assistente.
- RF-07 (Sugestão de Escalação Contextual) não pode ser implementado de forma completa sem todos os contatos.

**Feedback sobre a especificação**
Completar a tabela de escalação com ramal, e-mail ou fila de atendimento para cada canal. Tratar como dados de configuração (não hardcoded), permitindo atualização sem novo deploy. Acrescentar AC correspondente ao RF-07 para cada tema de escalação.

---

### #06 — Atualização: Prazo "24 horas" — horas corridas ou úteis?

| Campo | Detalhe |
|---|---|
| **Seção** | 5. Regras para Atualização da Base |
| **Tipo** | Ambiguidade |
| **Severidade** | Média |

**Evidência do problema**
> *"SharePoint: Até 24 horas após publicação"*
> *"Confluence: Até 24 horas após publicação ou edição"*

Não está especificado se o prazo é em horas corridas (24h × 7) ou horas úteis (dentro do horário comercial 08h–18h). Um documento publicado às 17h50 de sexta ficaria disponível até 17h50 de sábado (corridas) ou até 17h50 da segunda (úteis)?

**Impactos e Riscos**
- SLA de indexação não testável objetivamente.
- Ambiguidade gera desentendimento entre produto e operações de TI na definição do monitoramento de falha.

**Feedback sobre a especificação**
Especificar explicitamente: "até 24 horas corridas" ou "até o próximo dia útil". Dado que o contexto é operacional (logística pode operar fins de semana), recomenda-se "24 horas corridas" como padrão mais seguro para o negócio.

---

### #07 — Atualização: Alerta de documentos sem revisão em 60 dias — destinatário não definido

| Campo | Detalhe |
|---|---|
| **Seção** | 5. Regras para Atualização da Base |
| **Tipo** | Gap Crítico |
| **Severidade** | Média |

**Evidência do problema**
> *"Documentos sem revisão em 60 dias geram alerta automático."*

O alerta vai para quem? Qual canal (e-mail, sistema, painel administrativo)? Quem é responsável por agir no alerta? Em quanto tempo deve ser resolvido?

**Impactos e Riscos**
- Alerta que não chega a ninguém é inútil.
- Sem responsável definido, o alerta pode ser ignorado sistematicamente, criando falsa sensação de controle.
- Requisito não testável pelo QA sem destinatário.

**Feedback sobre a especificação**
Especificar destinatário (ex: curador da área responsável pelo documento + gestor direto), canal de entrega (e-mail corporativo), prazo de resposta (ex: 5 dias úteis) e consequência da não-resposta (ex: documento marcado automaticamente como "pendente de validação" e exibido com aviso ao atendente).

---

### #08 — RF-05 e RF-06: Sem especificação do gatilho de detecção de novos documentos

| Campo | Detalhe |
|---|---|
| **Seção** | 6. Requisitos Funcionais |
| **Tipo** | Gap Crítico |
| **Severidade** | Alta |

**Evidência do problema**
> - RF-05: *"Detecta e incorpora novos documentos automaticamente"*
> - RF-06: *"Remove da base documentos arquivados" / "Entrada: marcação de obsolescência"*

RF-05 não especifica o que dispara a detecção: polling por intervalo? Webhook? Notificação por evento? Se é polling, qual a frequência? RF-06 menciona "marcação de obsolescência" mas não há requisito funcional de quem/como/onde essa marcação é feita — não existe RF para interface de curadoria.

**Impactos e Riscos**
- Sem mecanismo de detecção definido, o time técnico pode implementar soluções incompatíveis com a infraestrutura existente (SharePoint + Confluence têm APIs distintas).
- Sem interface de curadoria, a "marcação de obsolescência" precisa ser feita diretamente na fonte, sem confirmação de que o sistema processou a remoção.

**Feedback sobre a especificação**
Acrescentar: (a) mecanismo de detecção por fonte (ex: "polling a cada 4 horas no SharePoint via API Microsoft Graph; webhook no Confluence via plugin de notificação"); (b) RF-09 — Interface de Curadoria — para que gestores das 3 áreas possam marcar documentos como obsoletos diretamente no assistente, com confirmação de processamento.

---

### #09 — RNF-03: Métrica de precisão de 90% sem metodologia de medição

| Campo | Detalhe |
|---|---|
| **Seção** | 7. Requisitos Não Funcionais |
| **Tipo** | Ambiguidade / Ausência de Critério de Aceite |
| **Severidade** | Alta |

**Evidência do problema**
> *"RNF-03: Precisão mínima de 90% nas respostas para temas cobertos pela base"*

"Precisão" não está definida: é acurácia factual? Recall? F1-score? Avaliação humana? Avaliação automatizada? Por quem? Com qual conjunto de teste? Com que frequência é medida?

**Impactos e Riscos**
- Critério de aceite da homologação não pode ser definido sem metodologia.
- Métricas ambíguas geram disputas entre produto, QA e cliente sobre o que foi entregue.
- Risco de aprovação de um sistema que entrega 90% de respostas "parecidas" mas factualmente incorretas.

**Feedback sobre a especificação**
Definir: "Precisão medida por avaliação humana em amostra mínima de 100 perguntas do acervo de chamados reais, cobertos por documentação indexada, validadas por um revisor de cada área (Operações, Compliance, Comercial). Resposta considerada correta se: (a) factualmente alinhada ao documento fonte; (b) fonte citada corretamente; (c) não apresenta informação adicional fora da base."

---

### #10 — RNF-05: Autenticação na "rede corporativa" não cobre trabalho remoto/home office

| Campo | Detalhe |
|---|---|
| **Seção** | 7. Requisitos Não Funcionais |
| **Tipo** | Gap Crítico |
| **Severidade** | Média |

**Evidência do problema**
> *"RNF-05: Acesso restrito a usuários autenticados na rede corporativa"*

"Rede corporativa" implica acesso físico ou via VPN. Não está especificado se atendentes em home office (situação comum pós-pandemia, inclusive em logística) acessarão via VPN ou se haverá outra solução (SSO federado, Azure AD, etc.).

**Impactos e Riscos**
- Atendentes remotos podem ficar sem acesso ao assistente, anulando o benefício do produto para parte da equipe.
- Escopo ambíguo para arquitetura de segurança.

**Feedback sobre a especificação**
Especificar: "Acesso via autenticação corporativa SSO (Single Sign-On) integrada ao Active Directory da NovaTech, acessível via rede interna ou VPN homologada. Atendentes em home office devem usar VPN para acessar o sistema."

---

### #11 — Seção 9 (Rastreabilidade): Ausência de RF correspondente ao login do atendente

| Campo | Detalhe |
|---|---|
| **Seção** | 9. Requisitos de Rastreabilidade |
| **Tipo** | Inconsistência |
| **Severidade** | Média |

**Evidência do problema**
> *"O sistema registra por consulta: data/hora, login do atendente, texto da pergunta, documentos consultados e resposta gerada."*

O PRD exige rastreamento do "login do atendente", mas não há nenhum requisito funcional (RF) que especifique o mecanismo de autenticação e identificação do usuário. O RNF-05 menciona "usuários autenticados" mas sem RF de login explícito.

**Impactos e Riscos**
- Rastreabilidade por login requer que o sistema saiba quem é o usuário — sem RF de autenticação, isso é uma dependência implícita não especificada.
- Logs com identificação nominal de colaboradores são dados pessoais sujeitos à LGPD (ver também #12).

**Feedback sobre a especificação**
Criar RF-09 (ou RF conforme numeração ajustada): "Autenticação de Usuário — o sistema identifica o atendente pelo login corporativo (integrado ao AD/SSO) e associa cada consulta ao usuário autenticado." Vincular RF ao RNF-05 e à seção 9.

---

### #12 — Gap de LGPD: Logs com consultas de usuários sem menção à privacidade de dados

| Campo | Detalhe |
|---|---|
| **Seção** | 7 (RNF-07) e 9 (Rastreabilidade) |
| **Tipo** | Gap Crítico |
| **Severidade** | Alta |

**Evidência do problema**
> *"RNF-07: Logs de auditoria retidos por no mínimo 12 meses"*
> *"O sistema registra: login do atendente, texto da pergunta, documentos consultados e resposta gerada."*

Os logs contêm: (a) dado pessoal do colaborador (login); (b) conteúdo de perguntas que podem expor dados de clientes finais da NovaTech (ex: "Qual o prazo para a devolução do pedido do cliente X?"). Nenhuma seção do PRD menciona LGPD, consentimento, minimização de dados, acesso restrito aos logs ou processo de resposta a direitos do titular.

**Impactos e Riscos**
- Risco de não conformidade com LGPD (Lei 13.709/2018).
- Logs retidos por 12 meses com dados pessoais sem base legal definida.
- Potencial exposição de dados de clientes finais da NovaTech nos registros de auditoria.

**Feedback sobre a especificação**
Acrescentar seção ou RNF dedicado: definir base legal para retenção dos logs (ex: legítimo interesse para auditoria contratual); especificar que perguntas contendo dados pessoais de clientes devem ser anonimizadas antes do armazenamento; definir perfis de acesso ao log (restrito a Compliance e gestores, não disponível para atendentes comuns).

---

### #13 — Seção 8: Critérios de aceite ausentes para todos os RNFs

| Campo | Detalhe |
|---|---|
| **Seção** | 8. Critérios de Aceitação |
| **Tipo** | Ausência de Critério de Aceite |
| **Severidade** | Alta |

**Evidência do problema**
> A seção 8 apresenta um critério de aceite por RF (RF-01 a RF-08), mas nenhum critério de aceite para os 7 RNFs.

Como o QA valida RNF-01 (10 segundos, 95% dos casos)? Com que carga de usuários simultâneos? Como valida RNF-02 (99% de disponibilidade)? Em qual período de observação?

**Impactos e Riscos**
- Homologação de RNFs sem critério definido é subjetiva e disputável.
- Risco de o sistema ir a produção sem validação formal de desempenho e disponibilidade.

**Feedback sobre a especificação**
Acrescentar ACs para cada RNF. Exemplos mínimos:
- RNF-01: "Teste de carga com 45 usuários simultâneos (toda a equipe) durante 30 minutos; 95% das consultas respondidas em até 10 segundos."
- RNF-02: "Monitoramento por 30 dias em staging com SLA calculado; disponibilidade ≥ 99% em horário comercial."
- RNF-04: "100% de uma amostra aleatória de 50 respostas deve conter documento, versão e seção. Qualquer resposta sem fonte = falha imediata."

---

### #14 — Curadoria mensal: Processo sem dono, pauta ou critério de decisão

| Campo | Detalhe |
|---|---|
| **Seção** | 5. Regras para Atualização da Base |
| **Tipo** | Gap |
| **Severidade** | Média |

**Evidência do problema**
> *"Curadoria mensal obrigatória com as 3 áreas (Operações, Compliance, Comercial)."*

Não está especificado: quem convoca? Quem facilita? Qual é a pauta mínima? O que acontece quando as áreas divergem sobre uma decisão documental? Quem tem a palavra final?

**Impactos e Riscos**
- Processo sem dono é processo que não acontece.
- Divergência entre áreas pode bloquear decisões e deixar documentos contraditórios na base.

**Feedback sobre a especificação**
Definir: responsável pela convocação (sugestão: Product Manager do assistente ou área de Operações como líder); pauta mínima (revisão das perguntas sem resposta do mês, revisão de documentos com alerta de 60 dias, validação de novos documentos indexados); critério de decisão em caso de impasse (ex: voto da área de Compliance prevalece em questões regulatórias; Operações em questões de processo).

---

### #15 — Interface do usuário: Completamente ausente no PRD

| Campo | Detalhe |
|---|---|
| **Seção** | Produto como um todo |
| **Tipo** | Gap Crítico |
| **Severidade** | Alta |

**Evidência do problema**
O PRD não especifica em nenhum momento como o atendente acessa o assistente: é uma aba no CRM atual? Uma aplicação web separada? Um widget embutido? Um chat em canal Teams/Slack? Uma aplicação desktop?

**Impactos e Riscos**
- Sem definição de interface, o time técnico não consegue estimar esforço ou escolher stack.
- O objetivo de "menos de 2 minutos" depende criticamente de onde e como o assistente está posicionado no fluxo de trabalho do atendente — uma janela separada tem fricção muito maior do que um painel integrado ao CRM.
- RF-01 (consulta em linguagem natural) pressupõe uma interface de entrada de texto, mas ela nunca é especificada.

**Feedback sobre a especificação**
Acrescentar seção "Interface e Integração" com: (a) canal de acesso (web app, integração CRM, bot de mensageria); (b) fluxo de uso mínimo (atendente está no chamado → abre painel → digita pergunta → recebe resposta com fonte); (c) requisito de integração com sistema de chamados (se aplicável ao item #03). Mesmo em versão 1 de descoberta, a interface deve ser descrita em nível de comportamento esperado, não de UI detalhada.

---

## Quadro Consolidado de Achados

| # | Seção | Tipo | Severidade | Título Resumido |
|---|---|---|---|---|
| 01 | 2 — Escopo | Ambiguidade | Alta | Critério de elegibilidade no Confluence não testável |
| 02 | 2 — Escopo | Ambiguidade | Alta | "Versão mais recente" de planilhas sem critério técnico |
| 03 | 3 — Contradições | Gap Crítico | Alta | Regra de chamados requer integração não especificada |
| 04 | 2 e 3 | Inconsistência | Alta | PROC-042 v1 excluída da base mas tratada em regra de runtime |
| 05 | 4 — Falta de resposta | Inconsistência | Média | Canais de escalação com informações de contato incompletas |
| 06 | 5 — Atualização | Ambiguidade | Média | Prazo de 24h — horas corridas ou úteis? |
| 07 | 5 — Atualização | Gap Crítico | Média | Alerta de 60 dias sem destinatário definido |
| 08 | 6 — RF-05/RF-06 | Gap Crítico | Alta | Gatilho de detecção de novos documentos não especificado |
| 09 | 7 — RNF-03 | Ambiguidade | Alta | Métrica de 90% de precisão sem metodologia de medição |
| 10 | 7 — RNF-05 | Gap Crítico | Média | Autenticação não cobre trabalho remoto/home office |
| 11 | 9 — Rastreabilidade | Inconsistência | Média | Login do atendente sem RF correspondente de autenticação |
| 12 | 7 e 9 | Gap Crítico | Alta | Logs com dados pessoais sem menção à LGPD |
| 13 | 8 — Critérios de Aceite | Ausência de AC | Alta | Nenhum critério de aceite para os 7 RNFs |
| 14 | 5 — Atualização | Gap | Média | Curadoria mensal sem dono, pauta ou critério de decisão |
| 15 | Produto (geral) | Gap Crítico | Alta | Interface do usuário completamente ausente no PRD |

---

## Pontos Positivos da Especificação

Para equilibrar o feedback, os seguintes aspectos demonstram qualidade acima do esperado para uma versão 1:

- **Curadoria de fontes bem fundamentada**: A distinção entre fontes indexadas e excluídas, com justificativas por documento, é precisa e acionável.
- **Honestidade sobre gaps documentais**: A seção 4 lista explicitamente os temas sem cobertura formal — isso é maturidade de produto.
- **Pré-requisitos de lançamento claros**: O arquivamento da PROC-042 v1 como pré-requisito é uma decisão de produto correta e bem colocada.
- **Requisitos funcionais com ação, entrada e saída**: A estrutura da tabela RF facilita a escrita de casos de teste.
- **Mapeamento de riscos realista**: O risco R-05 (atendente confia sem verificar) é um risco comportamental raro de aparecer em PRDs de v1.
- **Objetivo do sistema conciso e mensurável**: As 3 linhas do objetivo são objetivas, orientadas ao usuário e têm meta quantificada.

---

*Fim do feedback — PRD-NovaTech-Assistente-RAG-versao1 | Revisão: 09/06/2026*
