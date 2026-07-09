# GDPR → Product Feature Mapping

This is the product-relevant slice of GDPR. For each obligation, one or more product capabilities a compliance SaaS typically offers. Use to scaffold backlog items quickly.

**Convention:** The "addressee" column indicates who bears the obligation — our customer (data controller / processor). Features are for **them**, not for us.

## Article-level Mapping

| Article | Obligation | Addressee | Product Capability |
|---|---|---|---|
| Art. 5 | Principles (lawfulness, fairness, transparency, purpose limitation, minimization, accuracy, storage limitation, integrity/confidentiality, accountability) | Controller | Privacy policy generator; purpose/retention fields on data types; evidence log |
| Art. 6 | Lawful basis for processing | Controller | Legitimate-interest assessment (LIA) wizard; consent/contract/legal-obligation basis selector per processing activity |
| Art. 7 | Conditions for consent | Controller | Consent management platform; granular per-purpose toggles; withdrawal flow; consent log |
| Art. 8 | Child's consent for information society services | Controller | Age gate component; parental consent flow |
| Art. 9 | Special categories of data | Controller | Sensitive-data flag; explicit-consent capture; DPIA trigger |
| Art. 10 | Criminal convictions data | Controller | Restricted-data flag; legal-basis validator |
| Art. 12-14 | Transparency / information duties | Controller | Privacy policy generator; just-in-time notices; layered notices UI |
| Art. 15 | Right of access (DSAR) | Controller | DSAR intake form; identity verification; data collection workflow; response pack generator; 1-month SLA timer |
| Art. 16 | Right to rectification | Controller | Rectification request flow; update propagation to processors |
| Art. 17 | Right to erasure / be forgotten | Controller | Erasure request flow; retention-override log; third-party erasure propagation |
| Art. 18 | Right to restriction | Controller | Restriction flag on records; re-enable workflow |
| Art. 19 | Notification obligation | Controller | Processor notification routing |
| Art. 20 | Right to portability | Controller | Export in structured, commonly-used, machine-readable format |
| Art. 21 | Right to object | Controller | Object workflow; LIA re-assessment trigger |
| Art. 22 | Automated decision-making | Controller | AI decision inventory; human-review flag; explanation template |
| Art. 24 | Responsibility of controller | Controller | Accountability evidence log; governance dashboard |
| Art. 25 | Data protection by design/default | Controller | Design-review checklist; default-privacy settings audit |
| Art. 26 | Joint controllers | Controller | Joint-controllership agreement template |
| **Art. 28** | **Processor contracts** | Controller + Processor | **DPA generator**; **sub-processor registry**; **DPA change notification** |
| Art. 29 | Processing under authority | Processor | Instruction log |
| **Art. 30** | **Records of processing activities (RoPA)** | Controller + Processor | **RoPA builder**; **export as PDF/CSV for authority audit** |
| Art. 32 | Security of processing | Controller + Processor | Technical & organizational measures (TOM) registry; encryption/backup/access-control evidence |
| **Art. 33** | **Breach notification to supervisory authority (72h)** | Controller | **Incident workflow**; **72-hour countdown**; **notification template per DPA** |
| **Art. 34** | **Communication to data subjects** | Controller | **Affected-individual notification generator**; **risk assessment** |
| **Art. 35** | **DPIA** | Controller | **DPIA wizard**; **risk threshold detector (Art. 35(3) list + national supplements)**; **residual-risk gate** |
| Art. 36 | Prior consultation with authority | Controller | Consultation request template; supporting-documents bundle |
| Art. 37-39 | DPO role | Controller | DPO record; responsibilities checklist; communication log |
| **Art. 44-50** | **International transfers** | Controller + Processor | **SCC template library (modules 1-4)**; **transfer impact assessment (TIA) wizard**; **adequacy decision lookup**; **BCR registry** |

## Feature Bundles (typical customer-facing surface)

Reduce product surface area by grouping:

| Bundle | GDPR Articles | Feature Set |
|---|---|---|
| **Privacy Policy Suite** | 12-14, 5 | Generator, version history, language variants, consent-ingestion log |
| **Consent Manager** | 6(1)(a), 7, 8, 21 | Preference center, log, withdrawal, re-consent campaign |
| **RoPA & Accountability** | 24, 30 | Activity registry, TOM register, evidence vault, controller/processor role flag |
| **DSAR Hub** | 12(3), 15-22 | Intake, identity verification, routing, response pack, SLA timer |
| **DPIA Workspace** | 25, 35, 36 | Screening questionnaire, threshold detector, risk matrix, consultation template |
| **Breach Response** | 33, 34 | Incident intake, 72h timer, DPA template library, affected-user comms |
| **Vendor / Processor Ops** | 28, 29, 32 | Sub-processor registry, DPA generator, change notifications, TOM library |
| **International Transfers** | 44-50 | SCC builder, TIA wizard, BCR registry, adequacy lookup |
| **DPO Console** | 37-39 | DPO profile, workflow assignments, activity log |

## Priority Signal Heuristics

| Obligation type | Default `regulatory_urgency` |
|---|---|
| 72-hour countdown (breach) | HIGH — customers cannot miss it |
| 1-month SLA (DSAR) | HIGH — defaults to denial-by-inaction → fine risk |
| Evidence-on-demand (RoPA, DPIA) | MEDIUM-HIGH — fine risk on audit |
| Principle-level (Art. 5) | MEDIUM — broad but interpretable |
| Documentation-only (TOM registry) | MEDIUM — required but not live-timer |
| Optional but recommended (BCR over SCC) | LOW — nice to have |

## Cross-references

- For **DPIA procedure depth**, delegate to `legal-os:dpo` (they have `dpia-template.md`)
- For **breach timeline specifics**, delegate to `legal-os:dpo` (`breach-timeline.md`)
- For **contract clause mechanics**, delegate to `legal-os:contract-manager`
- For **jurisdictional variation** (national DPA specifics, Art. 35(3) national lists), hand off to `localization-po`

## Personas That Usually Drive GDPR Items

From typical ObligoBoard profile (verify against live config):
- **DPO** — owns most articles; Art. 30, 33, 35, 37-39 specifically
- **SME Founder / non-lawyer controller** — needs guided flows over raw legal controls (Art. 6, 7, 12-14, 30)
- **Fractional CCO / external DPO** — multi-tenant cases, batch ops, delegated authority
