# Market-Entry Localization Checklist

Use this checklist when the user asks "can we sell into {market}?" or "what do we need for {market}?". Feeds directly into a Localization Brief output.

## A. Language & Copy

- [ ] **Product UI strings** — translated to local language (including micro-copy, error messages, tooltips)
- [ ] **Legal documents** — privacy policy, cookie notice, T&Cs translated by native speaker, not machine-only
- [ ] **Email templates** — transactional (welcome, password reset, receipts) and transactional-legal (breach notification, DSAR response, cookie consent confirmation)
- [ ] **Customer-facing alerts** — consent banners, just-in-time notices
- [ ] **Support content** — help center, FAQs, chatbot in local language
- [ ] **Marketing pages** — distinct from product but shape first impression
- [ ] **Onboarding** — first-run experience, empty states
- [ ] **Pluralization, gender, formal/informal register** — matters more in some languages (DE Sie/du, IT Lei/tu, FR vous/tu) than others

## B. Format & Locale

- [ ] **Date format** (DD/MM/YYYY vs MM/DD/YYYY vs YYYY-MM-DD)
- [ ] **Time format** (24h vs 12h, timezone display)
- [ ] **Number format** (1,000.00 vs 1.000,00)
- [ ] **Currency** (EUR, GBP, CHF, TRY, USD — symbol position, decimal separators)
- [ ] **Address format** (country-specific postal format)
- [ ] **Phone format** (country code + national pattern)
- [ ] **Tax ID format** (VAT number format, Italian Codice Fiscale, Spanish NIF, etc.)
- [ ] **Name format** (given/family order, use of middle names, maternal/paternal surnames in ES/PT)
- [ ] **Week start** (Sunday vs Monday)

## C. Legal Overlays

- [ ] **National DPA** identified; breach notification path tested
- [ ] **National DPIA list** (Art. 35(3)) applied to screening logic
- [ ] **Employment-context rules** (e.g., IT Workers' Statute, DE BDSG §26, FR Code du travail)
- [ ] **Sector rules** (e.g., FR CNIL référentiels, ES LOPDGDD digital rights)
- [ ] **Children's age of consent** (13-16 per country — GDPR Art. 8(1))
- [ ] **Cookie law transposition** (ePrivacy national variant — UK PECR, DE TTDSG, IT Provvedimento cookie 2021, etc.)
- [ ] **Marketing consent rules** (opt-in vs. soft opt-in — some countries allow soft opt-in for existing customers)
- [ ] **Retention periods** (national tax/accounting rules overlay GDPR minimization)
- [ ] **Medical / financial / telecom sector additions** if applicable

## D. Infrastructure / Data Residency

- [ ] **Data residency expectations** — some customers / sectors require in-country or EU-only residency
- [ ] **International transfer mechanism** — SCCs + TIA for non-adequate destinations; UK-IDTA for UK transfers
- [ ] **Sub-processor list** — localized / localized addendum
- [ ] **CDN / edge location** — relevance to residency claims
- [ ] **Backup location**

## E. Payment & Tax

- [ ] **Local payment methods** — SEPA direct debit (EU), Bancontact (BE), iDEAL (NL), Sofort (DE), SOFORT (AT), Przelewy24 (PL), Multibanco (PT), Klarna (SE), Bankia/BBVA rails (ES)
- [ ] **Tax handling** — VAT MOSS / OSS for B2C; reverse charge for B2B; country-specific invoicing rules
- [ ] **Invoice format** — IT electronic invoicing (FatturaPA), ES eInvoicing, DE GoBD compliance, FR Facture-X
- [ ] **Consumer cancellation rights** — 14-day EU right; longer in some jurisdictions

## F. Accessibility

- [ ] **EN 301 549** / WCAG 2.1 AA baseline
- [ ] **Country-specific overlays:**
  - Italy: Legge Stanca (L. 4/2004) + AgID guidelines for public-sector-adjacent
  - Germany: BITV 2.0 (public), BFSG (private sector from 2025)
  - France: RGAA
  - UK: Public Sector Bodies Accessibility Regulations 2018
  - Spain: Real Decreto 1112/2018
  - Netherlands: DigiToegankelijk

## G. Support & Operations

- [ ] **Local business hours** — support during local timezone
- [ ] **Local language support** — humans or at minimum LLM+disclosed
- [ ] **Response-time SLAs** — may be regulated for certain sectors
- [ ] **Local entity** — required for B2B enterprise sales in some countries (e.g., public-sector procurement)
- [ ] **Local bank account** — SEPA reduces need but enterprise customers sometimes require

## H. Reputation & Trust Signals

- [ ] **Local customer testimonials / case studies**
- [ ] **Local press / analyst mentions**
- [ ] **Trust seals** that resonate locally:
  - Germany: Trusted Shops, TÜV Datenschutz
  - France: LabelSite, e-confiance
  - Italy: Bollino of quality, ISO certifications stand in well
  - UK: Cyber Essentials, ISO 27001
- [ ] **DPA membership / certification** (if available under Art. 42)

## Scoring the Market-Readiness

Compute readiness per section. Suggested rubric (per section):

- 0 pts = nothing in place
- 1 pt = partial
- 2 pts = fully localized

Total per section (max 16 pts). Per-market summary:

| Score | Recommendation |
|---|---|
| ≥ 12 | **GO** — ready to launch; minor gaps to track |
| 8-11 | **CONDITIONAL** — launch with caveats; prioritized gap list |
| < 8 | **NO-GO** — focus on closing gaps first |

## Output

Feed this into the Localization Brief format (see `${CLAUDE_PLUGIN_ROOT}/skills/lead-po/references/output-formats.md`). Derive Backlog Items from each unchecked box — one backlog item per unchecked item, with priority driven by:

- Is it a **legal blocker** (e.g., missing local DPA breach path)? → P0
- Is it a **trust blocker** (no local language) for B2B? → P1
- Is it a **conversion drag** (format issues, payment methods)? → P2
- Is it a **polish item** (micro-copy tone)? → P3
