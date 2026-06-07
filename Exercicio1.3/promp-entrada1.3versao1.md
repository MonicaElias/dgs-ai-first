# Prompt de Entrada 1.3 — Especificação de Requisitos do Produto (PRD) para Pipeline de RAG

## Metadados

- **Data:** 07/06/2026
- **Papel do usuário:** Product Specialist
- **Fase do projeto:** Intenção e Descoberta
- **Empresa:** NovaTech
- **Artefato gerado:** `PRD-NovaTech-Assistente-RAG-versao1.docx`

---

## Prompt do Usuário

### Contexto do Cenário Atual

A NovaTech é uma empresa de médio porte do setor de logística com 1.200 funcionários. Sua operação depende de um conjunto extenso de documentação interna: manuais de procedimento operacional, políticas de compliance, tabelas de SLA por tipo de cliente, regras de cálculo de frete, e normas de segurança de carga.

Hoje, essa documentação está espalhada em três fontes: um SharePoint corporativo com 800 documentos (PDFs e Word), uma wiki interna no Confluence com 400 páginas, e uma pasta de rede com planilhas de referência atualizadas mensalmente.

O problema: a equipe de atendimento ao cliente (45 pessoas) gasta em média 12 minutos por chamado buscando informações nessas fontes para responder dúvidas de clientes sobre prazos, regras de frete, políticas de devolução e procedimentos de reclamação. Isso gera atrasos, respostas inconsistentes e frustração tanto dos atendentes quanto dos clientes.

- O volume médio é de 320 chamados/dia, dos quais 60% envolvem consulta a documentação.
- A documentação é atualizada mensalmente por 3 áreas diferentes (Operações, Compliance, Comercial), sem processo unificado de revisão.
- Alguns documentos se contradizem entre versões — a equipe de atendimento hoje resolve isso "perguntando para quem sabe".
- A expectativa da diretoria é reduzir o tempo médio de busca de 12 para menos de 2 minutos por chamado.

### Dados do Discovery da Jornada do Atendente

- Os atendentes hoje abrem em média 4 fontes diferentes por chamado.
- As dúvidas mais comuns são sobre prazos de entrega (35%), regras de frete (25%), política de devolução (20%) e outros (20%).
- Em 15% dos casos, o atendente não encontra resposta e escala para o supervisor.

### Inputs Fornecidos

- Cenário completo do Projeto
- Dados coletados durante o Discovery
- `anexo-a-documentacao-simulada-novatech.md`
- Explicação simplificada do pipeline de RAG: *"Documentos são divididos em pedaços (chunks), transformados em representações numéricas (embeddings), armazenados num banco vetorial, e recuperados por similaridade quando o usuário faz uma pergunta. O LLM então gera uma resposta usando os chunks recuperados como contexto."*

### Objetivo

Gerar uma especificação de requisitos do produto (não técnica, mas precisa) que contenha:

1. Fontes de dados que devem ser indexadas e quais fontes devem ser excluídas por estarem obsoletas.
2. Como o assistente deve lidar com documentos contraditórios.
3. Qual o comportamento esperado quando a pergunta do atendente não tem resposta na base de dados.
4. Requisitos de atualização quando houver novos documentos publicados, em quanto tempo devem estar disponíveis no assistente.
5. Requisitos de rastreabilidade das respostas citando a fonte e trechos relevantes.

### Regras

- Não usar conhecimento externo aos documentos anexados.
- As respostas devem ser confiáveis e baseadas na documentação da empresa.
- A escrita deve ser em linguagem não técnica, usando a linguagem de produto.
- A escrita deve conter as regras relacionadas ao negócio, já habituadas pelos atendentes.

### Critérios de Avaliação

- Os requisitos demonstram entendimento de que a qualidade do RAG depende da curadoria dos dados, não só da tecnologia.
- O tratamento de contradições e ausência de resposta mostra maturidade de produto.
- Os requisitos são testáveis (o QA conseguiria verificar cada um).

### Estrutura de Saída Solicitada

1. Objetivo do Sistema (máximo 3 linhas)
2. Escopo das fontes de conhecimento indexadas e não indexadas
3. Regras para informações contraditórias
4. Regras para falta de respostas do assistente e comportamento esperado
5. Regras para requisitos de atualização da base de dados
6. Requisitos funcionais em tabela: ID, Nome do Requisito, Descrição (Ação, Entradas e Saídas)
7. Requisitos não funcionais em tabela: ID, Nome do Requisito Não Funcional, Categoria
8. Critérios de Aceitação e Regras de Negócio para os requisitos do item 6
9. Requisitos de rastreabilidade das respostas do sistema
10. Mapeamento dos Riscos
11. Resumo executivo (máximo 12 linhas)

**Saída:** Documento Word com o nome `PRD-NovaTech-Assistente-RAG-versao1.docx`

---

## Resposta do Assistente

### Abordagem

O assistente leu o arquivo `anexo-a-documentacao-simulada-novatech.md`, que contém 5 documentos simulados da NovaTech:

| Documento | Versão | Data | Tipo |
|-----------|--------|------|------|
| POL-001 — Política de Devolução de Mercadorias | 3.1 | 15/01/2024 | Normativo |
| PROC-042 — Procedimento de Cálculo de Frete Especial | 1.0 | 03/03/2023 | Operacional (obsoleto) |
| PROC-042-v2 — Procedimento de Cálculo de Frete Especial (Revisado) | 2.0 | 10/11/2023 | Operacional (vigente) |
| SLA-2024 — Tabela de SLA por Tipo de Cliente | 2024.1 | 02/01/2024 | Contratual |
| FAQ-Atendimento — Perguntas Frequentes do Time de Suporte | Não controlada | Diversas | Informal (excluído) |

Identificou contradições e gaps documentais antes de especificar os requisitos. Gerou o documento Word usando automação via Microsoft Word COM (Word 16.0).

---

### Síntese do PRD Gerado

#### 1. Objetivo do Sistema

O Assistente NovaTech é uma ferramenta de consulta inteligente que permite ao time de atendimento encontrar respostas precisas sobre prazos, fretes, devoluções e SLAs em menos de 2 minutos, consultando exclusivamente a documentação oficial indexada. O sistema cita a fonte de cada resposta e alerta o atendente em casos de contradições documentais ou ausência de informação. O assistente apoia a decisão do atendente, mas não substitui a validação humana em situações de exceção.

---

#### 2. Escopo das Fontes de Conhecimento

**Fontes indexadas (incluir):**
- POL-001 — Política de Devolução de Mercadorias (v3.1 | SharePoint | 15/01/2024)
- PROC-042-v2 — Procedimento de Cálculo de Frete Especial Revisado (v2.0 | SharePoint | 10/11/2023)
- SLA-2024 — Tabela de SLA por Tipo de Cliente (v2024.1 | SharePoint | 02/01/2024)
- Wiki Confluence — somente páginas com responsável formal, versão declarada e data de revisão
- Planilhas de referência — pasta de rede, somente a versão mais recente de cada arquivo

**Fontes NÃO indexadas (excluir):**
- PROC-042 v1.0 — versão desatualizada, deve ser arquivada formalmente antes do go-live (**pré-requisito de lançamento**)
- FAQ-Atendimento — documento informal sem validação de Compliance ou Operações
- PROC-043 — em processo de revisão pelo Compliance, não publicar até conclusão
- Documentos sem responsável formal, versão declarada ou data de revisão
- Versões anteriores de planilhas na pasta de rede

---

#### 3. Regras para Informações Contraditórias

- O assistente usa sempre a versão com data de emissão mais recente como referência principal.
- Alerta o atendente sobre o conflito antes de apresentar a resposta.
- Nunca apresenta dois valores diferentes como igualmente válidos.

**Caso específico — PROC-042 v1 vs v2:**

| Campo em conflito | PROC-042 v1 | PROC-042 v2 (vigente) |
|-------------------|-------------|----------------------|
| Multiplicador Sul | 1,2 | 1,3 |
| Multiplicador Sudeste | 1,0 | 1,1 |
| Multiplicador Centro-Oeste | 1,3 | 1,4 |
| Multiplicador Nordeste | 1,4 | 1,5 |
| Multiplicador Norte | 1,6 | 1,8 |
| Fator de peso (1.001–3.000 kg) | 1,2 | 1,15 |
| Fator de peso (acima de 3.000 kg) | 1,5 | 1,4 |
| Prazo adicional frete especial | +2 dias úteis | +3 dias úteis |

Para chamados abertos antes de 01/12/2023 ainda em processamento, o assistente alerta para confirmação com o supervisor.

---

#### 4. Regras para Falta de Resposta

- Informa claramente: *"Não encontrei resposta para essa pergunta na documentação disponível."*
- Não especula, estima ou usa informação fora da base.
- Sugere canal de escalação específico por tema:
  - Cargas perigosas → Gestão de Riscos (ramal 4500)
  - Negociação/descontos → Diretoria Comercial
  - Questões regulatórias → Compliance
  - Sem canal identificado → Supervisor imediato
- Registra a pergunta sem resposta para curadoria mensal.

**Gaps documentais identificados (sem cobertura formal):**
- Política de carga danificada em trânsito
- Seguro de carga (percentuais e condições)
- Frete padrão para cargas abaixo de 500 kg
- Processo interno da Gestão de Riscos após ramal 4500

---

#### 5. Regras para Atualização da Base

| Tipo de fonte | Prazo para disponibilização |
|---------------|-----------------------------|
| SharePoint | Até 24 horas após publicação |
| Confluence | Até 24 horas após publicação ou edição |
| Planilhas (pasta de rede) | Até 4 horas após atualização do arquivo |
| Remoção de documentos obsoletos | Até 4 horas após marcação de obsolescência |

- Curadoria mensal obrigatória com as 3 áreas (Operações, Compliance, Comercial).
- Documentos sem revisão em 60 dias geram alerta automático.
- O processo não exige ação manual do atendente — é completamente automático.

---

#### 6. Requisitos Funcionais

| ID | Nome do Requisito | Ação | Entradas e Saídas |
|----|-------------------|------|-------------------|
| RF-01 | Consulta em Linguagem Natural | Atendente digita pergunta; assistente busca na base e retorna resposta | Entrada: texto livre. Saída: resposta objetiva + fonte |
| RF-02 | Citação Obrigatória da Fonte | Para toda resposta, apresenta documento de origem | Entrada: resposta gerada. Saída: nome, versão, seção e trecho |
| RF-03 | Alerta de Contradição Documental | Alerta sobre conflito antes de responder, indicando versão vigente | Entrada: pergunta sobre tema contraditório. Saída: resposta com flag + instrução de confirmação |
| RF-04 | Sinalização de Ausência de Resposta | Informa claramente quando não há resposta e sugere escalação | Entrada: pergunta sem cobertura. Saída: mensagem de não encontrado + canal de escalação |
| RF-05 | Indexação Automática de Novos Documentos | Detecta e incorpora novos documentos automaticamente | Entrada: documento novo nas fontes. Saída: disponível para consulta dentro do prazo |
| RF-06 | Remoção Automática de Documentos Obsoletos | Remove da base documentos arquivados | Entrada: marcação de obsolescência. Saída: documento indisponível em até 4h + notificação |
| RF-07 | Sugestão de Escalação Contextual | Identifica o tema e sugere canal correto de escalação | Entrada: pergunta sem resposta + tema. Saída: canal específico (ramal, e-mail ou área) |
| RF-08 | Registro de Perguntas Sem Resposta | Registra automaticamente perguntas não atendidas | Entrada: evento de ausência de resposta. Saída: registro disponível em relatório mensal |

---

#### 7. Requisitos Não Funcionais

| ID | Requisito Não Funcional | Categoria |
|----|------------------------|-----------|
| RNF-01 | Tempo de resposta máximo de 10 segundos por consulta (95% dos casos) | Desempenho |
| RNF-02 | Disponibilidade mínima de 99% no horário comercial (08h–18h, dias úteis) | Confiabilidade |
| RNF-03 | Precisão mínima de 90% nas respostas para temas cobertos pela base | Qualidade |
| RNF-04 | 100% das respostas devem conter referência ao documento de origem | Rastreabilidade |
| RNF-05 | Acesso restrito a usuários autenticados na rede corporativa | Segurança |
| RNF-06 | Interface e respostas em português brasileiro, linguagem acessível ao atendente | Usabilidade |
| RNF-07 | Logs de auditoria retidos por no mínimo 12 meses | Conformidade |

---

#### 8. Critérios de Aceitação (exemplos por RF)

- **RF-01:** Pergunta "Qual o prazo para devolução?" → resposta "7 dias úteis" com citação de POL-001 seção 3.1.
- **RF-02:** Toda resposta contém nome, versão e seção do documento. Resposta sem fonte não é exibida.
- **RF-03:** Pergunta sobre multiplicador Sul → exibe 1,3 (v2) com alerta que v1 apresentava 1,2.
- **RF-04:** Pergunta sobre seguro de carga → "Não encontrei documentação formal. Consulte o Comercial."
- **RF-05:** Documento publicado no SharePoint disponível em até 24h — testável com documento de controle do QA.
- **RF-06:** Documento obsoleto desaparece da base em até 4h — testável tentando consultar o conteúdo removido.
- **RF-07:** Pergunta sobre cargas perigosas sem resposta → sugestão do ramal 4500 (Gestão de Riscos).
- **RF-08:** Relatório mensal disponível até o 5.º dia útil com: data/hora, texto da pergunta e motivo.

---

#### 9. Requisitos de Rastreabilidade

Cada resposta do assistente deve conter:
- Nome do documento de origem
- Número da versão
- Seção ou capítulo consultado
- Trecho literal ou identificação do trecho utilizado

O sistema registra por consulta: data/hora, login do atendente, texto da pergunta, documentos consultados e resposta gerada. Logs retidos por 12 meses para auditoria e contestações contratuais.

---

#### 10. Mapeamento dos Riscos

| ID | Risco | Probabilidade | Impacto | Mitigação |
|----|-------|--------------|---------|-----------|
| R-01 | PROC-042 v1 e v2 coexistem sem resolução formal | Alta | Alto | Arquivamento da v1 pela Comercial = **pré-requisito de lançamento** |
| R-02 | FAQ informal indexado como fonte válida | Alta | Alto | FAQ excluído do escopo; gaps do FAQ formalizados antes do go-live |
| R-03 | Gaps documentais em temas frequentes (15–20% das dúvidas) | Média | Médio | Criar documentos formais para os temas identificados antes do lançamento |
| R-04 | Atraso na indexação entrega informação desatualizada | Média | Alto | Alertas automáticos por e-mail em caso de falha ou atraso na indexação |
| R-05 | Atendente confia no assistente sem verificar fonte em exceções | Média | Alto | Treinamento obrigatório; assistente sempre exibe fonte e instrução de confirmação |
| R-06 | Documentação desatualizada permanece na base sem curadoria | Média | Alto | Revisão mensal formalizada; alerta para docs sem revisão em 60 dias |
| R-07 | PROC-043 em revisão pode conter informações conflitantes | Alta | Médio | Não indexar até publicação formal pelo Compliance |

---

#### 11. Resumo Executivo

O Assistente RAG NovaTech tem como meta reduzir o tempo médio de busca de informação de 12 para menos de 2 minutos por chamado, impactando diretamente os 192 chamados diários que dependem de consulta documental. O sistema indexará exclusivamente documentos oficiais validados — POL-001, PROC-042-v2, SLA-2024, páginas formais do Confluence e planilhas de referência — excluindo o FAQ informal e versões obsoletas.

A entrega de valor depende mais da curadoria documental do que da tecnologia: sem resolver a coexistência das versões da PROC-042 e sem formalizar os gaps identificados, o assistente não cobrirá 15 a 20% das dúvidas mais comuns. O pré-requisito de lançamento é o arquivamento formal da PROC-042 v1 pela Diretoria Comercial.

Foram especificados 8 requisitos funcionais e 7 não funcionais, todos com critérios de aceitação testáveis pelo QA. O sistema garante rastreabilidade completa com logs de 12 meses para auditoria e contestações contratuais. A estratégia de ausência de resposta com escalação contextual protege o atendente de dar informações incorretas ao cliente.

---

## Artefato Gerado

- **Arquivo:** `PRD-NovaTech-Assistente-RAG-versao1.docx`
- **Localização:** `Prática 1/`
- **Formato:** Microsoft Word (.docx)
- **Tamanho:** ~27 KB
- **Seções:** 11 (conforme estrutura solicitada)
- **Ferramenta usada para geração:** PowerShell + Microsoft Word COM (Word 16.0)
