# Prompts de Entrada — Exercício 2.2
**Projeto:** NovaTech — Fase de Estruturação  
**Papel:** Product Specialist  
**Data:** 13/06/2026  
**Artefatos gerados:** guardrails-completo.md · classificacao-enforcement.md · rastreabilidade-aos-incidentes.md

---

## Prompt 1 — Definição de Guardrails

**Arquivo gerado:** `Pratica 2/guardrails-completo.md`

---

Sou um Product Specialist e estamos na fase de estruturação do projeto da empresa NovaTec e preciso construir a definição de guardrails como artefato de produto.

### Contexto do cenário atual:

A NovaTech é uma empresa de médio porte do setor de logística com 1.200 funcionários. Sua operação depende de um conjunto extenso de documentação interna: manuais de procedimento operacional, políticas de compliance, tabelas de SLA por tipo de cliente, regras de cálculo de frete, e normas de segurança de carga.
Hoje, essa documentação está espalhada em três fontes: um SharePoint corporativo com 800 documentos (PDFs e Word), uma wiki interna no Confluence com 400 páginas, e uma pasta de rede com planilhas de referência atualizadas mensalmente.
O problema: a equipe de atendimento ao cliente (45 pessoas) gasta em média 12 minutos por chamado buscando informações nessas fontes para responder dúvidas de clientes sobre prazos, regras de frete, políticas de devolução e procedimentos de reclamação. Isso gera atrasos, respostas inconsistentes e frustração tanto dos atendentes quanto dos clientes.
O volume médio é de 320 chamados/dia, dos quais 60% envolvem consulta a documentação.
A documentação é atualizada mensalmente por 3 áreas diferentes (Operações, Compliance, Comercial), sem processo unificado de revisão.
Alguns documentos se contradizem entre versões — a equipe de atendimento hoje resolve isso "perguntando para quem sabe".
A expectativa da diretoria é reduzir o tempo médio de busca de 12 para menos de 2 minutos por chamado.
Dados do Discovery da jornada do atendente: Os atendentes hoje abrem em média 4 fontes diferentes por chamado.
As dúvidas mais comuns são sobre prazos de entrega (35%), regras de frete (25%), política de devolução (20%) e outros (20%). Em 15% dos casos, o atendente não encontra resposta e escala para o supervisor.

### Contexto do Projeto

Estou formalizando os guardrails do assistente NovaTech, usado por atendentes internos para responder dúvidas sobre SLAs, frete e devoluções no domínio de logística.

### Guardrails informais já identificados (fase anterior)

1. Sempre citar fonte
2. Nunca inventar prazos ou valores
3. Quando não encontrar resposta, dizer explicitamente
4. Responder em português formal

### Incidentes reais ocorridos em testes internos

- **INCIDENTE-01:** O assistente respondeu que o prazo de devolução para carga perigosa é 7 dias, quando na verdade cargas perigosas NÃO podem ser devolvidas.
- **INCIDENTE-02:** O assistente citou "PROC-042, seção 2" mas os multiplicadores informados eram da versão 1 (desatualizada), não da v2 (vigente).
- **INCIDENTE-03:** O assistente disse "Não encontrei informação sobre isso" para uma pergunta sobre SLA Gold, mas o documento SLA-2024 estava indexado e continha a resposta.

### Documentação NovaTech — Anexo A (fonte de verdade)

`@Prática 1/anexo-a-documentacao-simulada-novatech.md`

### Tarefa

Elabore o documento de guardrails do assistente NovaTech organizado nas três seções abaixo.

**Regras obrigatórias:**
- Os guardrails devem ser específicos ao domínio NovaTech — não aceito guardrails genéricos como "seja preciso" ou "seja útil"
- Cada guardrail deve ser acionável: um desenvolvedor deve saber exatamente como implementá-lo
- Use a linguagem ubíqua do domínio (carga perigosa, SLA Gold, PROC-042, frete especial etc.)

#### Seção 1 — DEVE (comportamentos obrigatórios)
O que o assistente sempre deve fazer, sem exceção.

#### Seção 2 — NÃO DEVE (comportamentos proibidos)
O que o assistente nunca pode fazer, independente da pergunta.

#### Seção 3 — QUANDO EM DÚVIDA (comportamentos de fallback)
O que o assistente faz quando não tem certeza, quando a fonte está desatualizada, ou quando a pergunta cruza dois contextos.

Para cada guardrail nas três seções, entregue:
- **ID:** GR-001, GR-002...
- **Descrição:** o comportamento esperado em linguagem clara
- **Trigger:** quando este guardrail é ativado
- **Exemplo correto:** como o assistente deve responder
- **Exemplo incorreto:** o que o assistente NÃO deve fazer

**Entrega:** Gere o documento de guardrails num arquivo Markdown com o nome: `guardrails-completo.md`

---

## Prompt 2 — Classificação de Enforcement

**Arquivo gerado:** `Exercicio2.2/classificacao-enforcement.md`

---

Sou um Product Specialist e estamos na fase de estruturação do projeto da empresa NovaTec e preciso construir a classificação de enforcement dos guardrails gerados na etapa anterior.

### Contexto

Preciso classificar cada guardrail do documento abaixo quanto ao mecanismo de enforcement mais adequado.

**Documento de guardrails NovaTech:** `@Exercicio2.2/guardrails-completo.md`

### Conceitos que você deve aplicar

- **Enforcement via prompt (probabilístico):** o comportamento é instruído no system prompt ou na query — o modelo tende a seguir, mas não há garantia absoluta. Adequado para comportamentos interpretativos, de tom ou de formato.
- **Enforcement via código (determinístico):** o comportamento é garantido por lógica fora do modelo — validação, filtro, pós-processamento ou bloqueio em camada de aplicação. Adequado para verificações binárias, checagens de versão, bloqueios por categoria de dado.

### Tarefa

Para cada guardrail (GR-001, GR-002...), entregue:

- **ID do guardrail**
- **Classificação:** Prompt / Código / Híbrido (prompt + código)
- **Justificativa:** por que este mecanismo é o mais adequado para este guardrail específico
- **Como implementar:** descrição técnica objetiva do mecanismo
  - Se prompt: trecho exato de instrução para o system prompt
  - Se código: qual camada valida (pré-processamento, pós-processamento, camada de dados) e qual a lógica
  - Se híbrido: o que o prompt cobre e o que o código cobre

**Formato de saída:** tabela markdown + bloco de detalhamento por guardrail classificado como código ou híbrido.

```
| ID | Guardrail (resumo) | Classificação | Justificativa |
```

**Entrega:** Gere o documento de classificação de enforcement em um arquivo Markdown com o nome: `classificacao-enforcement.md`

---

## Prompt 3 — Rastreabilidade aos Incidentes

**Arquivo gerado:** `Exercicio2.2/rastreabilidade-aos-incidentes.md`

---

Sou um Product Specialist e estamos na fase de estruturação do projeto da empresa NovaTec e preciso construir a rastreabilidade de incidentes que cada guardrail previne.

### Contexto

Preciso conectar os guardrails do assistente NovaTech aos incidentes que os motivaram, criando rastreabilidade entre risco e controle.

**Documento de guardrails com classificação:**
- `@Exercicio2.2/guardrails-completo.md`
- `@Exercicio2.2/classificacao-enforcement.md`

### Incidentes de referência

- **INCIDENTE-01:** Assistente afirmou prazo de 7 dias para devolução de carga perigosa — categoria proibida de devolução.
- **INCIDENTE-02:** Assistente citou multiplicadores da versão 1 (desatualizada) de PROC-042, ignorando a v2 vigente.
- **INCIDENTE-03:** Assistente retornou "Não encontrei informação" para SLA Gold, documento indexado e disponível.

### Tarefa

#### Parte 1 — Matriz de rastreabilidade

Para cada guardrail, indique:
- Qual(is) incidente(s) ele previne diretamente
- Como ele teria evitado o incidente (mecanismo causal)
- Se ele é preventivo (evita o erro) ou corretivo (detecta após)

**Formato:** tabela markdown
```
| ID Guardrail | Incidente(s) coberto(s) | Como previne | Preventivo / Corretivo |
```

#### Parte 2 — Análise de cobertura

Após a matriz, responda:
1. Algum incidente ficou sem cobertura por nenhum guardrail?
2. Algum guardrail não está rastreado a nenhum incidente ou risco concreto? (candidato a remoção ou generalização)
3. Há riscos derivados dos incidentes que os guardrails atuais ainda não cobrem?

#### Parte 3 — Documento final consolidado

Reescreva o documento de guardrails incorporando a rastreabilidade. Cada guardrail deve ter um campo adicional:
- **Rastreabilidade:** INCIDENTE-0X — [descrição do risco coberto]

Este é o entregável final do exercício.

**Entrega:** Gere o documento final de rastreabilidade dos incidentes contemplando as partes 1, 2 e 3 num arquivo Markdown com o nome: `rastreabilidade-aos-incidentes.md`

---

## Estrutura de artefatos gerados

```
Pratica 2/
└── guardrails-completo.md          ← Prompt 1

Exercicio2.2/
├── guardrails-completo.md          ← Prompt 1 (cópia de trabalho)
├── classificacao-enforcement.md    ← Prompt 2
├── rastreabilidade-aos-incidentes.md ← Prompt 3
└── promp-entrada2.2.md             ← este arquivo
```

## Cadeia de dependência entre os prompts

```
Prompt 1                    Prompt 2                      Prompt 3
────────────────────        ──────────────────────────    ──────────────────────────────
Entrada:                    Entrada:                      Entrada:
  · Contexto NovaTech         · guardrails-completo.md     · guardrails-completo.md
  · Guardrails informais      · Conceitos prompt/código    · classificacao-enforcement.md
  · 3 incidentes              · Tarefa de classificação    · 3 incidentes de referência
  · Anexo A (docs)                                         · Tarefa em 3 partes
                            Saída:
Saída:                        · Tabela de classificação   Saída:
  · 17 guardrails               (Prompt/Código/Híbrido)     · Matriz de rastreabilidade
    organizados em 3            · Trechos de system prompt  · Análise de cobertura
    seções (DEVE /              · Pseudocódigos             · 3 riscos derivados
    NÃO DEVE /                  · Diagrama arquitetural      não cobertos
    QUANDO EM DÚVIDA)                                       · Guardrails v2.0 com
                                                             campo Rastreabilidade
```
