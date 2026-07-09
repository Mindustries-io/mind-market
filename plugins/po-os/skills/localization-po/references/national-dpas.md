# National DPA Profiles (Product-Relevant)

Each entry captures what a product team needs to know to localize for the market. Not a legal treatise — a product-focused cheat sheet. For deep legal analysis, delegate to `legal-os:legal-researcher`.

Keep in mind: **these profiles age.** Cross-check with a `WebSearch` or delegate to `legal-os:regulatory-intel` before acting on enforcement-priority claims.

---

## 🇮🇹 Italy — Garante per la protezione dei dati personali

- **Short name:** Garante
- **URL:** https://www.garanteprivacy.it
- **Notification portal:** https://servizi.gpdp.it/databreach/s/
- **Language-of-record:** Italian (all formal filings, complaint responses)
- **Privacy policy language:** Italian required for Italian-facing services; multilingual policies fine as long as Italian version exists

**Known enforcement priorities (verify currency):**
- Cookie walls and non-compliant consent banners (Linee guida cookie 2021)
- Worker monitoring (GDPR + Italian Workers' Statute Art. 4)
- Dark patterns in consent UX
- Ad-tech and real-time bidding
- Telemarketing (Registro Pubblico delle Opposizioni)

**Product quirks that matter:**
- **Cookie banner specifics:** scroll-to-consent is not valid; "accept all" equally prominent as "reject all"; granular purposes; no pre-ticked boxes
- **Worker context:** consent is rarely a lawful basis for processing employee data (power imbalance). Features that rely on employee "consent" need a second lawful basis.
- **DPO designation:** stricter than some member states on when mandatory
- **Breach notification language:** preferably Italian; English accepted with translation if urgent
- **Minors' age of consent:** 14 (GDPR Art. 8(1) allows 13-16)

**Product features that win trust with Italian customers:**
- Italian-language privacy-policy generator (not machine-translated)
- Cookie banner template that passes Garante's Linee guida
- Employee-context warning when the user selects "consent" as lawful basis for HR processing
- Breach template pre-translated into Italian

---

## 🇫🇷 France — Commission Nationale de l'Informatique et des Libertés

- **Short name:** CNIL
- **URL:** https://www.cnil.fr
- **Notification portal:** https://www.cnil.fr/fr/notifier-une-violation-de-donnees-personnelles
- **Language-of-record:** French
- **Privacy policy language:** French required for French-facing services

**Known enforcement priorities:**
- Cookies and trackers (CNIL has been the most aggressive in EU on cookie enforcement)
- Google Analytics / US transfers (multiple formal notices)
- Commercial prospecting and telemarketing
- Biometric data in workplaces
- Health data

**Product quirks:**
- **Référentiels:** CNIL publishes sector-specific reference guides (HR, health, CRM, connected vehicles). Product features should mirror the referential for that sector when customer selects it.
- **Règlement des règles professionnelles (deontological codes):** some sectors (lawyers, healthcare) have additional layers
- **Strict interpretation of "prior consent"** for non-essential cookies — scroll-to-accept is invalid; banner must be equally easy to reject
- **ADR / class-action readiness** — recent increase in litigation; evidence-export features become table stakes

**Product features that win trust with French customers:**
- French-language everything (policies, emails, notifications, in-product copy)
- Référentiel-aligned templates (HR, CRM, health, marketing)
- Analytics integration options that are French-data-residency-friendly (Matomo, Piwik Pro, AT Internet)

---

## 🇬🇧 United Kingdom — Information Commissioner's Office

- **Short name:** ICO
- **URL:** https://ico.org.uk
- **Notification portal:** https://ico.org.uk/for-organisations/report-a-breach/
- **Language-of-record:** English (Welsh also official)
- **Privacy policy language:** English

**Framework:** UK GDPR + Data Protection Act 2018 + PECR (Privacy and Electronic Communications Regulations)

**Known enforcement priorities:**
- Nuisance marketing calls (PECR fines)
- Child safety online (Age Appropriate Design Code)
- AI / ADM transparency
- Public-sector data use

**Product quirks:**
- **PECR** is the UK cookie + electronic-marketing law. It's related to EU ePrivacy but diverges on some fine details — product cookie-banner templates need a UK variant.
- **Age Appropriate Design Code:** if product could be accessed by children, 15 standards apply. This is much stricter than GDPR Art. 8 alone.
- **ICO is pragmatic:** more guidance-heavy, less fine-heavy than CNIL. But fines for nuisance marketing are routine and large.
- **Post-Brexit divergence** is ongoing — watch Data Protection and Digital Information Bill progression
- **International transfers:** UK has its own adequacy regime and UK-IDTA / UK-addendum for SCCs

**Product features that win trust with UK customers:**
- PECR-compliant cookie templates (separate from generic EU)
- ICO-guide-aligned DPIA templates
- UK-IDTA generation for transfer cases
- Age-appropriate-design checklist when child data is in play

---

## 🇩🇪 Germany — Federal + 16 State DPAs

- **Federal:** Bundesbeauftragter für den Datenschutz und die Informationsfreiheit (BfDI) — narrow competence (federal agencies, telecom, postal). Most private-sector cases go to state DPAs.
- **State DPAs:** One per Land (LfDI/LDI by state). Key ones for private sector:
  - Baden-Württemberg (LfDI BW) — https://www.baden-wuerttemberg.datenschutz.de
  - Bavaria (BayLDA) — https://www.lda.bayern.de
  - Berlin (BlnBDI) — https://www.datenschutz-berlin.de
  - Hamburg (HmbBfDI) — https://datenschutz-hamburg.de
  - NRW (LDI NRW) — https://www.ldi.nrw.de

- **Language-of-record:** German
- **Privacy policy language:** German required for German-facing services; bilingual acceptable

**Known enforcement priorities:**
- Employee data protection (BDSG §26 / §88)
- Google Analytics / US transfers (BayLDA was first to fine)
- Cookie banners (state DPAs coordinate via DSK — Datenschutzkonferenz)
- Health and social data

**Product quirks:**
- **16 DPAs means 16 opinions.** DSK guidance papers try to harmonize but state-level decisions can diverge. When a customer is in a specific Land, their primary DPA matters.
- **BDSG — federal supplementary act** — has specifics beyond GDPR, especially for employment (§26), research, and video surveillance
- **Works-council consent doctrine** — for HR features, works-council involvement is often required. Product should surface this.
- **Cookie-Einwilligung:** Germany transposed ePrivacy strictly; TTDSG (Telekommunikation-Telemedien-Datenschutz-Gesetz) governs cookies — stricter than some countries

**Product features that win trust with German customers:**
- German-language everything, AGB-style tone (formal "Sie")
- Works-council notice / approval workflow for HR features
- Separate cookie/TTDSG-conforming templates
- State-DPA selector for breach notification

---

## 🇪🇸 Spain — Agencia Española de Protección de Datos

- **Short name:** AEPD
- **URL:** https://www.aepd.es
- **Notification portal:** https://sedeagpd.gob.es/
- **Language-of-record:** Spanish (Catalan/Basque/Galician also valid in autonomous communities)
- **Privacy policy language:** Spanish

**Framework:** GDPR + LOPDGDD (Organic Law 3/2018)

**Known enforcement priorities:**
- Cookies and consent (AEPD guide on cookies)
- Biometric time-attendance
- Credit-bureau data (Fichero Asnef)
- Video surveillance

**Product quirks:**
- **AEPD has published many practical guides** (cookies, video surveillance, AI). Product-facing templates should align with these guides.
- **DPO registration:** AEPD requires notification of DPO designation
- **Additional digital rights** (LOPDGDD): "right to digital disconnect" (Art. 88), "right to digital will" (Art. 96), "right to privacy in the workplace" (Art. 87-91). These are Spanish additions on top of GDPR.

**Product features that win trust with Spanish customers:**
- Spanish-language everything (+ optional Catalan/Basque/Galician for autonomous communities if customer requests)
- AEPD-cookie-guide-aligned banner template
- Digital-rights workflows specific to LOPDGDD
- DPO-registration-package generator for AEPD

---

## 🇳🇱 Netherlands — Autoriteit Persoonsgegevens

- **Short name:** AP
- **URL:** https://autoriteitpersoonsgegevens.nl
- **Notification portal:** https://autoriteitpersoonsgegevens.nl/nl/meldingen
- **Language-of-record:** Dutch
- **Privacy policy language:** Dutch for Dutch-facing services; English acceptable with clear disclosure

**Known enforcement priorities:**
- Cookie walls (AP has been strict — cookie walls are disallowed)
- Health data
- Tax authority incidents (high-profile past cases)

**Product features that win trust with Dutch customers:**
- Dutch-language everything
- AP-compliant cookie templates (no cookie walls)
- Strong DPIA workflow (NL has an Art. 35(3) list)

---

## 🇮🇪 Ireland — Data Protection Commission

- **Short name:** DPC
- **URL:** https://www.dataprotection.ie
- **Language-of-record:** English
- **Privacy policy language:** English

**Special role:** The DPC is the lead supervisory authority for most US big-tech EU establishments (Meta, Google EMEA, Apple, Microsoft have Irish HQs). For SaaS selling into Ireland, DPC expectations are relatively aligned with EDPB.

**Product quirks:**
- Fewer country-specific quirks than Italy/Germany/France
- DPC "pre-investigation engagement" process is a template for cooperation-mode
- DPC fines are large but few (cross-border cases)

---

## 🇨🇭 Switzerland — Federal Data Protection and Information Commissioner

- **Short name:** FDPIC (Edöb in German)
- **URL:** https://www.edoeb.admin.ch
- **Language-of-record:** German / French / Italian (official Swiss languages)

**Framework:** Revised nFADP (Federal Act on Data Protection) — effective 1 September 2023, not GDPR but close

**Key differences from GDPR:**
- No direct GDPR application, but adequacy decision from EU exists
- No 72-hour breach notification timeline — "as soon as possible"
- DPO is advisor role (Datenschutzberater); not mandatory but recommended
- Register of processing (different fields than GDPR Art. 30)
- Personal-data-breach notification to FDPIC required "as soon as possible" with fewer rigid forms
- Data subject rights similar to GDPR but with minor differences
- Fines go to individuals (up to CHF 250k) — uncommon model

**Product quirks:**
- **Adequacy means EU SCCs generally work** for EU→CH transfers
- **Swiss-specific privacy policy template** beats generic "GDPR + Swiss note"
- **Three-language support** often required (DE, FR, IT)

---

## 🇹🇷 Turkey — Kişisel Verileri Koruma Kurumu

- **Short name:** KVKK (Law) / KVK Kurulu (Authority)
- **URL:** https://www.kvkk.gov.tr
- **Language-of-record:** Turkish
- **Framework:** KVKK (Law No. 6698)

**Key differences:**
- "Explicit consent" doctrine narrower than GDPR lawful-basis model
- VERBIS registration (data controllers' registry) required above thresholds
- Cross-border transfers need explicit consent or Kurul approval

**Product feature must-haves:**
- Turkish-language flows
- VERBIS-registration package generator
- Explicit-consent capture UI aligned with Kurul decisions

---

## 🇺🇸 United States — Sector Patchwork

Not a single DPA, but worth noting for SaaS expansion:

- **CCPA / CPRA (California):** CPPA (California Privacy Protection Agency) — https://cppa.ca.gov
- **Other state laws:** VA CDPA, CO CPA, CT, UT, TX, etc. — fragmented; a product-level CCPA-alignment template often covers the common denominator
- **Sectoral:** HIPAA (health), GLBA (finance), COPPA (children), FERPA (education)

For an EU-focused compliance SaaS, US expansion typically means:
- CCPA-style "Do Not Sell / Share" signal + GPC support
- Privacy-policy blocks for US residents
- CPPA-aligned SAR (Subject Access Request) equivalent

---

## Cross-DPA Coordination

- **EDPB** — European Data Protection Board, issues binding decisions on cross-border matters
- **DSK** (Germany) — Datenschutzkonferenz, harmonization body for German state DPAs
- **Global Privacy Assembly** — international coordination

For cross-border customers, identify the **lead supervisory authority** under GDPR Art. 56 — this is the primary enforcer; other concerned authorities can intervene but the lead takes most cases.

## How to Use This File

When a user asks about a specific market:
1. Jump to that section
2. Cross-check enforcement claims with `WebSearch` or `legal-os:regulatory-intel` (this file ages fast)
3. Pull out product-relevant items
4. Produce Backlog Items citing the DPA specifically
5. Flag any "language-of-record" asks that require translation workstreams
