# Prompts de Entrada — Etapa 2: Avaliação Independente pelo Claude

**Sessão:** Avaliação de qualidade do assistente IA NovaTech (staging)  
**Data:** 24/06/2026  
**Participante:** Monica Elias (Product Specialist)  
**Modelo utilizado:** Claude Sonnet 4.6 (Claude Code)

---

## Contexto da Sessão

Este documento registra o prompt de entrada utilizado na Etapa 2 do exercício NovaTech, cujo objetivo foi obter uma avaliação independente do Claude como segundo avaliador das respostas de um assistente em staging.

O arquivo de entrada referenciado foi:
- `Prática 1/anexo-a-documentacao-simulada-novatech.md` — documentação oficial da NovaTech (fonte de verdade)

---

## Prompt 1 — Avaliação dos 6 Pares de Pergunta/Resposta

**Arquivo gerado:** `Prática 1/avaliação-claude.md`

---

Você é um avaliador especialista em qualidade de assistentes de IA para atendimento corporativo.

Vou te apresentar 6 pares de pergunta/resposta gerados por um assistente em staging da NovaTech, 
junto com a documentação oficial (Anexo A) que serve como fonte de verdade.

Sua tarefa é avaliar cada resposta de forma independente, sem se deixar influenciar pela 
confiança declarada pelo sistema 

---

ANEXO A — DOCUMENTAÇÃO NOVATECH (fonte de verdade)

---

RESPOSTAS DO ASSISTENTE PARA AVALIAÇÃO:

#1
Pergunta: "Qual o prazo de devolução para produtos standard?"
Resposta: "O prazo é de 7 dias úteis após o recebimento. O cliente deve abrir chamado no portal e anexar fotos."
Fonte citada: POL-001, seção 3.2
Confiança declarada: Alta

#2
Pergunta: "Meu cliente é Silver. Qual o prazo de resolução?"
Resposta: "O cliente Silver tem resolução em até 48h."
Fonte citada: SLA-2024
Confiança declarada: Alta

#3
Pergunta: "Posso devolver carga perigosa classe 3?"
Resposta: "Não. Cargas perigosas (classes 1 a 6 da ANTT) não podem ser devolvidas pelo processo padrão. Recomendo escalar para o supervisor."
Fonte citada: POL-001, seção 3.2
Confiança declarada: Alta

#4
Pergunta: "Qual a política para carga danificada durante transporte?"
Resposta: "A política de danos prevê reembolso integral quando comprovada negligência da transportadora, mediante laudo técnico e fotos."
Fonte citada: Nenhuma
Confiança declarada: Alta

#5
Pergunta: "Qual o SLA do cliente Enterprise?"
Resposta: "O tier Enterprise não foi encontrado na documentação. Os tiers documentados são Gold, Silver e Standard. Sugiro confirmar a classificação ou escalar ao supervisor."
Fonte citada: —
Confiança declarada: Baixa

#6
Pergunta: "Posso enviar carga perigosa com frete expresso?"
Resposta: "Sim, cargas perigosas podem ser enviadas via frete expresso mediante autorização prévia do compliance e documentação ANTT atualizada."
Fonte citada: FAQ-Atendimento, item 32
Confiança declarada: Alta

---

INSTRUÇÕES DE AVALIAÇÃO:

Para cada resposta, avalie:
1. Veredicto: CORRETA / PARCIALMENTE CORRETA / INCORRETA
2. Justificativa: com base no Anexo A, o que está certo ou errado?
3. Tipo de erro (se houver): alucinação | fonte não confiável | informação incompleta | outro
4. Risco operacional: baixo | médio | alto — considerando o impacto se um atendente agir com base nessa resposta

Seja criterioso. Não confie na confiança declarada pelo sistema — avalie o conteúdo.
Apresente sua avaliação em formato de tabela, seguida de um parágrafo de síntese destacando 
os casos mais críticos.


Entrega: Gere o documento com as avaliação do claude em um arquivo Markdown com o nome: avaliação-claude.md
@Prática 1/anexo-a-documentacao-simulada-novatech.md

---

## Resultado da Avaliação

| # | Pergunta (resumida) | Veredicto | Tipo de Erro | Risco |
|---|---------------------|-----------|--------------|-------|
| 1 | Prazo de devolução para produtos standard? | PARCIALMENTE CORRETA | Fonte incorreta + Informação incompleta | Médio |
| 2 | Prazo de resolução para cliente Silver? | PARCIALMENTE CORRETA | Informação incompleta | Médio |
| 3 | Posso devolver carga perigosa classe 3? | PARCIALMENTE CORRETA | Informação incompleta | Médio |
| 4 | Política para carga danificada durante transporte? | INCORRETA | Alucinação | **Alto** |
| 5 | Qual o SLA do cliente Enterprise? | CORRETA | — | Baixo |
| 6 | Posso enviar carga perigosa com frete expresso? | PARCIALMENTE CORRETA | Fonte não confiável | **Alto** |

**Principais achados da avaliação:**
- Apenas 1 de 6 respostas foi considerada Correta (resposta #5)
- 2 respostas apresentam risco operacional Alto (#4 e #6)
- Padrão sistêmico identificado: confiança declarada pelo sistema foi mais alta justamente nas respostas com maior risco real — indicando necessidade de revisão do mecanismo de calibração antes da homologação

---

## Arquivo Produzido na Sessão

| Arquivo | Descrição |
|---------|-----------|
| `Prática 1/avaliação-claude.md` | Avaliação independente do Claude como segundo avaliador, contendo tabela com veredicto, justificativa, tipo de erro e risco operacional para cada um dos 6 pares de pergunta/resposta, seguida de síntese com os casos críticos |

---

## Observações Técnicas

- O Anexo A foi lido diretamente pelo modelo via ferramenta de leitura de arquivo (`Prática 1/anexo-a-documentacao-simulada-novatech.md`) — o conteúdo completo dos 5 documentos NovaTech (POL-001, PROC-042, PROC-042-v2, SLA-2024 e FAQ-Atendimento) foi processado como fonte de verdade.
- A avaliação foi conduzida de forma independente, sem acesso à avaliação humana prévia, conforme instrução do exercício.
- O modelo foi explicitamente instruído a ignorar a confiança declarada pelo sistema ao julgar cada resposta.

---

*Documento gerado ao final da sessão da Etapa 2 — NovaTech / Prática 1.*
