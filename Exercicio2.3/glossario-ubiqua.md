# Glossário de Linguagem Ubíqua — NovaTech Assistant
**Section:** AGENTS.md > Product Rules & Guardrails > Ubiquitous Language Glossary  
**Role:** Domain Specialist  
**Source:** POL-001 v3.1 · PROC-042 v1.0 · PROC-042 v2.0 · SLA-2024 v2024.1 · FAQ-Atendimento  
**Purpose:** Eliminate domain ambiguities that an LLM would commit without explicit context. Every agent reading this file MUST use these definitions — not dictionary definitions, not common-language interpretations.

---

> **How to read this glossary:** Each entry follows a YAML-like block. The `do_not_confuse_with` field is the most critical: it states what the term explicitly does NOT mean in this codebase. When generating code, prompts, or responses, always prefer the exact term over a synonym.

---

```yaml
term: "Tier Gold"
definition: >
  Customer classification tier at NovaTech. Criteria: annual contract above
  R$ 500,000 OR more than 200 logistics operations per month. SLAs: first
  response up to 2 business hours (general tickets), up to 30 minutes
  (critical incidents); resolution up to 24 business hours (general), up to
  4 hours (critical). Includes dedicated account manager. SLA clock does NOT
  pause outside business hours for critical incidents.
context: >
  Appears in SLA-related queries, customer identification, response-time
  calculations, and SLA penalty evaluations. Used to select the correct row
  in SLA-2024 v2024.1, section 2.
do_not_confuse_with: >
  The precious metal gold. A quality standard or premium product tier in a
  generic e-commerce context. Any tier named "Platinum", "Premium", "VIP",
  or any label not in the set {Gold, Silver, Standard} — those tiers do NOT
  exist at NovaTech. Gold is specifically a contractual classification, not
  a subjective quality judgment.
example: >
  "Para clientes Gold, o SLA de primeira resposta em incidentes críticos
  é de até 30 minutos, sem pausa fora do horário comercial.
  Fonte: SLA-2024 v2024.1, seção 2."
```

---

```yaml
term: "Tier Silver"
definition: >
  Customer classification tier at NovaTech. Criteria: annual contract between
  R$ 100,000 and R$ 500,000 OR between 50 and 200 operations per month.
  SLAs: first response up to 4 business hours (general), up to 1 hour
  (critical incidents); resolution up to 48 business hours (general),
  up to 8 hours (critical). No dedicated account manager.
context: >
  Appears in SLA queries and tier identification. Located in SLA-2024
  v2024.1, sections 1 and 2.
do_not_confuse_with: >
  Silver as the precious metal. A mid-range product category. Any tier
  between Gold and Standard that is not explicitly named Silver.
  Silver is a formal contractual classification — never interpolate SLAs
  from other tiers when the tier is Silver.
example: >
  "Para clientes Silver, o tempo de resolução em chamados gerais é de
  até 48 horas úteis. Fonte: SLA-2024 v2024.1, seção 2."
```

---

```yaml
term: "Tier Standard"
definition: >
  Customer classification tier at NovaTech. Criteria: all customers NOT
  qualifying for Gold or Silver. SLAs: first response up to 8 business hours
  (general), up to 2 hours (critical incidents); resolution up to 72 business
  hours (general), up to 24 hours (critical). No dedicated account manager.
  Performance reports available on demand only.
context: >
  Appears in SLA queries when no specific tier is identified or when the
  customer does not meet Gold or Silver thresholds.
do_not_confuse_with: >
  "Standard" as in a default or inferior quality. Standard is a formal
  NovaTech tier with its own SLA commitments — not a fallback or a
  degraded service. Do not invent a tier between Standard and Silver.
example: >
  "Clientes Standard têm SLA de primeira resposta de até 8 horas úteis.
  Fonte: SLA-2024 v2024.1, seção 2."
```

---

```yaml
term: "Carga perigosa"
definition: >
  Cargo classified in ANTT classes 1 through 6 under Resolução ANTT
  nº 5.947/2021: class 1 (explosives), class 2 (gases), class 3 (flammable
  liquids), class 4 (flammable solids), class 5 (oxidizers and peroxides),
  class 6 (toxic and infectious substances). This classification triggers
  a hard block on the standard return process (POL-001 v3.1, section 3.2)
  and requires referral to Risk Management at extension 4500.
  Hazardous cargo above 500 kg follows a specific freight table (PROC-043),
  not PROC-042.
context: >
  Appears in return eligibility checks, freight calculation, and incident
  classification. Critical trigger for RULE-002 (MUST) and RULE-007
  (MUST NOT) in the behavior rules. Any query combining "carga perigosa"
  with return/delivery terms MUST be intercepted before reaching the LLM.
do_not_confuse_with: >
  Fragile cargo, refrigerated cargo, or high-value cargo — these are
  distinct categories with different rules. "Carga perigosa" has a
  specific legal definition (ANTT classes 1–6) and is NOT a colloquial
  description of any cargo that seems risky or dangerous. Do not apply
  the hazardous cargo block to cargo that is merely valuable or delicate.
example: >
  "Cargas perigosas (classes 1–6 ANTT) NÃO são elegíveis para devolução
  pelo processo padrão. Contatar Gestão de Riscos, ramal 4500.
  Fonte: POL-001 v3.1, seção 3.2."
```

---

```yaml
term: "SLA de resolução"
definition: >
  The maximum time from ticket opening to confirmed problem resolution.
  Values by tier (general tickets): Gold = up to 24 business hours;
  Silver = up to 48 business hours; Standard = up to 72 business hours.
  Values by tier (critical incidents): Gold = up to 4 hours; Silver = up
  to 8 hours; Standard = up to 24 hours. Measured from the ticket
  opening timestamp in the ticketing system (Azure DevOps).
  SLA clock pauses outside business hours (08h–18h, business days)
  for general tickets, but does NOT pause for critical incidents of
  Gold customers.
context: >
  Appears in SLA-related queries when the customer asks about problem
  resolution time. Must be distinguished from SLA de primeira resposta
  (first response time), which is a different — and shorter — commitment.
  Source: SLA-2024 v2024.1, section 2.
do_not_confuse_with: >
  SLA de primeira resposta (first response SLA) — that is the time until
  the first acknowledgment, not until resolution. Do not conflate the two:
  Gold first response is 2h, Gold resolution is 24h — these are separate
  metrics. "Resolução" specifically means the problem is closed, not just
  acknowledged.
example: >
  "O SLA de resolução para clientes Gold em chamados gerais é de até
  24 horas úteis após abertura do chamado. Fonte: SLA-2024 v2024.1, seção 2."
```

---

```yaml
term: "SLA de primeira resposta"
definition: >
  The maximum time from ticket opening to the first contact or
  acknowledgment from the support team. This does NOT require the
  problem to be solved. Values by tier (general tickets): Gold = up to
  2 business hours; Silver = up to 4 business hours; Standard = up to
  8 business hours. Values by tier (critical incidents): Gold = up to
  30 minutes; Silver = up to 1 hour; Standard = up to 2 hours.
context: >
  Appears in response-time queries and SLA compliance checks.
  Source: SLA-2024 v2024.1, section 2.
do_not_confuse_with: >
  SLA de resolução — first response only means the team replied,
  not that the issue is resolved. Do not give a single SLA number
  without specifying which metric it is: response or resolution.
example: >
  "O SLA de primeira resposta para clientes Silver é de até 4 horas úteis
  em chamados gerais. Fonte: SLA-2024 v2024.1, seção 2."
```

---

```yaml
term: "Multiplicador regional"
definition: >
  A numeric factor applied to the base freight value to account for
  operational costs by destination region. Values differ between
  PROC-042 v1.0 and v2.0. v2.0 values (for requests opened on or
  after 01/12/2023): Norte = 1.8, Nordeste = 1.5, Centro-Oeste = 1.4,
  Sul = 1.3, Sudeste = 1.1. v1.0 values (for requests opened before
  01/12/2023, still in processing): Norte = 1.6, Nordeste = 1.4,
  Centro-Oeste = 1.3, Sul = 1.2, Sudeste = 1.0.
  Formula: Freight value = Base value × Regional multiplier × Weight factor.
context: >
  Appears in special freight calculation queries for cargo above 500 kg.
  The correct version to apply depends on the request opening date.
  Source: PROC-042 v1.0, section 2.1 and PROC-042 v2.0, section 2.1.
do_not_confuse_with: >
  A flat regional surcharge or a fee added on top of the base value.
  The multiplier is a MULTIPLIER (applied as multiplication), not a
  percentage surcharge or additive fee. Also: do NOT mix v1 and v2
  multipliers — they apply to different date ranges and produce
  incorrect quotes if switched.
example: >
  "Para frete especial com destino ao Norte em chamados abertos após
  01/12/2023, o multiplicador regional vigente é 1.8.
  Fonte: PROC-042 v2.0, seção 2.1."
```

---

```yaml
term: "Frete especial"
definition: >
  Freight modality applicable to cargo with gross weight ABOVE 500 kg.
  Calculated using the formula: Base value × Regional multiplier ×
  Weight factor. Requires an additional 3 business days for handling
  (v2.0) or 2 business days (v1.0, pre-01/12/2023 requests).
  Cargo above 5,000 kg requires prior approval from the regional
  operations manager before confirming the quote to the customer.
  Hazardous cargo above 500 kg follows PROC-043 (not PROC-042).
  Source: PROC-042 v2.0.
context: >
  Appears in all freight calculation queries. Triggers the use of
  PROC-042 (v1.0 or v2.0 depending on request date) and mandatory
  version citation per RULE-001 and RULE-003.
do_not_confuse_with: >
  Express freight (frete expresso) — which refers to a faster delivery
  modality, not a weight-based category. Standard freight (frete padrão)
  covers cargo under 500 kg and is NOT governed by PROC-042 (no formal
  document covers it in the current indexed base). Also: frete especial
  is NOT a premium or luxury service — it is a weight-based operational
  classification.
example: >
  "Frete especial aplica-se a cargas acima de 500 kg. Para destino ao
  Nordeste em chamado aberto após 01/12/2023, multiplicador regional = 1.5.
  Fonte: PROC-042 v2.0, seções 1 e 2.1."
```

---

```yaml
term: "Fator de peso"
definition: >
  A numeric multiplier applied to the freight formula based on cargo
  weight ranges. Values differ between PROC-042 v1.0 and v2.0.
  v2.0 values: 500–1,000 kg = 1.0; 1,001–3,000 kg = 1.15;
  above 3,000 kg = 1.4.
  v1.0 values: 500–1,000 kg = 1.0; 1,001–3,000 kg = 1.2;
  above 3,000 kg = 1.5.
  The correct version depends on request opening date (transition rule:
  PROC-042 v2.0, section 5).
context: >
  Appears in special freight calculation alongside the regional multiplier.
  Always cite the version when citing a weight factor value.
  Source: PROC-042 v1.0, section 2 and PROC-042 v2.0, section 2.
do_not_confuse_with: >
  A flat surcharge per kilogram or a per-unit fee. The weight factor is
  a multiplier on the entire freight formula, not a marginal cost per kg.
  Also: do not apply weight factors from v1 to v2 requests or vice versa —
  the values differ (e.g., 1,001–3,000 kg range: 1.2 in v1 vs 1.15 in v2).
example: >
  "Para carga de 2.000 kg em chamado aberto após 01/12/2023, o fator
  de peso aplicável é 1.15 (faixa 1.001–3.000 kg).
  Fonte: PROC-042 v2.0, seção 2."
```

---

```yaml
term: "Processo de devolução padrão"
definition: >
  The standard return process defined in POL-001 v3.1, applicable to
  eligible cargo returned within 7 business days of confirmed receipt.
  Steps: (1) customer opens ticket on the Portal do Cliente with CT-e
  number, photos, and reason; (2) support team triages in up to 4 business
  hours; (3) if eligible, reverse collection is scheduled within 2 business
  days; (4) refund or credit processed within 5 business days of receipt
  at the distribution center. Explicitly EXCLUDES: hazardous cargo
  (ANTT classes 1–6), refrigerated cargo with broken cold chain, and cargo
  with a violated security seal (unless documented at delivery time).
context: >
  Appears in return eligibility queries. The "padrão" (standard) qualifier
  is critical — it explicitly signals that hazardous cargo, broken
  cold-chain cargo, and seal-violated cargo are NOT covered.
  Source: POL-001 v3.1, sections 3.1 and 3.2.
do_not_confuse_with: >
  A universal return process that applies to all cargo types. The word
  "padrão" is NOT a synonym for "default for all cases" — it is a named
  process with explicit exclusions. Never apply it to hazardous cargo
  even if the customer insists or the query seems routine.
example: >
  "O processo de devolução padrão aplica-se a mercadorias elegíveis
  dentro de 7 dias úteis do recebimento. Carga perigosa NÃO é elegível.
  Fonte: POL-001 v3.1, seções 3.1 e 3.2."
```

---

```yaml
term: "Coleta reversa"
definition: >
  The physical pickup of goods from the customer's location as part of
  the approved return process. Scheduled within 2 business days after
  return approval. It is a STEP in the processo de devolução padrão,
  not a synonym for the full return process. Cost rule: if the return
  originates from a NovaTech error (wrong cargo, in-transit damage),
  reverse freight is free to the customer; if originating from customer
  withdrawal, the reverse freight cost follows the same multipliers as
  the original freight.
context: >
  Appears in return-related queries, specifically when the customer asks
  "when will you pick up the cargo?" Source: POL-001 v3.1, section 3.3.
do_not_confuse_with: >
  The full return process (processo de devolução). Coleta reversa is only
  the physical collection step — approval, triagem, and refund are separate
  steps. Also: coleta reversa is NOT available for ineligible cargo
  categories (hazardous, broken cold chain, violated seal).
example: >
  "Após aprovação da devolução, a coleta reversa é agendada em até
  2 dias úteis. Fonte: POL-001 v3.1, seção 3.3."
```

---

```yaml
term: "CT-e (Conhecimento de Transporte Eletrônico)"
definition: >
  The electronic transport document that legally accompanies every
  shipment handled by NovaTech. The CT-e number uniquely identifies
  a freight operation and is REQUIRED in all return requests opened
  through the Portal do Cliente. It is also used to calculate
  proportional refunds in partial returns (based on weight/value
  per CT-e).
context: >
  Appears in return procedures, invoice validation, and partial return
  calculations. Source: POL-001 v3.1, sections 3.3 and 3.4.
do_not_confuse_with: >
  A ticket number, order ID, tracking code, or invoice number — these
  are different identifiers. CT-e is a specific Brazilian fiscal-transport
  document. Do not use generic terms like "document number" or "transport
  document" when "CT-e" is the correct term — agents generating UI or
  prompts MUST use the exact label "CT-e (Conhecimento de Transporte
  Eletrônico)" on first occurrence.
example: >
  "Para abrir o chamado de devolução, informe o número do CT-e
  (Conhecimento de Transporte Eletrônico). Fonte: POL-001 v3.1, seção 3.3."
```

---

```yaml
term: "Incidente crítico"
definition: >
  A ticket classification at NovaTech triggering accelerated SLA clocks
  (shorter deadlines than general tickets). A ticket is classified as
  critical if at least one of the following is true: (1) cargo with
  declared value above R$ 100,000 is in unknown status for more than
  6 hours; (2) hazardous cargo has any documentation or tracking
  irregularity; (3) more than 5 tickets from the same customer in the
  last 24 hours about the same problem; (4) any situation involving risk
  to human safety. For Gold customers, the SLA clock for critical
  incidents does NOT pause outside business hours.
context: >
  Appears in ticket classification, SLA calculation, and escalation
  decisions. Source: SLA-2024 v2024.1, section 3.
do_not_confuse_with: >
  A high-priority ticket, an urgent ticket, or any ticket the customer
  considers important. "Crítico" is a formal classification with specific
  measurable criteria — not a subjective label. A customer calling their
  issue "urgent" does not make it a critical incident unless it meets the
  criteria above.
example: >
  "Carga com valor acima de R$ 100.000 em status desconhecido há mais
  de 6 horas configura incidente crítico. SLA de primeira resposta Gold:
  até 30 minutos. Fonte: SLA-2024 v2024.1, seção 3."
```

---

```yaml
term: "Dias úteis"
definition: >
  Business days: Monday through Friday, excluding national public holidays.
  Saturdays and Sundays are NEVER counted as business days at NovaTech.
  The SLA clock runs within business hours (08h–18h) for general tickets.
  Exception: the SLA clock for critical incidents of Gold customers runs
  continuously (24/7) — it does not pause on weekends or holidays.
  All deadlines in POL-001 (return: 7 days, triage: 4 hours, reverse
  collection: 2 days, refund: 5 days) are expressed in dias úteis.
context: >
  Appears in every deadline-related response: return deadlines, SLA
  commitments, freight delivery estimates. The default unit for all
  NovaTech time commitments unless explicitly stated otherwise.
  Source: POL-001 v3.1, section 3.1; SLA-2024 v2024.1, sections 2 and 5.
do_not_confuse_with: >
  Calendar days (dias corridos). NEVER substitute "dias corridos" for
  "dias úteis" unless a document explicitly uses that term (none in the
  current indexed base does). 7 calendar days ≠ 7 business days —
  confusing these two changes a customer's return window by up to 4 days.
example: >
  "O prazo de devolução é de 7 dias úteis após o recebimento confirmado.
  Sábados, domingos e feriados nacionais não são contados.
  Fonte: POL-001 v3.1, seção 3.1."
```

---

```yaml
term: "FAQ-Atendimento"
definition: >
  An informal, collaboratively maintained document created organically
  by the support team over 2 years. It is NOT validated by Compliance or
  Operations. It has no formal owner and no version control. It represents
  practical team knowledge but may contain outdated or imprecise
  information. It is indexed in the knowledge base but carries lower
  authority than normative documents (POL, PROC, SLA). When it is the
  ONLY source for a response, a disclaimer MUST be included and
  confirmation with the competent area MUST be recommended before
  relaying to the customer.
context: >
  Appears as a fallback source when POL-001, PROC-042, and SLA-2024
  do not cover a topic. Critical topics covered ONLY in FAQ:
  cargo insurance (Item 22), express freight for hazardous cargo (Item 32),
  in-transit damaged cargo (Item 38).
do_not_confuse_with: >
  An official NovaTech policy document. FAQ-Atendimento entries MUST
  NEVER be presented as "conforme a política NovaTech" or cited with the
  same authority as POL-001, PROC-042, or SLA-2024. The document's
  informal status is not a detail to omit — it changes the reliability
  of the information for the customer.
example: >
  "A única informação disponível sobre seguro de carga está no
  FAQ-Atendimento (Item 22), documento informal não validado por Compliance.
  Recomendo confirmar com o Comercial antes de repassar ao cliente."
```

---

```yaml
term: "Janela de entrega"
definition: >
  The scheduled time window within which NovaTech commits to delivering
  cargo. For frete especial (cargo above 500 kg), the delivery deadline
  is calculated as the standard route transit time PLUS an additional
  handling period: +3 business days per PROC-042 v2.0 (for requests
  opened on or after 01/12/2023) or +2 business days per PROC-042 v1.0
  (for requests opened before 01/12/2023, still in processing).
  NOTE: NovaTech documentation does not use the exact phrase "janela
  de entrega" — the equivalent terms in the indexed documents are
  "prazo de entrega" and "prazo padrão da rota". Use those exact
  phrases when citing sources.
context: >
  Appears implicitly in freight calculation queries when customers ask
  about expected delivery dates for special freight. Source:
  PROC-042 v2.0, section 3.
do_not_confuse_with: >
  A time slot for pickup or for the customer to be present to receive
  the cargo (a scheduling window). In NovaTech's context, the delivery
  deadline is the outer bound, not a specific arrival appointment.
  Also: "janela de entrega" is not the same as the SLA clock — delivery
  deadlines and support SLAs are separate commitments.
example: >
  "Para frete especial em chamado aberto após 01/12/2023, o prazo de
  entrega é o prazo padrão da rota acrescido de 3 dias úteis adicionais
  para manuseio. Fonte: PROC-042 v2.0, seção 3."
```

---

## Terms Summary Index

| Term | Type | Critical Ambiguity | Source |
|------|---------|--------------------|--------|
| Tier Gold | Customer classification | Not the metal; SLA clock ≠ pauses for critical incidents | SLA-2024 v2024.1, sec. 1–2 |
| Tier Silver | Customer classification | Not between Gold and Standard; no dedicated manager | SLA-2024 v2024.1, sec. 1–2 |
| Tier Standard | Customer classification | Not "inferior service"; only 3 tiers exist | SLA-2024 v2024.1, sec. 1–2 |
| Carga perigosa | Cargo classification | Legal definition (ANTT classes 1–6), not colloquial; hard return block | POL-001 v3.1, sec. 3.2 |
| SLA de resolução | SLA metric | Distinct from SLA de primeira resposta (shorter deadline) | SLA-2024 v2024.1, sec. 2 |
| SLA de primeira resposta | SLA metric | Acknowledgment only — does NOT mean problem is solved | SLA-2024 v2024.1, sec. 2 |
| Multiplicador regional | Freight factor | Multiplier (not surcharge); two different value sets (v1 vs v2) | PROC-042 v2.0, sec. 2.1 |
| Frete especial | Freight modality | Weight-based (>500 kg), not premium/express | PROC-042 v2.0, sec. 1 |
| Fator de peso | Freight factor | Multiplier (not per-kg fee); v1 and v2 values differ | PROC-042 v2.0, sec. 2 |
| Processo de devolução padrão | Return process | Explicit exclusions: hazardous, cold-chain, violated-seal cargo | POL-001 v3.1, sec. 3.1–3.2 |
| Coleta reversa | Return step | One step in the return process — not the full return | POL-001 v3.1, sec. 3.3 |
| CT-e | Document identifier | Not a ticket/order/tracking number; required for all returns | POL-001 v3.1, sec. 3.3 |
| Incidente crítico | Ticket classification | Formal criteria-based — not synonymous with "urgent" | SLA-2024 v2024.1, sec. 3 |
| Dias úteis | Time unit | Business days (Mon–Fri) — NEVER calendar days (dias corridos) | POL-001 v3.1, sec. 3.1 |
| FAQ-Atendimento | Document type | Informal, unvalidated — NOT official policy documentation | FAQ-Atendimento header |
| Janela de entrega | Delivery concept | Not a specific appointment slot; equivalent to "prazo de entrega" | PROC-042 v2.0, sec. 3 |

---

*Glossary authored by Domain Specialist — Exercise 2.3. Based on Anexo A: POL-001 v3.1, PROC-042 v1.0, PROC-042 v2.0, SLA-2024 v2024.1, and FAQ-Atendimento. Validated against all documented contradictions and gaps identified in Anexo A meta-notes.*
