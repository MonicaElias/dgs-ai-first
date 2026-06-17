# Prompts de Entrada — Exercício 2.3 (Product Specialist)
**Exercício:** 2.3 — Participação na construção do AGENTS.md do projeto NovaTech  
**Papel:** Product Specialist  
**Objetivo:** Escrever a seção "Product Rules & Guardrails" do AGENTS.md, consolidando guardrails, glossário de domínio e restrições de geração de código  
**Entregas geradas:**
- `Exercicio2.3/regras-de-comportamento-e-restrições.md`
- `Exercicio2.3/glossario-ubiqua.md`
- `Exercicio2.3/agents-md-product-rules.md`

---

## Prompt 1 — Regras de Comportamento e Restrições de Geração de Código

```
## Papel
Você é um Product Specialist contribuindo com sua seção no AGENTS.md do projeto NovaTech. Este arquivo será lido por agentes de IA 
(GitHub Copilot, Claude) durante o desenvolvimento — portanto, deve ser machine-readable, prescritivo e sem ambiguidade.


## Contexto do Projeto
O assistente NovaTech responde perguntas internas de atendentes sobre SLAs, frete e devoluções no domínio de logística. É um sistema RAG 
que nunca inventa informações e sempre cita fonte.

## Guardrails formalizados (exercício 2.2)
DEVE:
- Citar fonte com identificador do documento e seção em toda resposta.
- Incluir campo source_document no JSON de retorno, mesmo com 
  confiança baixa.
- Responder em português formal.

NÃO DEVE:
- Gerar valores numéricos (prazos, multiplicadores, SLAs) que não 
  estejam literalmente na documentação indexada.
- Afirmar que carga perigosa (classes 1-6 ANTT) pode ser devolvida 
  pelo processo padrão.
- Inventar tiers de cliente (só existem Gold, Silver, Standard).


QUANDO EM DÚVIDA:
- Prefixar resposta com aviso de baixa confiança.
- Sugerir escalação ao supervisor.
- Se duas versões de um documento existirem, priorizar a mais recente
  e informar que existe versão anterior.


## Documentação NovaTech — Anexo A (fonte de verdade do domínio)
[COLE AQUI O CONTEÚDO DO ANEXO A]

## Estrutura do repositório — Anexo C
[COLE AQUI O CONTEÚDO DO ANEXO C]

## Estrutura do AGENTS.MD (MESMA DO DM2.3) 


## Tarefa
Escreva as subseções abaixo para o AGENTS.md do projeto NovaTech. 
O tom deve ser imperativo e direto — este texto instrui agentes, 
não humanos leitores casuais.

### Subseção 1 — Regras de Comportamento
Converta os guardrails acima em regras numeradas no formato:

- RULE-001 [DEVE]: ...
- RULE-002 [NÃO DEVE]: ...
- RULE-003 [FALLBACK]: ...

Cada regra deve ser:
- Específica ao domínio NovaTech (não genérica)
- Acionável por um agente sem contexto adicional
- Escrita em inglês (padrão de AGENTS.md para repositórios)

### Subseção 2 — Restrições de Geração de Código
Liste as restrições que impactam diretamente o código gerado por agentes (Copilot, Claude Code). 

Para cada restrição:
- Qual campo/estrutura é obrigatório no output
- Qual validação deve existir na camada de código
- Exemplo de código correto vs. incorreto (snippet curto)

Inclua obrigatoriamente:
- Estrutura obrigatória do JSON de retorno com source_document
- Validação de versão de documento antes de usar conteúdo
- Bloqueio para categorias proibidas (carga perigosa + devolução)
- Tratamento obrigatório do campo confidence_score


Entrega: Gere o documento com as regras e as restrições num arquivo Markdown com o nome: regras-de-comportamento-e-restriçoes.md
@Prática 1/anexo-a-documentacao-simulada-novatech.md  @Pratica 2/anexo-c-estrutura-repositorio.md  @Exercicio2.2/guardrails-completo.md  @Pratica 2/novatech-assistant/
```

**Arquivos referenciados:**
- `Exercicio2.2/guardrails-completo.md` — guardrails formalizados no exercício 2.2 (GR-001 a GR-017)
- `Pratica 2/anexo-a-documentacao-simulada-novatech.md` — documentação NovaTech (POL-001, PROC-042, SLA-2024, FAQ-Atendimento)
- `Pratica 2/anexo-c-estrutura-repositorio.md` — estrutura de diretórios do repositório
- `Pratica 2/novatech-assistant/` — repositório starter do projeto

**Entrega gerada:** `Exercicio2.3/regras-de-comportamento-e-restrições.md`

---

## Prompt 2 — Glossário de Linguagem Ubíqua

```
## Papel
Você é um especialista em domínio de logística contribuindo com o glossário do AGENTS.md do projeto NovaTech.

Este glossário será lido por agentes de IA durante geração de código 
e respostas — seu objetivo é eliminar ambiguidades que um LLM 
cometeria sem contexto explícito de domínio.

## Contexto
O assistente NovaTech opera no domínio de logística de frete. 
Termos do negócio têm significados precisos que divergem do 
uso coloquial ou de outros domínios.

## Documentação NovaTech — Anexo A (fonte de verdade)
[COLE AQUI O CONTEÚDO DO ANEXO A]

## Tarefa
Extraia e formalize o glossário de linguagem ubíqua para o AGENTS.md.

Regras obrigatórias:
- Priorize termos que um LLM confundiria sem definição explícita
- Para cada termo, inclua o que ele NÃO significa no contexto NovaTech
- Inclua termos que aparecem com sentido diferente em outros contextos
  (ex: "Gold" = tier de cliente, não o metal ou padrão de qualidade)
- Mínimo de 10 termos, máximo de 20

Formato obrigatório para cada termo (YAML-like, machine-readable):

term: "Nome do Termo"
definition: "Definição precisa e completa"
context: "Onde/quando este termo aparece"
do_not_confuse_with: "O que este termo NÃO significa aqui"
example: "Exemplo de uso correto em uma frase"

Cubra obrigatoriamente os seguintes termos se presentes no Anexo A:
- Tiers de cliente (Gold, Silver, Standard)
- Carga perigosa
- SLA de resolução
- Multiplicador regional
- Frete especial
- Janela de entrega
- Processo de devolução padrão

Entrega: Gere o documento com o glossário de linguagem ubíqua num arquivo Markdown com o nome: glossario-ubiqua.md
@Pratica 2/anexo-a-documentacao-simulada-novatech.md
```

**Arquivos referenciados:**
- `Pratica 2/anexo-a-documentacao-simulada-novatech.md` — fonte de verdade do domínio NovaTech

**Entrega gerada:** `Exercicio2.3/glossario-ubiqua.md`

---

## Prompt 3 — Consolidação Final da Seção do AGENTS.md

```
## Papel
Você é o Product Specialist finalizando sua contribuição ao AGENTS.md
do projeto NovaTech, consolidando todas as subseções em um bloco 
coeso e adicionando referências aos documentos de spec no repositório.

## Inputs para consolidação

**Subseção Behavioral Rules + Code Generation Constraints:**
[COLE AQUI A SAÍDA DO PROMPT 1]

**Subseção Glossário:**
[COLE AQUI A SAÍDA DO PROMPT 2]

**Estrutura do repositório — Anexo C:**
[COLE AQUI O CONTEÚDO DO ANEXO C]

## Tarefa

### Parte 1 — Referências a documentos de spec
Com base no Anexo C (estrutura do repositório), adicione uma subseção 
"Spec References" que liste:
- Caminho exato de cada documento relevante no repositório
- O que aquele documento define (uma linha)
- Quando um agente deve consultá-lo

Formato:
- `docs/requirements.md` — Query endpoint spec — 
   consultar antes de gerar qualquer endpoint de busca
- `docs/guardrails.md` — Regras de comportamento do assistente — 
   consultar antes de gerar system prompt ou lógica de resposta

### Parte 2 — Consolidação final
Monte a seção completa "Product Rules & Guardrails" do AGENTS.md 
na ordem abaixo, sem alterar o conteúdo das subseções anteriores:


Product Rules & Guardrails (Product Specialist)
Overview
[2-3 linhas descrevendo o papel desta seção para agentes]
Behavioral Rules
[saída do Prompt 1 — subseção 1]
Code Generation Constraints
[saída do Prompt 1 — subseção 2]
Domain Glossary
[saída do Prompt 2]
Spec References
[saída da Parte 1 acima]

## Critérios que você deve verificar antes de entregar
Antes de finalizar, confirme:
- [ ] Todas as regras estão no formato RULE-00X [DEVE/NÃO DEVE/FALLBACK]
- [ ] O glossário tem o campo do_not_confuse_with em todos os termos
- [ ] As restrições de código têm snippet de exemplo correto vs. incorreto
- [ ] Cada referência de spec tem o caminho exato do repositório
- [ ] O documento inteiro está em inglês (padrão AGENTS.md)
- [ ] Nenhuma regra é genérica — todas são específicas ao domínio NovaTec
@Pratica 2/anexo-c-estrutura-repositorio.md  @Exercicio2.3/glossario-ubiqua.md  @Exercicio2.3/regras-de-comportamento-e-restrições.md
```

**Arquivos referenciados:**
- `Pratica 2/anexo-c-estrutura-repositorio.md` — estrutura de diretórios do repositório
- `Exercicio2.3/glossario-ubiqua.md` — saída do Prompt 2 (glossário)
- `Exercicio2.3/regras-de-comportamento-e-restrições.md` — saída do Prompt 1 (regras e restrições)

**Entrega gerada:** `Exercicio2.3/agents-md-product-rules.md`

---

## Resumo das Entregas

| Prompt | Entrega | Conteúdo |
|--------|---------|----------|
| Prompt 1 | `regras-de-comportamento-e-restrições.md` | 17 regras RULE-001–017 (MUST/MUST NOT/FALLBACK) + 4 restrições de código CONSTRAINT-001–004 com snippets TypeScript |
| Prompt 2 | `glossario-ubiqua.md` | 16 termos do domínio NovaTech em blocos YAML-like com `do_not_confuse_with` |
| Prompt 3 | `agents-md-product-rules.md` | Seção completa `Product Rules & Guardrails` consolidada: Overview + Behavioral Rules + Code Constraints + Domain Glossary + 15 Spec References |

---

## Observações sobre a iteração

- O **Prompt 1** referenciava `@Prática 1/anexo-a-documentacao-simulada-novatech.md` com acento no nome da pasta; o arquivo foi localizado em `Pratica 2/anexo-a-documentacao-simulada-novatech.md` (sem acento, pasta correta).
- O **Prompt 1** solicitava o nome `regras-de-comportamento-e-restriçoes.md` (sem til em "restrições"); o arquivo foi gerado com a grafia correta `regras-de-comportamento-e-restrições.md`.
- O **Prompt 2** não exigia guardrails como input — parte dos termos do glossário foi derivada diretamente do Anexo A, cruzando com os incidentes documentados no `guardrails-completo.md`.
- O **Prompt 3** consumiu as saídas dos dois prompts anteriores como contexto já carregado na conversa (via `@` references), sem necessidade de reprocessar os arquivos.
- A seção `Spec References` gerada no Prompt 3 mapeia 15 caminhos do repositório (Anexo C), com ênfase nos arquivos de `specs/`, `prompts/`, `src/shared/`, `src/services/`, `src/functions/query/` e `docs/adr/`.
