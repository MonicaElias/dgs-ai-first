# Tarefa 1 - Etapa 1 | Mapa de temas e hipóteses de gaps

---

## Seção 1 — Dados de Entrada

| # | Arquivo | Tipo | Versão | Data |
|---|---------|------|--------|------|
| 1 | FAQ-atendimento-resumido.md | Informal / colaborativo | Não controlada | Diversas |
| 2 | POL-001-politica-devolucao-resumido.md | Normativo | 3.1 | 15/01/2024 |
| 3 | PROC-042-frete-especial-v1-resumido.md | Procedimento | 1.0 | 03/03/2023 |
| 4 | PROC-042-v2-frete-especial-revisado1-resumido.md | Procedimento | 2.0 | 10/11/2023 |
| 5 | SLA-2024-tabela-sla-clientes-resumido.md | Contratual | 2024.1 | 02/01/2024 |

---

## Seção 2 — Mapa de Temas Cobertos

| Tema | Documento(s) de Referência | Status de Cobertura |
|------|---------------------------|---------------------|
| Prazo para solicitação de devolução (7 dias úteis) | POL-001 | Coberto — normativo |
| Cargas inelegíveis para devolução padrão (perigosas, cadeia de frio, lacre violado) | POL-001 | Coberto — normativo |
| Procedimento de abertura de chamado de devolução (Portal, CT-e, fotos) | POL-001 | Coberto — normativo |
| Devolução parcial por volume com reembolso proporcional | POL-001 | Coberto — normativo |
| Custo do frete reverso (cliente vs. NovaTech) | POL-001 | Coberto — normativo |
| Cargas perigosas — encaminhamento para Gestão de Riscos | FAQ | Coberto — somente informal |
| Carga danificada em trânsito — registro de ocorrência e encaminhamento a sinistros | FAQ | Coberto — somente informal |
| Fórmula de cálculo do frete especial (tarifa base × multiplicador × fator peso) | PROC-042 v1, PROC-042 v2 | Conflito entre versões |
| Multiplicadores regionais | PROC-042 v1, PROC-042 v2 | Conflito de valores (v1: 1,0–1,6 / v2: 1,1–1,8) |
| Prazo adicional de entrega para frete especial | PROC-042 v1, PROC-042 v2 | Conflito de valores (v1: +2 dias / v2: +3 dias) |
| Aprovação prévia para cargas acima de 5.000 kg | PROC-042 v1, PROC-042 v2 | Coberto — consistente entre versões |
| Descontos de volume para clientes recorrentes | PROC-042 v1, PROC-042 v2 | Conflito de regras (v1: negociação / v2: automático) |
| Frete especial para cargas perigosas acima de 500 kg | PROC-042 v1, PROC-042 v2 (ref. PROC-043) | Gap — PROC-043 não disponível |
| Tiers de cliente (Gold, Silver, Standard) | SLA-2024, FAQ | Coberto — consistente |
| SLA de resposta e resolução por tier | SLA-2024, FAQ | Coberto — FAQ alinha com SLA-2024 |
| Incidentes críticos — critérios e prazos diferenciados | SLA-2024 | Coberto — normativo |
| Disponibilidade do portal de tracking por tier (98% a 99,5%) | SLA-2024 | Coberto — normativo |
| Gerente de conta dedicado (exclusivo Gold) | SLA-2024 | Coberto — normativo |
| Créditos por violação de SLA (progressivos a partir da 2ª ocorrência) | SLA-2024 | Coberto — normativo |
| Seguro de carga adicional (0,3% padrão / 0,8% perigosas) | FAQ | Coberto — somente informal |
| Autonomia do atendente para concessão de desconto | FAQ, PROC-042 v2 | Parcialmente coberto — inconsistente |
| Frete expresso para cargas perigosas (autorização Compliance + ANTT) | FAQ | Coberto — somente informal |
| Tracking parado — critérios de anomalia por rota | FAQ | Coberto — somente informal |
| Critérios de elegibilidade e classificação de tier | SLA-2024 | Parcialmente coberto — sem critérios detalhados |
| Procedimento de solicitação de crédito por violação de SLA | — | Gap — não coberto |

---

## Seção 3 — Conflitos e Sobreposições

### Conflito 1 — Multiplicadores regionais (PROC-042 v1 × v2)
- **v1 (2023):** Sudeste 1,0 / Norte 1,6
- **v2 (2023):** Sudeste 1,1 / Norte 1,8
- **Situação:** Ambas coexistem no SharePoint sem hierarquia formal definida. Nenhuma declara obsolescência da outra.

### Conflito 2 — Prazo adicional de entrega (PROC-042 v1 × v2)
- **v1:** +2 dias úteis ao prazo padrão da rota
- **v2:** +3 dias úteis ao prazo padrão da rota
- **Situação:** Diferença de 1 dia útil sem transição formalizada, gerando risco de promessa errada ao cliente.

### Conflito 3 — Descontos de volume (PROC-042 v1 × v2)
- **v1:** Descontos negociados caso a caso pelo Comercial e formalizados em aditivo contratual
- **v2:** Desconto automático de 5% a partir de 8 fretes/mês e 10% a partir de 15 fretes/mês
- **Situação:** Regras operacionalmente incompatíveis. A v2 elimina a negociação abaixo dos limiares, mas a v1 ainda está ativa.

### Conflito 4 — Devolução de cargas perigosas (POL-001 × FAQ)
- **POL-001:** Cargas perigosas (classes 1–6 ANTT) não são elegíveis pelo processo padrão de devolução.
- **FAQ:** Orienta o atendente a não dizer que é "impossível" e a encaminhar para Gestão de Riscos (ramal 4500), sugerindo que exceções são viáveis.
- **Situação:** O FAQ contradiz a política normativa, expondo o atendente a prometer algo que a política não autoriza formalmente.

### Conflito 5 — Autonomia para desconto (FAQ × PROC-042 v2)
- **FAQ:** Atendente não tem autonomia para dar desconto; deve encaminhar ao Comercial.
- **PROC-042 v2:** Prevê desconto automático a partir de 8 fretes/mês, sem mencionar intermediação do atendente.
- **Situação:** Não está claro se o desconto automático da v2 é operacionalizado pelo sistema ou requer ação do atendente/Comercial.

---

## Seção 4 — Hipóteses de Gaps

| # | Gap | Evidência do Gap | Impacto Positivo (se resolvido) | Impacto Negativo (se mantido) | Perguntas para Descoberta |
|---|-----|-----------------|--------------------------------|-------------------------------|---------------------------|
| G1 | PROC-043 ausente — frete para cargas perigosas acima de 500 kg | PROC-042 v1 e v2 referenciam PROC-043 como tabela própria, mas o documento não está disponível nos resumos | Atendimento padronizado e sem risco de precificação incorreta para cargas perigosas | Atendente aplica tabela errada ou nega o serviço por falta de base normativa | O PROC-043 existe formalmente? Está em vigência? Quem é o responsável por ele? Está em revisão conforme citado pela v2? |
| G2 | Seguro de carga sem documento normativo | FAQ menciona percentuais (0,3% / 0,8%) e condicionante de contratos a partir de 2023, mas não há documento oficial nas entradas | Clareza para o cliente e uniformidade na oferta do adicional | Atendentes podem informar valores desatualizados ou incorretos; risco comercial e jurídico | Existe apólice ou documento normativo do seguro de carga? Qual a cobertura exata? Os percentuais mencionados no FAQ foram validados? |
| G3 | Ausência de critério formal de obsolescência entre PROC-042 v1 e v2 | Ambos os documentos declaram não ter hierarquia formal definida; o FAQ orienta uso da v2 sem autoridade para isso | Elimina risco de cálculo divergente entre atendentes | Clientes recebem valores diferentes para o mesmo serviço; risco de contestação contratual | Qual versão está vigente oficialmente? Existe despacho ou comunicado formalizando a transição? A regra de transição da v2 (01/12/2023) foi comunicada ao time? |
| G4 | Carga danificada em trânsito sem procedimento normativo | Apenas o FAQ cobre este fluxo (48h, fotos, laudo, e-mail sinistros@); nenhum documento normativo formal | Processo padronizado reduz disputas e acelera reembolso | Atendentes seguem procedimento informal não validado; risco de inconsistência e passivo jurídico | Existe política ou procedimento formal para sinistros em trânsito? Quem é o responsável (Jurídico, Operações)? O e-mail sinistros@ é o canal oficial? |
| G5 | SLA de tracking por rota não formalizado | FAQ define critérios práticos por região (Norte até 10 dias, Sul/Sudeste >3 dias = anomalia), mas nenhum documento normativo trata disso | Reduz abertura indevida de chamados urgentes; melhora gestão de filas | Atendentes usam critério informal divergente; clientes Gold podem exigir escalada por SLA não documentado | O SLA-2024 cobre prazos de tracking por rota? Existe documento operacional que formalize os prazos por região? |
| G6 | Critérios de classificação de tier não detalhados | SLA-2024 menciona que tiers são definidos por volume e valor contratual, mas não especifica os limites exatos | Facilita qualificação de novos clientes e autoatendimento | Atendentes não conseguem responder com precisão quando o cliente questiona seu tier; risco de reclassificação indevida | Quais são os limiares exatos de volume e valor para cada tier? Existe documento de classificação comercial? Quem decide a reclassificação? |
| G7 | Procedimento de solicitação de crédito por violação de SLA ausente | SLA-2024 define os créditos progressivos, mas não descreve como o cliente os solicita nem como o time processa | Reduz atrito pós-incidente e aumenta confiança do cliente | Cliente não sabe como reivindicar; atendente não tem processo claro; risco de não cumprimento do compromisso contratual | Qual é o canal e prazo para o cliente solicitar o crédito? Quem aprova? O sistema de medição (Azure DevOps) gera alerta automático? |
| G8 | Frete expresso para cargas perigosas sem procedimento normativo | FAQ menciona necessidade de autorização do Compliance e ANTT atualizada, e estima 2 dias para aprovação, mas sem referência a documento formal | Expectativa correta do cliente e fluxo previsível de autorização | Atendente pode subestimar o prazo ou omitir a necessidade de documentação ANTT; risco regulatório | Existe procedimento escrito para frete expresso de cargas perigosas? Quem autoriza no Compliance? Qual a documentação ANTT exigida? |

---

## Seção 5 — Resumo Executivo

A análise dos cinco documentos revela uma base normativa parcialmente estruturada, com cobertura sólida em devolução de mercadorias (POL-001) e SLA (SLA-2024), mas com lacunas críticas nas áreas de frete especial e sinistros. O principal risco operacional imediato é a coexistência sem hierarquia formal das versões v1 e v2 da PROC-042, que gera cálculos divergentes de frete, prazos contraditórios e regras de desconto incompatíveis entre atendentes. O documento de cargas perigosas (PROC-043), referenciado por ambas as versões da PROC-042, está ausente do conjunto normativo disponível. O FAQ, embora útil como guia prático, contradiz em dois pontos a documentação oficial: na possibilidade de devolução de cargas perigosas por exceção (conflito com POL-001) e na autonomia para descontos (ambíguo frente à PROC-042 v2). Temas de alto impacto contratual — seguro de carga, carga danificada em trânsito e procedimento de crédito por violação de SLA — estão cobertos apenas informalmente ou não estão cobertos. Recomenda-se priorizar a formalização da hierarquia entre versões da PROC-042, a localização ou criação do PROC-043, e a criação de procedimentos normativos para sinistros e créditos de SLA antes de qualquer iniciativa de automação ou expansão do atendimento.
