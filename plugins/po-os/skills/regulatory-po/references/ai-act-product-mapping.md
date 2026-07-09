# EU AI Act → Product Feature Mapping

Regulation (EU) 2024/1689. The AI Act is a **product-liability + obligations** regulation on AI systems and general-purpose AI (GPAI) models. Our customers are typically AI **deployers** and sometimes **providers**; our product must help them meet their specific role's duties.

## Phased Application (verify current dates with `legal-os:regulatory-intel`)

- **Prohibitions (Chapter II):** in force early, ~6 months after EIF
- **GPAI rules (Chapter V):** ~12 months after EIF
- **Most obligations (Chapters III-IV, including high-risk):** ~24 months after EIF
- **High-risk Annex II systems (regulated product safety):** ~36 months after EIF

Effective dates drive urgency signals. When a date is imminent, urgency is HIGH; when > 12 months away, MEDIUM; > 24 months, LOW-MEDIUM with customer-readiness rationale.

## Actor Classification (foundational for product scoping)

| Actor | Definition | Primary Duties |
|---|---|---|
| **Provider** | Develops or places on the market | Art. 16 (full obligations for high-risk); Art. 50 transparency; Art. 53+ GPAI |
| **Deployer** | Uses an AI system under its authority | Art. 26 (high-risk deployer duties); Art. 50 transparency |
| **Importer / Distributor** | Markets in EU from third-country / re-sells | Arts. 23, 24 — lighter, chain-of-custody |
| **Affected person** | Subject of an AI output | Rights under Art. 86 (explanation) |

Our customer's role dictates which features they need. The product must ask up-front: "For which AI system, in which role?" and then show only relevant obligations.

## Risk Tier → Obligations

| Tier | Examples | Product Capability |
|---|---|---|
| **Prohibited (Art. 5)** | Social scoring, real-time biometric identification (narrow exceptions), emotion recognition at work/school, untargeted facial image scraping | Prohibited-practice screener; block/alert feature |
| **High-risk (Annex III)** | HR/hiring, credit scoring, education access, biometric categorization, critical infrastructure, law enforcement, migration, justice | Full compliance workstream: Art. 9 risk mgmt, Art. 10 data gov, Art. 11 tech docs, Art. 12 logging, Art. 13 transparency, Art. 14 human oversight, Art. 15 accuracy/robustness/cybersecurity, Art. 17 QMS, Art. 43 conformity assessment |
| **Limited risk (Art. 50)** | Chatbots, generative content, deepfakes, emotion recognition | Transparency UI patterns; watermark / labelling tools; end-user disclosure generator |
| **Minimal risk** | All other AI | Voluntary codes of conduct |

## High-Risk Article → Feature

| Article | Obligation | Feature |
|---|---|---|
| **Art. 9** | Risk management system — ongoing, documented | Risk register per AI system; foreseeable-misuse matrix; mitigation log with dates |
| **Art. 10** | Data governance (training/validation/testing) | Dataset registry; quality criteria checklist; bias monitoring; data-lineage record |
| **Art. 11 + Annex IV** | Technical documentation | Tech-doc builder with Annex IV sections; version control; export PDF |
| **Art. 12** | Automatic logging | Log-ingestion API; retention config; subject-correlation ID |
| **Art. 13** | Transparency to deployer | Instructions-for-use template; limitations/accuracy statement |
| **Art. 14** | Human oversight | Oversight-plan template; role assignment; override log |
| **Art. 15** | Accuracy, robustness, cybersecurity | Metric tracker (accuracy/precision/recall by segment); adversarial-robustness record; security TOM record |
| **Art. 16-21** | Provider-specific (QMS, reporting, registration) | QMS evidence vault; post-market monitoring log; serious-incident reporter; EU-database registration flow |
| **Art. 26** | Deployer duties (task assignment, monitoring, FRIA where applicable) | Deployer intake; FRIA wizard; monitoring log |
| **Art. 27** | Fundamental Rights Impact Assessment (FRIA) for public-service deployers | FRIA workflow; rights-impact matrix |

## GPAI (Chapter V)

| Article | Obligation | Feature |
|---|---|---|
| **Art. 53** | Tech docs, downstream info, copyright/training-data policy | GPAI doc-pack builder; training-data summary generator |
| **Art. 55** | GPAI with systemic risk: model eval, adversarial testing, incident reporting, cybersecurity | Systemic-risk threshold assessor; eval registry; red-team log; incident reporter |

## Transparency (Art. 50) — most broadly applicable

Every generator/chatbot/deepfake/emotion-recognition deployer must tell users. Feature set:

- End-user disclosure component (label "You are interacting with AI")
- Machine-readable watermark for synthetic content (image, audio, video, text)
- Detection / provenance-metadata export
- Localized copy library for disclosures

This is the **highest-frequency** AI-Act feature need — almost every customer using generative AI has Art. 50 duties. Prioritize accordingly.

## Cross-Cutting Features

1. **AI Inventory** — customers must know which AI systems they have, their role, and risk tier. Core onboarding.
2. **Conformity Assessment Workflow** — for high-risk, the pathway (self-assessment via QMS vs. notified body) depends on Annex. Product routes the customer.
3. **Post-Market Monitoring** — continuous obligation; product must support recurring check-ins.
4. **Serious Incident Reporting (Art. 73)** — deadline-driven (15 days for serious, 2 days for widespread infringement / critical-infra), so we need a timer feature similar to GDPR Art. 33.
5. **EU Database Registration** — providers of high-risk systems must register. Product can pre-fill the registration package.

## Priority Signal Heuristics

| Situation | Default `regulatory_urgency` |
|---|---|
| Prohibition applies to customer's current practice | P0 — immediate cease or feature block |
| High-risk conformity-assessment deadline < 3 months | P0 |
| Art. 50 transparency gap on live customer product | P1 |
| GPAI systemic-risk eval needed (customer is provider) | P1-P2 depending on deadline |
| FRIA for public-service deployer | P1 |
| Post-market monitoring cadence | P2 |
| Voluntary code-of-conduct adoption | P3 |

## Non-obvious Product Considerations

- **Standardisation references:** harmonised standards (EN) are being drafted. When a standard lands, the product should incorporate its checklist — monitor CEN-CENELEC.
- **National competent authority variation:** member states designate authorities. `localization-po` owns tracking.
- **Interface with GDPR:** Art. 26(1) requires data protection law compliance too. AI Act items often need a sibling DPIA backlog item — tag them as dependencies.
- **SME / micro-enterprise reliefs:** simplified technical-documentation format and voluntary codes. If customer base is SME-heavy, these reliefs are high-value features.

## Cross-references

- For **legal interpretation** of ambiguous provisions (e.g., "significant risk of harm" thresholds), delegate to `legal-os:legal-researcher`
- For **standard-setting updates** (CEN-CENELEC, ISO/IEC), delegate to `legal-os:regulatory-intel`
- For **jurisdictional variation** (national competent authority designations, incident-reporting single-point-of-contact per country), hand off to `localization-po`

## Personas That Usually Drive AI Act Items

- **AI/ML lead** — technical documentation, risk management, model evaluation
- **Compliance/DPO** — FRIA, post-market monitoring, incident reporting
- **Legal counsel** — conformity assessment pathway choice, GPAI classification
- **Business owner** — AI inventory ownership, deployer obligation fulfilment
