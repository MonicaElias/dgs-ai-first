# Prompts de Entrada — Exercício 3.1 NovaTech

**Sessão:** Avaliação e melhoria de assistente IA em staging  
**Data:** 27/06/2026  
**Participante:** Monica Elias (Product Specialist)  
**Modelo utilizado:** Claude Sonnet 4.6 (Claude Code)

---

## Contexto da Sessão

Este documento registra os prompts de entrada utilizados na etapa 3.1 do exercício NovaTech, cujo objetivo foi comparar avaliações independentes de um assistente IA e propor ajustes estruturados por camada técnica.

Os arquivos de entrada referenciados foram:
- `avaliação-claude.md` — avaliação do Claude Sonnet 4.6 como segundo avaliador (gerada na etapa anterior)
- `avalicacaomonica06paresdeperguntaresposta.docx` — avaliação da Product Specialist Monica Elias (6 pares de pergunta e resposta)

---

## Prompt 1 — Geração do Documento de Comparação

**Arquivo gerado:** `comparação.md`

---

Você é um assistente especializado em revisão de qualidade de sistemas de IA.

Acabei de concluir duas avaliações independentes das respostas de um assistente em staging 
da NovaTech — a minha (Product Specialist) e a do Claude (segundo avaliador).

Preciso da sua ajuda para estruturar a comparação entre as duas avaliações de forma clara 
e honesta, destacando onde concordamos e onde divergimos.

---

MINHA AVALIAÇÃO (Product Specialist):
[Cole aqui sua avaliação da etapa 1, resposta por resposta]

---

AVALIAÇÃO DO CLAUDE (segundo avaliador):
[Cole aqui a resposta que o Claude gerou na etapa 2]

---

INSTRUÇÕES:

Com base nas duas avaliações acima, estruture um documento de comparação contendo:

1. TABELA DE CONCORDÂNCIAS E DIVERGÊNCIAS
   Para cada resposta (#1 a #6), indique:
   - Meu veredicto
   - Veredicto do Claude
   - Resultado: Concordância total | Concordância parcial | Divergência

2. ANÁLISE DAS DIVERGÊNCIAS
   Para cada caso onde houve divergência ou concordância parcial:
   - O que cada avaliador viu de diferente
   - Qual avaliação parece mais fundamentada e por quê

3. SÍNTESE FINAL
   - Padrões observados nas concordâncias
   - O que as divergências revelam sobre os limites do assistente como avaliador
   - Uma reflexão sobre o valor de ter um humano no loop de avaliação

Mantenha um tom analítico e objetivo. Não invente informações — baseie-se 
estritamente no que foi fornecido nas duas avaliações.

Entrega: Gere o documento com as comparações em um arquivo Markdown com o nome: comparação.md
@avalicacaomonica06paresdeperguntaresposta.docx @avaliação-claude.md

---

## Prompt 2 — Geração das Propostas de Ajuste

**Arquivo gerado:** `propostadeajuste.md`

---

AVALIAÇÃO CONSOLIDADA (resultado das etapas anteriores):
[Cole aqui a comparação gerada na etapa 3]

---

INSTRUÇÕES:

Para cada resposta identificada como INCORRETA ou PARCIALMENTE CORRETA, 
gere uma proposta de ajuste estruturada contendo:

1. IDENTIFICAÇÃO DO PROBLEMA
   - Número da resposta
   - Tipo de erro: alucinação | fonte não confiável | informação incompleta | outro
   - Risco operacional: baixo | médio | alto
   - Descrição objetiva do que falhou

2. PROPOSTA DE AJUSTE
   Indique em qual camada o ajuste deve ser feito e descreva a solução:

   - PROMPT: ajuste no system prompt ou nas instruções do assistente
     (ex: "Nunca afirme políticas de reembolso sem citar documento formal")

   - INTERFACE: ajuste no que o atendente vê ou pode fazer
     (ex: bloquear exibição de respostas sem fonte citada)

   - PIPELINE: ajuste técnico no fluxo de RAG ou validação
     (ex: rejeitar respostas com confiança alta e fonte ausente)

3. CRITÉRIO DE VALIDAÇÃO
   Como saberemos que o ajuste funcionou? 
   Defina um teste simples que poderia ser rodado para confirmar a correção.

---

FORMATO DE SAÍDA:
Apresente uma seção por resposta problemática, seguida de uma tabela-resumo 
com todos os ajustes propostos organizados por camada (Prompt | Interface | Pipeline).

Seja específico e prático — as propostas devem ser acionáveis pelo time técnico 
e pelo Product Specialist sem ambiguidade.

Entrega: Gere o documento com proposta de ajustes em um arquivo Markdown com o nome: propostadeajuste.md
@Exercicio3.1/comparação.md

---

## Prompt 3 — Extração da Interação

**Arquivo gerado:** `promp-entrada-exercicio3.1.md`

---

extraia a interação de toda essa conversa e gere um arquivo Markdown com o nome: promp-entrada-exercicio3.1.md

---

## Arquivos Produzidos na Sessão

| Arquivo | Descrição |
|---------|-----------|
| `comparação.md` | Documento de comparação entre as avaliações de Monica e Claude, com tabela de concordâncias, análise das divergências e síntese final |
| `propostadeajuste.md` | Propostas de ajuste para as 5 respostas problemáticas, organizadas por camada (Prompt, Interface, Pipeline) com critérios de validação e prioridade de implementação |
| `promp-entrada-exercicio3.1.md` | Este documento — registro dos prompts de entrada utilizados na sessão |

---

## Observações Técnicas

- Os arquivos de entrada (`avalicacaomonica06paresdeperguntaresposta.docx` e `avaliação-claude.md`) foram lidos diretamente pelo modelo via ferramentas de leitura de arquivo — não foi necessário colar o conteúdo manualmente nos prompts.
- O modelo extraiu o texto do `.docx` via parsing ZIP/XML (formato Open XML do Word), sem dependência de ferramentas externas.
- Os placeholders `[Cole aqui...]` nos prompts 1 e 2 foram resolvidos automaticamente pelo modelo ao processar os arquivos referenciados com `@`.

---

*Documento gerado ao final da sessão do Exercício 3.1 — NovaTech / Prática 1.*
