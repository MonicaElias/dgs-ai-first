# Classificação de Enforcement dos Guardrails — Assistente NovaTech
**Versão:** 1.0  
**Data:** 13/06/2026  
**Responsável:** Product Specialist  
**Referência:** guardrails-completo.md v1.0

---

## Conceitos aplicados

| Mecanismo | Natureza | Quando usar |
|-----------|----------|-------------|
| **Prompt** | Probabilístico — o modelo tende a seguir, sem garantia absoluta | Comportamentos interpretativos, de tom, formato ou raciocínio contextual que exigem julgamento |
| **Código** | Determinístico — garantido por lógica fora do modelo | Verificações binárias, bloqueios por categoria, filtragem de versão de documento, detecção de padrão léxico fixo |
| **Híbrido** | Prompt + Código — camadas complementares | Quando o código garante o gatilho/verificação estrutural e o prompt garante a qualidade da resposta dentro do comportamento correto |

---

## Tabela resumo de classificação

| ID | Guardrail (resumo) | Classificação | Justificativa |
|----|-------------------|---------------|---------------|
| GR-001 | Citar fonte com código, versão e seção | **Híbrido** | O prompt instrui o formato; código pós-processamento valida presença da citação na resposta gerada |
| GR-002 | Tratar carga perigosa como exceção em devoluções | **Híbrido** | Detecção léxica de "carga perigosa" + "devolução" via código; resposta correta garantida pelo prompt |
| GR-003 | Usar PROC-042 v2 para chamados após 01/12/2023 | **Híbrido** | Código controla qual versão do documento é indexada/recuperada no RAG por data do chamado; prompt reforça identificação explícita da versão usada |
| GR-004 | Busca exaustiva antes de declarar ausência | **Código** | A garantia de varredura de todos os documentos é estrutural, implementada no pipeline RAG — não pode depender do modelo |
| GR-005 | Confirmar tier antes de informar SLAs | **Híbrido** | Código detecta query sobre SLA sem tier identificado no contexto; prompt instrui a solicitação de confirmação e o formato da resposta |
| GR-006 | Responder em português formal com linguagem do domínio | **Prompt** | Tom, formalidade e vocabulário são comportamentos interpretativos sem verificação binária confiável via código |
| GR-007 | NÃO afirmar elegibilidade de devolução para carga perigosa | **Híbrido** | Código detecta combinação crítica de termos e injeta instrução obrigatória; prompt garante a resposta correta com encaminhamento ao ramal 4500 |
| GR-008 | NÃO usar multiplicadores PROC-042 v1 para chamados novos | **Código** | A não exposição da v1 ao modelo é garantida por filtragem no índice RAG — o modelo não pode corrigir o que não vê |
| GR-009 | NÃO confirmar tiers inexistentes (Platinum, Premium) | **Híbrido** | Código detecta tier name fora do conjunto {Gold, Silver, Standard} na query; prompt instrui a resposta corretiva |
| GR-010 | NÃO citar FAQ-Atendimento como fonte oficial | **Híbrido** | Código injeta metadado "informal" em todos os chunks do FAQ no índice RAG e aciona disclaimer automático; prompt instrui o formato da resposta com ressalva |
| GR-011 | NÃO sugerir autonomia de desconto ao atendente | **Híbrido** | Código detecta termos de desconto/negociação na query; prompt instrui a resposta de redirecionamento ao Comercial |
| GR-012 | NÃO informar valor/prazo sem versão do documento | **Híbrido** | Código valida pós-geração se valores numéricos críticos estão acompanhados de identificador de versão (regex); prompt instrui o formato correto |
| GR-013 | Conflito PROC-042 v1 vs. v2 sem data do chamado | **Híbrido** | Código detecta query de frete especial sem data do chamado no contexto; prompt instrui apresentação das duas versões e solicitação da data |
| GR-014 | Fonte única é o FAQ informal | **Híbrido** | Código identifica quando todos os documentos recuperados no RAG são do FAQ-Atendimento e injeta flag de aviso; prompt instrui o formato de resposta com ressalva explícita |
| GR-015 | Pergunta cruza dois domínios sem cobertura completa | **Prompt** | Identificar cruzamento de domínios é interpretativo — requer julgamento contextual do modelo; código não consegue detectar combinações semânticas abertas com confiabilidade |
| GR-016 | Carga acima de 5.000kg requer aprovação prévia | **Híbrido** | Código extrai peso mencionado na query (regex/NER); se > 5.000kg, injeta alerta obrigatório de aprovação; prompt instrui a resposta completa com referência à PROC-042 |
| GR-017 | Ausência de informação após busca exaustiva | **Código** | A garantia de que todos os documentos foram consultados antes do "não encontrei" é estrutural no pipeline RAG — o modelo não tem como saber o que o retriever não retornou |

---

## Detalhamento técnico — Guardrails com componente de Código ou Híbrido

---

### GR-001 — Citar fonte com código, versão e seção | **Híbrido**

**O que o prompt cobre:**
```
System prompt — instrução de formato:
"Toda resposta que contenha prazo, valor monetário, multiplicador, percentual 
ou regra de procedimento DEVE incluir, ao final do trecho, a citação no formato: 
Fonte: [CÓDIGO-DOC] v[VERSÃO], seção [X.Y]. 
Exemplo: Fonte: POL-001 v3.1, seção 3.1. 
Nunca omita a versão do documento. Se não houver versão disponível no contexto 
recuperado, sinalize: [versão não identificada no documento recuperado]."
```

**O que o código cobre — pós-processamento:**
- **Camada:** Pós-geração (intercepta a resposta antes de entregar ao atendente)
- **Lógica:** Regex scan na resposta gerada. Se a resposta contiver dígitos seguidos de unidades (dias, h, kg, R$, %) ou palavras como "multiplicador", "fator de peso", "prazo", verificar presença de padrão `(POL|PROC|SLA)-\d+ v\d+\.\d+, seção \d+`.
- **Ação se falhar:** Retornar aviso ao modelo com instrução de reescrita incluindo a citação, ou sinalizar ao atendente que a resposta não passou na validação de fonte.

---

### GR-002 — Tratar carga perigosa como exceção em devoluções | **Híbrido**

**O que o código cobre — pré-processamento:**
- **Camada:** Análise da query antes do envio ao modelo
- **Lógica:** Detectar co-ocorrência de termos de carga perigosa (`carga perigosa`, `classe ANTT`, `explosivos`, `gases`, `inflamáveis`, `tóxic`, `infectant`) com termos de devolução (`devolução`, `devolver`, `retorno`, `coleta reversa`, `prazo de devolução`, `elegível`).
- **Ação se detectado:** Injetar no contexto enviado ao modelo o bloco fixo:
```
[REGRA OBRIGATÓRIA — CARGA PERIGOSA]: Cargas perigosas (classes 1-6 ANTT, 
POL-001 v3.1 seção 3.2) NÃO são elegíveis para devolução pelo processo padrão. 
Encaminhar ao ramal 4500. Esta regra tem prioridade sobre qualquer outra informação 
recuperada sobre prazos de devolução.
```

**O que o prompt cobre:**
```
System prompt — instrução de domínio:
"Cargas perigosas classificadas nas classes 1 a 6 da ANTT nunca seguem o processo 
padrão de devolução descrito na POL-001 seção 3.1. A seção 3.1 (prazo de 7 dias úteis) 
se aplica APENAS a cargas não listadas na seção 3.2. Quando a pergunta envolver 
devolução de carga perigosa, responda com a exceção da seção 3.2 e direcione ao 
ramal 4500 (Gestão de Riscos)."
```

---

### GR-003 — Usar PROC-042 v2 para chamados após 01/12/2023 | **Híbrido**

**O que o código cobre — camada de dados / RAG:**
- **Camada:** Configuração do índice de recuperação (RAG)
- **Lógica de filtragem:**
  1. Extrair data do chamado do contexto da conversa (campo estruturado do sistema de chamados, se disponível, ou extração via regex da query).
  2. Se `data_chamado >= 2023-12-01`: filtrar o índice RAG para retornar **apenas chunks da PROC-042 v2.0**. A v1.0 não é exposta ao modelo.
  3. Se `data_chamado < 2023-12-01` (chamados em processamento): retornar chunks da PROC-042 v1.0 e injetar metadado de contexto: `[CHAMADO PRÉ-TRANSIÇÃO — usar multiplicadores v1]`.
  4. Se data não identificada: ativar o fallback do GR-013 (apresentar ambas as versões).

**O que o prompt cobre:**
```
System prompt — instrução de versionamento:
"Ao responder sobre frete especial, sempre identifique explicitamente qual versão 
da PROC-042 está sendo usada (v1.0 ou v2.0) e a data de corte que determina a versão 
aplicável (01/12/2023, conforme seção 5 da PROC-042 v2.0). Nunca cite apenas 'PROC-042' 
sem especificar a versão."
```

---

### GR-004 — Busca exaustiva antes de declarar ausência | **Código**

**Implementação exclusivamente em código — pipeline RAG:**
- **Camada:** Orquestrador do pipeline de recuperação
- **Lógica:**
  1. Para cada query, executar retrieval em **paralelo** nos 5 índices: POL-001, PROC-042-v1, PROC-042-v2, SLA-2024, FAQ-Atendimento.
  2. Consolidar os resultados. O modelo só recebe a instrução de "resposta vazia" se **todos os 5 retrievals** retornarem score abaixo do threshold de relevância configurado.
  3. Junto com a instrução de "nenhum documento relevante encontrado", passar ao modelo a lista dos 5 documentos consultados e o tema identificado na query para sugestão de área de escalação.
  4. **Proibir** que o modelo gere "não encontrei informação" sem receber essa flag do orquestrador.

```
# Pseudocódigo do orquestrador
results = await parallel_retrieve([
    index_pol001, index_proc042v1, index_proc042v2, 
    index_sla2024, index_faq
], query=user_query, threshold=0.7)

relevant = [r for r in results if r.score >= threshold]

if not relevant:
    context = build_not_found_context(
        docs_consulted=["POL-001 v3.1", "PROC-042 v1.0", "PROC-042 v2.0", 
                        "SLA-2024 v2024.1", "FAQ-Atendimento"],
        query_topic=classify_topic(user_query)
    )
    return send_to_model(context, instruction=NOT_FOUND_TEMPLATE)
```

---

### GR-005 — Confirmar tier antes de informar SLAs | **Híbrido**

**O que o código cobre — pré-processamento:**
- **Camada:** Análise da query e do contexto da conversa
- **Lógica:** Verificar se a query menciona SLA, prazo de atendimento, tempo de resposta, penalidade, ou gerente de conta **sem** que o tier do cliente tenha sido estabelecido no contexto da conversa (histórico da sessão ou campo de tier do sistema de chamados).
- **Ação se tier ausente:** Injetar no prompt: `[TIER NÃO IDENTIFICADO]: antes de responder sobre SLAs, solicite o tier do cliente (Gold, Silver ou Standard). Não forneça valores de SLA sem tier estabelecido.`

**O que o prompt cobre:**
```
System prompt — instrução de SLA:
"Nunca cite prazos de SLA (primeira resposta, resolução, incidentes críticos) sem 
antes identificar o tier do cliente (Gold, Silver ou Standard). Tiers existentes: 
Gold (contrato > R$500k/ano ou > 200 ops/mês), Silver (R$100k-500k/ano ou 50-200 
ops/mês), Standard (demais). Não existe tier Platinum, Premium, VIP ou similar."
```

---

### GR-007 — NÃO afirmar elegibilidade de devolução para carga perigosa | **Híbrido**

> Este é o guardrail de maior criticidade operacional e regulatória (INCIDENTE-01). Requer dupla camada.

**O que o código cobre — pré E pós-processamento:**

*Pré-processamento (bloqueio preventivo):*
- Detectar co-ocorrência de `(carga perigosa | classe [1-6] ANTT | explosivo | gás | inflamável | tóxico | infectante)` com `(devolução | devolver | prazo | retorno | elegível | coleta reversa)`.
- Injetar no contexto do modelo o bloco obrigatório de GR-002 (ver acima).

*Pós-processamento (validação da resposta):*
- Scan da resposta gerada: se contiver `(carga perigosa | [Cc]argo perigosa)` + qualquer número de dias + `(devolução | devolv)`, sinalizar como resposta inválida.
- Bloquear entrega ao atendente e reenviar ao modelo com flag de correção.
- Registrar ocorrência no log de violações para auditoria.

**O que o prompt cobre:**
```
System prompt — regra crítica:
"REGRA DE SEGURANÇA CRÍTICA: Nunca afirme que carga perigosa (classes 1-6 ANTT) 
pode ser devolvida pelo processo padrão. O prazo de 7 dias úteis da POL-001 seção 3.1 
NÃO se aplica a cargas perigosas. A resposta correta sempre é: não elegível pelo 
processo padrão + encaminhar ao ramal 4500 (Gestão de Riscos). 
Fonte obrigatória: POL-001 v3.1, seção 3.2."
```

---

### GR-008 — NÃO usar multiplicadores PROC-042 v1 para chamados novos | **Código**

**Implementação exclusivamente em código — índice RAG:**
- **Camada:** Configuração e filtragem do índice de recuperação
- **Lógica:**
  1. Ao indexar os documentos, criar dois índices separados: `proc042_v1` e `proc042_v2`.
  2. No momento do retrieval, aplicar filtro por data do chamado (ver GR-003).
  3. Para chamados após 01/12/2023: **desabilitar** o índice `proc042_v1` para esta query. Os chunks da v1 não são recuperados nem expostos ao modelo.
  4. Para chamados antes de 01/12/2023 ainda em processamento: habilitar apenas `proc042_v1` e injetar metadado `[CHAMADO PRÉ-TRANSIÇÃO]`.
  5. Para data desconhecida: habilitar ambos os índices com metadados de versão diferenciados (ativa GR-013).

```
# Configuração do filtro no retriever
def get_proc042_filter(ticket_date: date | None) -> dict:
    if ticket_date is None:
        return {"include": ["proc042_v1", "proc042_v2"]}  # GR-013 fallback
    elif ticket_date >= date(2023, 12, 1):
        return {"include": ["proc042_v2"], "exclude": ["proc042_v1"]}
    else:
        return {"include": ["proc042_v1"], "exclude": ["proc042_v2"]}
```

---

### GR-009 — NÃO confirmar tiers inexistentes | **Híbrido**

**O que o código cobre — pré-processamento:**
- **Camada:** Análise léxica da query
- **Lógica:** Detectar menção a tier names fora do conjunto `{Gold, Silver, Standard}`. Lista de termos a monitorar: `Platinum`, `Premium`, `VIP`, `Diamond`, `Elite`, `Bronze`, `básico` (quando usado como tier name).
- **Ação:** Injetar no contexto: `[TIER INVÁLIDO DETECTADO — "{termo}"]: informar ao atendente que este tier não existe na NovaTech. Tiers válidos: Gold, Silver, Standard (SLA-2024 v2024.1, seção 1).`

**O que o prompt cobre:**
```
System prompt — instrução de tiers:
"Os únicos tiers de cliente NovaTech são: Gold, Silver e Standard, conforme SLA-2024 
v2024.1 seção 1. Nunca confirme a existência de outros tiers. Se o cliente mencionar 
'Platinum' ou similar, informe que não existe e oriente a verificar o número do 
contrato para identificar o tier correto."
```

---

### GR-010 — NÃO citar FAQ-Atendimento como fonte oficial | **Híbrido**

**O que o código cobre — camada de dados:**
- **Camada:** Indexação e metadados RAG
- **Lógica:**
  1. Ao indexar o FAQ-Atendimento, adicionar metadado `document_type: "informal"` em todos os chunks.
  2. No retrieval, verificar se os documentos recuperados são todos `document_type: "informal"`.
  3. Se sim, injetar automaticamente no contexto: `[FONTE INFORMAL]: os documentos recuperados são do FAQ-Atendimento (não validado por Compliance). A resposta deve incluir ressalva explícita sobre o caráter informal da fonte.`
  4. Se mistura de formais e informais, injetar: `[FONTE MISTA]: parte da informação vem do FAQ informal. Distinguir na resposta o que é normativo (POL/PROC/SLA) do que é prática informal (FAQ).`

**O que o prompt cobre:**
```
System prompt — hierarquia de fontes:
"Hierarquia de documentos NovaTech:
1. DOCUMENTOS NORMATIVOS (alta confiança): POL-001, PROC-042, SLA-2024 — validados 
   por Compliance ou Diretoria. Citar como fonte oficial.
2. FAQ-ATENDIMENTO (baixa confiança): documento informal, sem responsável formal, 
   não validado. SEMPRE incluir a ressalva: 'esta informação está disponível apenas 
   no FAQ-Atendimento, documento informal não validado por Compliance. Confirme com 
   a área responsável antes de repassar ao cliente.'"
```

---

### GR-011 — NÃO sugerir autonomia de desconto ao atendente | **Híbrido**

**O que o código cobre — pré-processamento:**
- **Camada:** Análise da query
- **Lógica:** Detectar termos de desconto/negociação: `desconto`, `redução`, `abatimento`, `negociar`, `exceção tarifária`, `percentual menor`, `preço menor`.
- **Ação:** Injetar no contexto: `[DESCONTO SOLICITADO]: atendentes NÃO têm autonomia para conceder desconto. Descontos automáticos por volume: PROC-042 v2.0 seção 4 (8+ fretes/mês: 5%; 15+ fretes/mês: 10%). Outros casos: encaminhar ao Comercial.`

**O que o prompt cobre:**
```
System prompt — instrução de descontos:
"O atendente não tem autonomia para conceder descontos. Os únicos descontos aplicáveis 
são os automáticos por volume de frete especial (PROC-042 v2.0, seção 4). Para qualquer 
outro pedido de desconto, a resposta é: encaminhar ao Comercial com justificativa. 
Nunca sugira valores de desconto fora da tabela PROC-042 v2."
```

---

### GR-012 — NÃO informar valor/prazo sem versão do documento | **Híbrido**

**O que o código cobre — pós-processamento:**
- **Camada:** Validação da resposta gerada
- **Lógica (regex):** Detectar padrões de valores críticos sem identificador de versão associado:
  - Multiplicadores sem versão: `[0-9]\.[0-9]` precedido ou seguido de "multiplicador" ou "fator de peso" sem `v[0-9]+\.[0-9]+` no contexto próximo (janela de 50 caracteres).
  - Prazos sem versão: `\d+ dias úteis` ou `\d+h úteis` sem referência `v[0-9]+\.[0-9]+`.
- **Ação se detectado:** Retornar ao modelo com instrução: "Sua resposta contém valores numéricos sem identificação de versão do documento. Reescreva incluindo a versão para cada valor citado."

**O que o prompt cobre:**
```
System prompt — formato obrigatório para valores:
"Sempre que citar um valor numérico (multiplicador, fator de peso, prazo em dias/horas, 
percentual de SLA ou penalidade), o formato obrigatório é:
[VALOR] ([CÓDIGO-DOC] [VERSÃO], seção [X.Y])
Exemplo: multiplicador Norte: 1.8 (PROC-042 v2.0, seção 2.1)
Nunca cite apenas o código do documento sem a versão."
```

---

### GR-013 — Conflito PROC-042 v1 vs. v2 sem data do chamado | **Híbrido**

**O que o código cobre — pré-processamento:**
- **Camada:** Orquestrador da sessão
- **Lógica:**
  1. Verificar se a query envolve frete especial (termos: `frete especial`, `multiplicador`, `fator de peso`, `acima de 500kg`, `carga pesada`).
  2. Verificar se a data do chamado está disponível no contexto da sessão (campo do sistema de chamados ou extração da query).
  3. Se ausente: ativar modo "versão ambígua" — recuperar chunks de **ambas** as versões e injetar instrução: `[DATA DO CHAMADO AUSENTE]: apresentar ambas as versões e solicitar a data antes de calcular.`

**O que o prompt cobre:**
```
System prompt — instrução de ambiguidade de versão:
"Quando receber a flag [DATA DO CHAMADO AUSENTE], apresente uma tabela comparativa 
com os multiplicadores das duas versões (v1.0 e v2.0) e solicite explicitamente a 
data de abertura do chamado antes de fornecer o cálculo. Nunca escolha uma versão 
arbitrariamente quando a data não estiver disponível."
```

---

### GR-014 — Fonte única é o FAQ informal | **Híbrido**

**O que o código cobre — pipeline RAG:**
- **Camada:** Pós-retrieval, pré-geração
- **Lógica:**
  1. Após o retrieval, verificar o `document_type` de todos os documentos recuperados com score acima do threshold.
  2. Se **todos** são `document_type: "informal"` (FAQ-Atendimento): injetar flag `[SOMENTE FAQ INFORMAL]` no contexto.
  3. Junto com a flag, injetar o texto de aviso padronizado: `"Esta resposta tem como única fonte o FAQ-Atendimento, documento informal não validado por Compliance. Inclua a ressalva e recomende confirmação com a área competente."`

**O que o prompt cobre:**
```
System prompt — instrução de fonte única informal:
"Quando receber a flag [SOMENTE FAQ INFORMAL], sua resposta deve:
1. Identificar a fonte como FAQ-Atendimento (documento informal)
2. Fornecer a informação disponível com a ressalva explícita
3. Recomendar confirmação com a área competente (Comercial, Compliance ou Operações)
4. Não apresentar a informação como política oficial da NovaTech"
```

---

### GR-016 — Carga acima de 5.000kg requer aprovação prévia | **Híbrido**

**O que o código cobre — pré-processamento:**
- **Camada:** Extração de entidades da query
- **Lógica:**
  1. Aplicar regex/NER para extrair valores de peso mencionados na query (padrões: `\d+\.?\d*\s*kg`, `\d+\.?\d*\s*toneladas`, `\d+\.?\d*\s*t`).
  2. Converter para kg. Se `peso_extraido > 5000`:
  3. Injetar no contexto: `[APROVAÇÃO OBRIGATÓRIA — CARGA > 5.000kg]: esta operação requer aprovação prévia do gerente de operações regional (PROC-042 v2.0, seção 4). Sinalizar antes de qualquer cálculo.`

**O que o prompt cobre:**
```
System prompt — instrução para cargas pesadas:
"Quando receber a flag [APROVAÇÃO OBRIGATÓRIA — CARGA > 5.000kg], sempre inicie 
a resposta com o aviso de aprovação obrigatória antes de qualquer cálculo estimado. 
Deixe claro que o valor calculado é apenas uma referência e que o frete não pode 
ser confirmado ao cliente sem a aprovação do gerente de operações regional."
```

---

### GR-017 — Ausência de informação após busca exaustiva | **Código**

**Implementação exclusivamente em código — orquestrador RAG:**
- **Camada:** Orquestrador do pipeline de recuperação (mesma lógica do GR-004, com extensão para o template de resposta)
- **Lógica:**
  1. Garantir varredura dos 5 índices (ver GR-004).
  2. Se nenhum resultado relevante: classificar o tema da query usando um classificador leve (mapeamento para áreas: Operações → PROC; Compliance → POL; Comercial → SLA/PROC-042; Atendimento → FAQ).
  3. Montar o template de resposta estruturada:
```
[RESPOSTA NÃO ENCONTRADA]
Documentos consultados: POL-001 v3.1, PROC-042 v1.0, PROC-042 v2.0, 
SLA-2024 v2024.1, FAQ-Atendimento.
Tema identificado: {topic}
Área mais indicada para escalação: {area}
Recurso externo possível: {external_resource_if_known}
```
  4. Enviar este template ao modelo com instrução de formatar a resposta final ao atendente sem inventar informações adicionais.

```
# Pseudocódigo do template de ausência
def build_not_found_response(query_topic: str) -> str:
    area_map = {
        "frete_padrao": ("Comercial", "\\\\novatech-fs\\comercial\\tabelas\\"),
        "seguro": ("Comercial", None),
        "carga_perigosa_riscos": ("Gestão de Riscos", "ramal 4500"),
        "carga_danificada": ("Jurídico", "sinistros@novatech.com.br"),
    }
    area, resource = area_map.get(query_topic, ("Supervisor", None))
    return NOT_FOUND_TEMPLATE.format(area=area, resource=resource)
```

---

## Visão arquitetural das camadas de enforcement

```
┌─────────────────────────────────────────────────────────────┐
│                     QUERY DO ATENDENTE                      │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│              PRÉ-PROCESSAMENTO (Código)                     │
│  GR-002 · GR-005 · GR-007 · GR-009 · GR-011                │
│  GR-013 · GR-016                                            │
│  → Detecção léxica · Extração de entidades (peso, data)     │
│  → Injeção de flags e blocos de contexto obrigatório        │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│              PIPELINE RAG (Código)                          │
│  GR-003 · GR-004 · GR-008 · GR-010 · GR-014 · GR-017       │
│  → Filtragem de versão por data do chamado                  │
│  → Varredura obrigatória dos 5 índices                      │
│  → Metadados document_type (formal/informal)                │
│  → Injeção de metadados no contexto do modelo               │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│              MODELO LLM + SYSTEM PROMPT                     │
│  GR-001 · GR-002 · GR-003 · GR-005 · GR-006 · GR-007       │
│  GR-009 · GR-010 · GR-011 · GR-012 · GR-013 · GR-014       │
│  GR-015 · GR-016                                            │
│  → Instrução de formato · Linguagem · Regras de domínio     │
│  → Comportamentos de fallback · Cruzamento de domínios      │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│              PÓS-PROCESSAMENTO (Código)                     │
│  GR-001 · GR-007 · GR-012                                   │
│  → Validação de presença de citação (regex)                 │
│  → Validação de valores numéricos com versão                │
│  → Bloqueio de resposta inválida (carga perigosa + prazo)   │
│  → Log de violações para auditoria                          │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                  RESPOSTA AO ATENDENTE                      │
└─────────────────────────────────────────────────────────────┘
```

---

*Documento elaborado com base nos guardrails-completo.md v1.0. Os trechos de system prompt são sugestões de implementação — devem ser integrados ao system prompt consolidado do assistente NovaTech e testados em conjunto com os mecanismos de código descritos.*
