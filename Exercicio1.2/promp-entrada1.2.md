# Registro de Conversa — Entrada 1.2

**Projeto:** DGS IA First — NovaTech
**Data:** 06/06/2026
**Arquivo gerado:** jornada-atendente-assistente-ia.md

---

## Documentos Fornecidos como Contexto

### Documento 1 — `entrega-final.md`
**Tipo:** Mapa de temas e hipóteses de gaps
**Localização:** `Exercicio1.1/entrega-final.md`

Conteúdo sintetizado:
- **Seção 1:** Dados de entrada — 5 documentos analisados (FAQ, POL-001, PROC-042 v1, PROC-042 v2, SLA-2024)
- **Seção 2:** Mapa de temas cobertos — 24 temas mapeados com status (coberto, conflito, gap)
- **Seção 3:** Conflitos e sobreposições — 5 conflitos identificados entre documentos
  - Conflito 1: Multiplicadores regionais (v1 × v2)
  - Conflito 2: Prazo adicional de entrega (v1 × v2)
  - Conflito 3: Descontos de volume (v1 × v2)
  - Conflito 4: Devolução de cargas perigosas (POL-001 × FAQ)
  - Conflito 5: Autonomia para desconto (FAQ × PROC-042 v2)
- **Seção 4:** Hipóteses de gaps — 8 gaps identificados (G1 a G8)
- **Seção 5:** Resumo executivo

---

### Documento 2 — `analise-de-inconsistencia-proc042.md`
**Tipo:** Análise comparativa de inconsistências
**Localização:** `Exercicio1.1/analise-de-inconsistencia-proc042.md`

Conteúdo sintetizado:
- Documentos analisados: PROC-042 v1 (03/03/2023) e PROC-042 v2 (10/11/2023)
- **6 inconsistências identificadas:**
  - INC-001: Fatores de peso divergentes (faixas média e alta)
  - INC-002: Multiplicadores regionais divergentes em todas as regiões (Norte: 1,6 vs. 1,8)
  - INC-003: Prazo adicional de entrega divergente (+2 vs. +3 dias úteis)
  - INC-004: Política de desconto de volume contraditória (gatilho, percentual e alçada incompatíveis)
  - INC-005: Ausência de hierarquia formal entre documentos (ambos coexistem sem revogação)
  - INC-006: Status da PROC-043 referenciada de forma divergente
- **Recomendações:** 6 ações formais para resolução
- **Matriz compilada:** tabela com ID, tipo, tema, análise e fator de risco por inconsistência

---

### Documento 3 — `cruzamento-inconsistencia-vs-FAQ.md`
**Tipo:** Análise integrada — inconsistências × FAQ informal
**Localização:** `Exercicio1.1/cruzamento-inconsistencia-vs-FAQ.md`

Conteúdo sintetizado:
- **7 divergências identificadas entre FAQ e documentos normativos:**
  - DIV-001: Devolução de carga perigosa — FAQ contradiz POL-001
  - DIV-002: Desconto de volume — FAQ cria orientação híbrida inexistente nos documentos formais
  - DIV-003: Versão vigente da PROC-042 — FAQ orienta sem autoridade formal
  - DIV-004: Seguro de carga — FAQ define percentuais (0,3% / 0,8%) sem respaldo normativo
  - DIV-005: Carga danificada em trânsito — FAQ define processo sem documento normativo
  - DIV-006: SLA de tracking por rota — FAQ define critérios sem respaldo no SLA-2024
  - DIV-007: Frete expresso para carga perigosa — FAQ descreve fluxo sem documento normativo
- **4 riscos principais:** jurídico/contratual, financeiro/comercial, operacional/reputacional, regulatório
- **8 dúvidas para levantamento com o cliente**

---

### Documento 4 — `FAQ-atendimento-resumido.md`
**Tipo:** Documento colaborativo informal (não validado)
**Localização:** `Exercicio1.1/FAQ-atendimento-resumido.md`

Conteúdo sintetizado:
- Mantido informalmente pelo time de atendimento da NovaTech
- Cobre: devoluções, cargas perigosas, frete especial, tiers de cliente, seguro de carga, SLAs, cargas danificadas, autonomia do atendente
- **Classificação:** documento informal — NÃO validado por Compliance ou Operações
- **Responsável:** nenhum responsável formal

---

## Prompt Principal Fornecido

```
Sou um Product Specialist e estou conduzindo uma fase de intenção e descoberta de um novo
projeto e preciso com base no Discovery inicial mapear a jornada do atendente usando um
assistente de IA.

Objetivo: Gerar uma jornada estruturada e orientada para a operação real do atendimento.

Contexto do cenário atual:

A NovaTech é uma empresa de médio porte do setor de logística com 1.200 funcionários.
Sua operação depende de um conjunto extenso de documentação interna: manuais de
procedimento operacional, políticas de compliance, tabelas de SLA por tipo de cliente,
regras de cálculo de frete, e normas de segurança de carga.

Hoje, essa documentação está espalhada em três fontes: um SharePoint corporativo com ~800
documentos (PDFs e Word), uma wiki interna no Confluence com ~400 páginas, e uma pasta de
rede com planilhas de referência atualizadas mensalmente.

O problema: a equipe de atendimento ao cliente (45 pessoas) gasta em média 12 minutos por
chamado buscando informações nessas fontes para responder dúvidas de clientes sobre prazos,
regras de frete, políticas de devolução e procedimentos de reclamação. Isso gera atrasos,
respostas inconsistentes e frustração tanto dos atendentes quanto dos clientes.

O volume médio é de 320 chamados/dia, dos quais 60% envolvem consulta a documentação.
A documentação é atualizada mensalmente por 3 áreas diferentes (Operações, Compliance,
Comercial), sem processo unificado de revisão.
Alguns documentos se contradizem entre versões — a equipe de atendimento hoje resolve isso
"perguntando para quem sabe".
A expectativa da diretoria é reduzir o tempo médio de busca de 12 para menos de 2 minutos
por chamado.

Dados do discovery:
- Os atendentes hoje abrem em média 4 fontes diferentes por chamado.
- As dúvidas mais comuns são sobre prazos de entrega (35%), regras de frete (25%),
  política de devolução (20%) e outros (20%).
- Em 15% dos casos, o atendente não encontra resposta e escala para o supervisor.

Instruções:

- Considere estes riscos e restrições disponíveis nos documentos:
  entrega-final.md
  analise-de-inconsistencia-proc042.md
  FAQ-atendimento.md
  cruzamento-inconsistencia-vs-FAQ.md

- Considere o documento `cruzamento-inconsistencia-vs-FAQ.md` como insumo para o fluxo
  principal, fluxo de fallback, fluxo de feedback e guardrails.

- Criar a jornada do atendente em formato texto com os dados:

    1. Resumo o papel do atendente usando um assistente de IA: Explique em 6 a 10 linhas.

    2. Fluxo principal: Descreva passo a passo o caminho feliz, considerando:
        - atendente recebe dúvida do cliente
        - atendente consulta o assistente de IA
        - atendente recebe a resposta com fonte
        - atendente usa a resposta no atendimento
       Apresente em formato sequencial, com passos numerados.

    3. Fluxo de fallback: Descreva o que deve acontecer quando:
        - o assistente não encontra resposta confiável
        - o atendente discorda da resposta apresentada
        - o caso exige validação humana
       Mostre claramente para onde a questão deve ser direcionada.

    4. Fluxo de feedback: Descreva como o atendente informa que a resposta estava:
        - errada
        - desatualizada
        - incompleta
        - sem fonte suficiente
        - em conflito com a prática operacional
       Explique como esse feedback pode alimentar melhoria contínua da base de
       conhecimento e do assistente.

    5. Guardrails obrigatórios do assistente: Defina pelo menos 2 guardrails específicos
       ao contexto da NovaTech. Não quero guardrails genéricos. Eles devem ser aplicáveis
       ao domínio de logística e atendimento.

       Exemplo do tipo de especificidade esperada:
       - nunca inventar prazo de entrega sem base documental

Requisitos de qualidade:
- Não invente capacidades técnicas não descritas.
- Não use conhecimento externo.
- Seja específico e operacional.
- Priorize linguagem clara para negócio e delivery.
- A jornada deve ser compreensível por pessoas não técnicas.
- O conteúdo deve estar pronto para virar um diagrama visual depois.

Entrega: Gere o conteúdo final em Markdown pronto para arquivo com o nome:
jornada-atendente-assistente-ia.md com a estrutura:
  Jornada do Atendente com Assistente de IA
    1. Resumo o papel do atendente usando a IA
    2. Fluxo principal
    3. Fluxo de fallback
    4. Fluxo de feedback
    5. Guardrails do assistente

@Exercicio1.1/entrega-final.md
@Exercicio1.1/analise-de-inconsistencia-proc042.md
@Exercicio1.1/cruzamento-inconsistencia-vs-FAQ.md
@Exercicio1.1/FAQ-atendimento-resumido.md
```

---

## Arquivo Gerado como Resposta

**Nome:** `jornada-atendente-assistente-ia.md`
**Localização:** `Exercicio1.1/jornada-atendente-assistente-ia.md`

Estrutura entregue:
1. Resumo do papel do atendente usando a IA (8 linhas)
2. Fluxo principal — 6 passos sequenciais (caminho feliz)
3. Fluxo de fallback — 4 situações com ações e tabela de encaminhamentos
4. Fluxo de feedback — 5 tipos com tabela e ciclo de melhoria em 3 etapas
5. Guardrails — 5 restrições específicas ao domínio NovaTech (rastreadas para INC e DIV)
6. Diagrama ASCII consolidado dos três fluxos

---

## Segundo Prompt — Extração da Conversa

```
Extraia a conversa desse chat em um arquivo markdown com nome: promp-entrada1.2.md
Inclua as entradas, prompts e documentos que forneci.
```

**Arquivo gerado:** `Exercicio1.1/promp-entrada1.2.md` (este documento)
