# Recorte de Domínio — NovaTech Assistant
**Versão:** 1.0  
**Data:** 2026-06-11  
**Responsável:** Product Specialist  
**Projeto:** Assistente de IA para Atendimento ao Cliente NovaTech

---

## 1. Visão Geral do Domínio

O assistente NovaTech opera no domínio de **logística de cargas**, respondendo perguntas do time de atendimento ao cliente com base na documentação oficial da empresa. O objetivo é reduzir o tempo médio de resolução de chamados de 12 minutos para 2 minutos, integrando-se ao ambiente Microsoft (Teams + SharePoint).

---

## 2. Bounded Contexts — Tabela Geral

| # | Nome do Contexto | Nome em Linguagem Ubíqua | Responsabilidade Principal | Principais Conceitos/Entidades | Relaciona-se com |
|---|-----------------|--------------------------|---------------------------|-------------------------------|------------------|
| 1 | Atendimento ao Cliente | **Central de Atendimento** | Receber perguntas dos atendentes em linguagem natural, retornar respostas fundamentadas na documentação oficial com indicação da fonte | Chamado, Atendente, Pergunta, Resposta, Fonte, Confiança da resposta, Escalação | Gestão Documental, Logística e Frete, SLA e Contratos |
| 2 | Gestão Documental | **Base de Conhecimento** | Organizar, versionar e disponibilizar a documentação oficial da NovaTech para consulta pelo assistente; tratar documentos contraditórios ou desatualizados | Documento, Versão, Vigência, Chunk, Índice de busca, Metadado, Contradição, Obsolescência, Fonte normativa vs. informal | Atendimento ao Cliente, todos os demais contextos |
| 3 | Logística e Frete | **Mesa de Operações** | Definir regras sobre tipos de carga, cálculo de frete, prazos de entrega, procedimentos de devolução e coleta reversa | Carga, CT-e, Frete especial, Frete padrão, Multiplicador regional, Fator de peso, Prazo de entrega, Coleta reversa, Devolução, Carga perigosa (classes 1–6 ANTT), Carga refrigerada, Lacre de segurança, Interceptação de carga | Atendimento ao Cliente, SLA e Contratos, Gestão Documental |
| 4 | SLA e Contratos | **Gestão de Nível de Serviço** | Garantir que os compromissos contratuais com clientes (prazos de resposta, resolução e disponibilidade) sejam conhecidos e respeitados; definir tiers de cliente e penalidades | Tier de cliente (Gold, Silver, Standard), SLA de resposta, SLA de resolução, Incidente crítico, Penalidade, Gerente de conta, Disponibilidade do portal, Contrato anual | Atendimento ao Cliente, Logística e Frete |
| 5 | Assistente IA | **Copiloto de Atendimento** | Orquestrar a busca semântica, montar o prompt com os chunks recuperados, acionar o modelo de linguagem e retornar a resposta com indicação de fonte; controlar guardrails e baixa confiança | Embedding, Busca vetorial, Chunk, Context budget, Guardrail, Confiança, Versão de documento ativa, Resposta fundamentada, Aviso de baixa confiança | Gestão Documental (leitura), Atendimento ao Cliente (interface), todos os contextos de negócio (como consumidor) |

---

## 3. Detalhamento por Bounded Context

### 3.1 Atendimento ao Cliente — *Central de Atendimento*

**Responsabilidade principal:** Ponto de entrada do atendente. Recebe perguntas em linguagem natural via Teams, aciona o assistente e exibe a resposta com a fonte da informação.

**Principais conceitos/entidades:**
- **Chamado:** unidade de trabalho aberta pelo atendente para resolver uma demanda do cliente final
- **Atendente:** usuário interno que consulta o assistente
- **Pergunta:** input em linguagem natural do atendente
- **Resposta fundamentada:** output do assistente com conteúdo e indicação de fonte
- **Escalação:** encaminhamento de chamado para área especializada (Gestão de Riscos, Jurídico, Comercial) quando o assistente indica que a resposta está fora do escopo ou é de baixa confiança
- **Aviso de baixa confiança:** sinalização explícita ao atendente quando o assistente não encontrou resposta com segurança suficiente

**Relaciona-se com:**
- *Gestão Documental* — para recuperar os documentos que fundamentam a resposta
- *Logística e Frete* — para responder perguntas sobre devolução, frete e prazos
- *SLA e Contratos* — para responder perguntas sobre prazos de atendimento e tiers de cliente

---

### 3.2 Gestão Documental — *Base de Conhecimento*

**Responsabilidade principal:** Armazenar, versionar e indexar a documentação da NovaTech. Garantir que o assistente acesse sempre a versão vigente dos documentos e sinalizar quando há contradição entre versões.

**Principais conceitos/entidades:**
- **Documento normativo:** POL (política), PROC (procedimento), SLA — têm responsável formal e classificação
- **Documento informal:** FAQ — sem responsável formal, não validado pelo Compliance; pode conter informações desatualizadas
- **Versão / Vigência:** identificador que determina qual versão de um documento está ativa (ex: PROC-042 v1 vs. v2)
- **Contradição documental:** situação em que dois documentos ativos apresentam informações divergentes (ex: multiplicadores regionais diferentes entre PROC-042-v1 e PROC-042-v2)
- **Chunk:** fragmento de documento processado para busca vetorial
- **Metadado de vigência:** campo que indica se o documento é o mais recente e deve ser priorizado

**Relaciona-se com:**
- *Assistente IA* — fornece os chunks recuperados para composição da resposta
- Todos os demais contextos de negócio — como fonte de verdade

---

### 3.3 Logística e Frete — *Mesa de Operações*

**Responsabilidade principal:** Definir as regras operacionais de transporte: tipos de carga, cálculo de frete especial, prazos, devoluções e coleta reversa.

**Principais conceitos/entidades:**
- **Frete especial:** modalidade aplicável a cargas acima de 500 kg, calculada com multiplicador regional e fator de peso
- **Frete padrão:** modalidade para cargas abaixo de 500 kg (não coberta pelos documentos indexados — gap identificado)
- **Multiplicador regional:** fator que ajusta o valor do frete conforme a região de destino (Sul, Sudeste, Centro-Oeste, Nordeste, Norte)
- **Fator de peso:** coeficiente aplicado à faixa de peso (500–1.000 kg, 1.001–3.000 kg, acima de 3.000 kg)
- **CT-e (Conhecimento de Transporte Eletrônico):** documento fiscal que acompanha a carga
- **Carga perigosa:** cargas classificadas nas classes 1 a 6 da ANTT; não elegíveis para devolução pelo processo padrão
- **Carga refrigerada:** carga que exige controle de temperatura; não elegível para devolução se a cadeia de frio foi rompida
- **Coleta reversa:** operação de retirada da mercadoria devolvida no cliente
- **Devolução:** processo formal de retorno de mercadoria após entrega, sujeito a prazo (7 dias úteis) e documentação

**Relaciona-se com:**
- *Atendimento ao Cliente* — as perguntas mais frequentes sobre prazos e fretes vêm deste contexto
- *SLA e Contratos* — prazos de entrega impactam cumprimento de SLA
- *Gestão Documental* — as regras estão nos documentos PROC-042 v1/v2 e POL-001

---

### 3.4 SLA e Contratos — *Gestão de Nível de Serviço*

**Responsabilidade principal:** Definir e monitorar os compromissos contratuais com clientes, classificados em tiers, com prazos, penalidades e regras de escalação.

**Principais conceitos/entidades:**
- **Tier de cliente:** classificação do cliente em Gold, Silver ou Standard (não existem outros tiers)
  - **Gold:** contrato acima de R$ 500.000/ano ou mais de 200 operações/mês
  - **Silver:** contrato entre R$ 100.000 e R$ 500.000/ano ou entre 50 e 200 operações/mês
  - **Standard:** todos os demais
- **SLA de resposta:** tempo máximo para o primeiro retorno ao cliente (Gold: 2h, Silver: 4h, Standard: 8h)
- **SLA de resolução:** tempo máximo para fechar o chamado (Gold: 24h, Silver: 48h, Standard: 72h)
- **Incidente crítico:** chamado classificado como urgente (carga de alto valor sem rastreio, carga perigosa com irregularidade, múltiplos chamados do mesmo cliente, risco à segurança)
- **Penalidade:** crédito aplicado sobre o valor do frete em caso de violação de SLA
- **Gerente de conta:** recurso exclusivo do tier Gold

**Relaciona-se com:**
- *Atendimento ao Cliente* — o atendente precisa saber o SLA do cliente antes de orientá-lo
- *Logística e Frete* — incidentes críticos frequentemente envolvem cargas em trânsito

---

### 3.5 Assistente IA — *Copiloto de Atendimento*

**Responsabilidade principal:** Orquestrar o pipeline RAG (recuperação + geração): converter a pergunta do atendente em busca semântica, recuperar os chunks relevantes, montar o prompt respeitando o context budget e retornar a resposta com fonte.

**Principais conceitos/entidades:**
- **Embedding:** representação vetorial da pergunta para busca semântica
- **Busca vetorial:** recuperação dos chunks mais relevantes no índice Azure AI Search
- **Context budget:** limite de tokens por consulta (~4K para system prompt + ~8K para chunks)
- **Guardrail:** regra de comportamento obrigatória (ex: nunca inventar prazos, sempre citar fonte)
- **Confiança:** grau de certeza do assistente sobre a resposta; baixa confiança dispara aviso
- **Versão ativa de documento:** critério de priorização quando há contradição documental (sempre a mais recente)

**Relaciona-se com:**
- *Gestão Documental* — consome os chunks e metadados de vigência
- *Atendimento ao Cliente* — entrega a resposta ao atendente via Teams
- *Logística e Frete*, *SLA e Contratos* — consome as regras de negócio como conteúdo das respostas

---

## 4. Fronteiras Críticas (onde os contextos se tocam e podem gerar confusão)

| Fronteira | Contextos envolvidos | Risco de confusão | Decisão de projeto |
|-----------|---------------------|-------------------|-------------------|
| **Documento normativo vs. FAQ informal** | Gestão Documental ↔ Atendimento ao Cliente | O FAQ contém informações práticas do time, mas não foi validado pelo Compliance. O assistente pode retornar resposta do FAQ contradizendo a POL ou PROC | O assistente deve priorizar documentos normativos (POL, PROC, SLA). Respostas baseadas apenas no FAQ devem incluir aviso de "fonte não normativa" |
| **PROC-042 v1 vs. PROC-042 v2 (contradição)** | Gestão Documental ↔ Logística e Frete | Duas versões ativas com multiplicadores e prazos diferentes. O assistente pode responder com valores da versão errada | Priorizar sempre a versão mais recente (v2). Informar o atendente da existência de versão anterior. Chamados abertos antes de 01/12/2023 usam a v1 |
| **Carga perigosa e devolução** | Logística e Frete ↔ Atendimento ao Cliente | O FAQ (item 3) sugere que pode haver exceções para devolução de carga perigosa; a POL-001 diz que não é elegível pelo processo padrão | O assistente deve seguir a POL-001 e nunca afirmar que carga perigosa (classes 1–6 ANTT) pode ser devolvida pelo processo padrão. Encaminhar ao ramal 4500 |
| **Tier de cliente e SLA** | SLA e Contratos ↔ Atendimento ao Cliente | Cliente pode reivindicar tier "Platinum" ou outro inexistente | O assistente deve informar que só existem três tiers: Gold, Silver e Standard |
| **Frete padrão (gap documental)** | Logística e Frete ↔ Gestão Documental | Não há documento indexado sobre frete abaixo de 500 kg | O assistente deve informar que não há informação disponível sobre frete padrão e orientar a consultar o Comercial |
| **Carga danificada vs. Devolução** | Logística e Frete ↔ Atendimento ao Cliente | Processo de carga danificada em trânsito é diferente de devolução, mas apenas o FAQ (informal) descreve esse processo | O assistente deve diferenciar os dois processos e encaminhar carga danificada para sinistros@novatech.com.br, com aviso de que a informação vem de fonte não normativa |

---

## 5. Fronteiras do Assistente: O que FAZ e o que NÃO FAZ

### O que o Assistente FAZ

| Capacidade | Contexto de negócio | Fonte de informação |
|-----------|--------------------|--------------------|
| Responde perguntas sobre regras de devolução de mercadorias | Logística e Frete | POL-001 |
| Informa o cálculo de frete especial (acima de 500 kg) e os multiplicadores regionais | Logística e Frete | PROC-042 v1 e v2 |
| Informa prazos de entrega para frete especial | Logística e Frete | PROC-042 v1 e v2 |
| Explica os tiers de cliente (Gold, Silver, Standard) e seus SLAs | SLA e Contratos | SLA-2024 |
| Informa critérios de incidente crítico e penalidades por SLA | SLA e Contratos | SLA-2024 |
| Esclarece dúvidas frequentes do time de atendimento | Atendimento ao Cliente | FAQ (com ressalva de fonte informal) |
| Sinaliza contradições entre documentos e indica a versão prioritária | Gestão Documental | Metadado de vigência |
| Informa quando não encontrou resposta (sem inventar informações) | Todos | — |
| Indica a fonte de cada resposta (documento + seção) | Todos | — |

### O que o Assistente NÃO FAZ

| Limitação | Motivo | Encaminhamento correto |
|-----------|--------|----------------------|
| Não calcula fretes em tempo real | Depende de tabela mensal externa (`frete-base-AAAAMM.xlsx`) não indexada | Consultar o sistema de cotação ou tabelas do setor Comercial |
| Não devolve cargas perigosas pelo processo padrão | POL-001 é explícita: não elegível | Ramal 4500 (Gestão de Riscos) |
| Não autoriza descontos ou negociações | Atendente não tem autonomia; regras de desconto passam pelo Comercial | Encaminhar ao Comercial com justificativa |
| Não processa chamados de carga danificada em trânsito | Processo passa pelo Jurídico, não pelo atendimento padrão | sinistros@novatech.com.br |
| Não consulta o sistema de tracking em tempo real | Sem integração com sistema de rastreamento | Portal do Cliente ou sistema de chamados |
| Não abre chamados no Portal do Cliente | Apenas orienta o atendente | Portal: portal.novatech.com.br |
| Não faz exceções fora dos documentos indexados | Guardrail: nunca inventar ou extrapolar | Escalar para supervisor |
| Não responde sobre frete padrão (abaixo de 500 kg) | Gap documental: não há PROC indexada para essa modalidade | Consultar tabela de fretes padrão com o Comercial |
| Não confirma seguro de carga com precisão | Apenas FAQ informal menciona o produto; sem documento normativo indexado | Confirmar com o Comercial |
| Não trata carga com lacre violado sem documentação | Exceção fora do processo padrão | Seguir POL-001 seção 3.2 e acionar Gestão de Riscos |

### Como o Assistente se Relaciona com os Outros Contextos

```
[Atendente] 
    │ pergunta em linguagem natural
    ▼
[Copiloto de Atendimento] ──── consome ──── [Base de Conhecimento]
    │                                              │
    │                                    (chunks + metadados de vigência)
    │                                              │
    │                              ┌───────────────┼────────────────┐
    │                         [POL-001]       [PROC-042]       [SLA-2024]
    │                         Política de     Frete            Contratos
    │                         Devolução       Especial         e SLA
    │
    │ retorna resposta + fonte
    ▼
[Atendente]
    │ escala quando necessário
    ▼
[Gestão de Riscos] / [Comercial] / [Jurídico]
(fora do escopo do assistente)
```

---

## 6. Relações entre os Contextos

O assistente NovaTech opera como uma **camada de mediação** entre o conhecimento documental da empresa e o time de atendimento. O contexto **Gestão Documental** é o ponto central: todos os demais contextos de negócio (Logística e Frete, SLA e Contratos) existem como documentação indexada nessa base, e o **Copiloto de Atendimento** é o motor que converte perguntas em buscas semânticas sobre essa base.

A fronteira mais crítica está na relação entre **Gestão Documental** e **Logística e Frete**, especificamente na coexistência de duas versões ativas do PROC-042 com valores divergentes. Essa contradição não foi resolvida internamente pela NovaTech (o PROC-042 v1 não foi arquivado), o que exige que o assistente sinalize a ambiguidade e priorize sempre a versão mais recente, informando o atendente da existência de versão anterior.

A segunda fronteira crítica é entre **Atendimento ao Cliente** e **Logística e Frete** no tratamento de cargas perigosas: o FAQ informal sugere a possibilidade de exceções que a política normativa (POL-001) não reconhece. O assistente deve seguir o documento normativo e nunca afirmar que a devolução de carga perigosa é possível pelo processo padrão.

Por fim, o contexto **SLA e Contratos** toca o **Atendimento ao Cliente** na triagem de chamados: o tier do cliente (Gold, Silver, Standard) determina os prazos que o time de atendimento deve cumprir, e o assistente deve ser capaz de responder sobre essa classificação sem inventar tiers inexistentes.

O **Copiloto de Atendimento** é, por design, um contexto de *somente leitura* sobre o domínio de negócio — ele nunca escreve, decide ou executa ações operacionais. Quando a resposta requer ação (abertura de chamado, escalação, negociação), o assistente orienta o atendente sobre o encaminhamento correto e encerra sua responsabilidade ali.

---

## 7. Glossário de Linguagem Ubíqua

| Termo | Definição no domínio NovaTech | Nunca confundir com |
|-------|-------------------------------|---------------------|
| **Carga perigosa** | Cargas classificadas nas classes 1 a 6 da ANTT (Resolução ANTT nº 5.947/2021): explosivos, gases, líquidos inflamáveis, sólidos inflamáveis, oxidantes/peróxidos, substâncias tóxicas | Carga de alto valor ou frágil |
| **Frete especial** | Modalidade de frete para cargas acima de 500 kg, com cálculo por multiplicador regional e fator de peso | Frete expresso ou prioritário |
| **CT-e** | Conhecimento de Transporte Eletrônico — documento fiscal obrigatório para registro de devolução | Nota fiscal do produto |
| **Gold / Silver / Standard** | Tiers de cliente da NovaTech — classificação por volume e valor de contrato. Não existe tier "Platinum" ou outro | Categorias de fidelidade de varejo |
| **SLA de resposta** | Tempo até o primeiro retorno ao cliente (pode ser "estamos verificando") | SLA de resolução (que é o fechamento do chamado) |
| **SLA de resolução** | Tempo até o fechamento efetivo do chamado | SLA de resposta |
| **Incidente crítico** | Chamado que atende ao menos um critério da seção 3 do SLA-2024 (carga de alto valor sem rastreio, carga perigosa irregular, múltiplos chamados, risco à segurança) | Chamado urgente por julgamento do atendente |
| **Coleta reversa** | Operação de retirada da mercadoria devolvida no endereço do cliente | Troca ou substituição de produto |
| **Cadeia de frio** | Controle contínuo de temperatura para cargas refrigeradas; ruptura invalida a elegibilidade de devolução | Embalagem térmica |
| **Vigência do documento** | Indicador que determina qual versão de um documento está ativa e deve ser priorizada | Data de emissão (um documento mais antigo pode ainda estar vigente) |
| **Chunk** | Fragmento de documento processado para busca semântica no índice RAG | Parágrafo ou seção completa |
| **Guardrail** | Regra de comportamento obrigatória do assistente (ex: nunca inventar prazos) | Sugestão ou boa prática |
| **Fonte normativa** | Documentos POL, PROC e SLA — validados por área responsável e Compliance | FAQ — documento informal, sem validação formal |
| **Multiplicador regional** | Fator aplicado ao valor base do frete conforme região de destino | Desconto ou acréscimo negociado |
| **Fator de peso** | Coeficiente aplicado à faixa de peso da carga no cálculo de frete especial | Taxa de volume ou dimensão |
