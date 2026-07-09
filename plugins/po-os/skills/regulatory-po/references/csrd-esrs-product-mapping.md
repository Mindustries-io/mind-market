# CSRD / ESRS → Product Feature Mapping

Corporate Sustainability Reporting Directive (Directive (EU) 2022/2464) and the European Sustainability Reporting Standards (ESRS) adopted under it. CSRD obliges reporting; ESRS defines **what** to disclose.

## Scope Summary

Reporting obligations phase in by company size (as of time of writing — verify current transition schedule with `legal-os:regulatory-intel` before generating items):

- **Wave 1 (FY 2024, reports in 2025):** Large Public Interest Entities (PIEs) already under NFRD
- **Wave 2 (FY 2025, reports in 2026):** Other large undertakings
- **Wave 3 (FY 2026, reports in 2027):** Listed SMEs (simplified standard)
- **Wave 4 (FY 2028):** Non-EU groups with EU presence

**Who our customers are matters.** If the product serves mostly small unlisted companies, CSRD is a *future* obligation for many, not current. Use this to temper urgency: Wave 3 SMEs need the tool in 2026; earlier waves need it now.

## ESRS Structure

| Pillar | Standards | Topic |
|---|---|---|
| Cross-cutting | ESRS 1 | General requirements |
| Cross-cutting | ESRS 2 | General disclosures |
| Environment | ESRS E1 | Climate change |
| Environment | ESRS E2 | Pollution |
| Environment | ESRS E3 | Water & marine |
| Environment | ESRS E4 | Biodiversity & ecosystems |
| Environment | ESRS E5 | Resource use & circular economy |
| Social | ESRS S1 | Own workforce |
| Social | ESRS S2 | Workers in value chain |
| Social | ESRS S3 | Affected communities |
| Social | ESRS S4 | Consumers & end-users |
| Governance | ESRS G1 | Business conduct |

## Standard → Product Capability

| Standard | Disclosure Highlight | Product Capability |
|---|---|---|
| **ESRS 1** | Double materiality assessment (DMA) methodology | DMA workflow; stakeholder-inventory form; impact-and-financial-materiality scoring matrix; audit trail |
| **ESRS 2** | Governance, strategy, risk management (GOV / SBM / IRO) disclosures | Governance-structure builder; material IRO (impact/risk/opportunity) register; cross-reference to ESRS topics |
| **ESRS E1** | Transition plan, GHG emissions Scopes 1/2/3, energy mix | Emissions calculator with activity-data inputs; Scope 3 category selector; target tracker |
| **ESRS E2** | Pollutants, substances of concern, microplastics | Pollutant registry per site; release-to-air/water/soil tracker |
| **ESRS E3** | Water withdrawal, consumption, discharge; water-stressed area flag | Water-usage tracker; water-stress-area lookup |
| **ESRS E4** | Biodiversity-sensitive areas, deforestation | Site-to-biodiversity-area mapping; deforestation screening |
| **ESRS E5** | Resource inflows, outflows, circularity indicators | Material-flow tracker; recycled-content ratio calculator |
| **ESRS S1** | Own workforce composition, pay gap, health & safety, training | HR-data ingestion; pay-gap calculator; incident log |
| **ESRS S2** | Value-chain worker conditions, child labour, forced labour | Supplier screening questionnaire; remediation case log |
| **ESRS S3** | Communities impacted by operations | Stakeholder-engagement register; grievance log |
| **ESRS S4** | Product & service impacts on consumers | Complaint/incident ingestion; accessibility compliance |
| **ESRS G1** | Business conduct, corruption, political influence, supplier relationships | Code-of-conduct attestation; lobbying register; supplier-payment-terms tracker |

## Cross-Standard Features (most reused)

1. **Double-materiality assessment (DMA) engine** — central to ESRS 1 + 2; assesses impact materiality and financial materiality for each potential topic. This is the #1 onboarding workflow for any CSRD SaaS.
2. **Topic scoping by materiality** — only material topics must be disclosed in depth; the product must hide/show standards based on DMA output.
3. **XBRL tagging** — reports must be tagged in digital format (iXBRL with ESRS XBRL taxonomy). Product needs tagging UI + export validator.
4. **Data lineage & audit** — assurance providers (limited then reasonable) will audit. Every disclosed number must trace to a source.
5. **Cross-referencing** — ESRS 2 requires pointers across standards. Product needs link/reference data model.
6. **Phased maturity** — transition reliefs allow some disclosures to be deferred early years. Product should support "applying relief X" flag and generate the mandated transition narrative.

## Priority Signal Heuristics

| Situation | Default `regulatory_urgency` |
|---|---|
| Customer is Wave 1/2, no tool in hand | HIGH |
| Customer is Wave 3 (SMEs), 2026 report | MEDIUM |
| Customer has voluntary reporter ambition | LOW-MEDIUM |
| Standard-specific disclosure for material topic | scales with DMA — if customer has flagged material, HIGH; if not material, LOW |

## Non-obvious Product Considerations

- **Assurance readiness** is a differentiator. Features that produce auditor-ready packs (evidence, lineage, reconciliation) win.
- **Voluntary SME adoption:** many non-mandatory SMEs will adopt a simplified version to win customers/financing. Voluntary SME standard (VSME) is being developed — track separately.
- **Sector-specific standards** are coming — monitor EFRAG for adoption timeline; modular product > monolithic.
- **Overlap with CBAM / SFDR / Taxonomy** — customers often want one place to manage all sustainability reporting. Plan data-model unification.

## Cross-references

- For **legal interpretation depth**, delegate to `legal-os:legal-researcher`
- For **enforcement signal**, delegate to `legal-os:regulatory-intel`
- For **market-by-market transposition differences**, hand off to `localization-po` (CSRD requires national transposition; timing and interpretations vary)

## Source Pointers

When researching: EFRAG (standards), European Commission (Directive), national authorities for transposition, CDP / GRI / ISSB for adjacent frameworks the product may interoperate with.
