# Legal OS — Comprehensive User Guide

A complete legal operating system for Claude Code. One orchestrator (the General Counsel), seven specialist agents, all driven from your CLI — purpose-built for EU compliance-focused SMEs.

---

## Table of Contents

1. [Quick Start](#quick-start)
2. [The Team — Agent Roster](#the-team)
3. [Architecture — How It Works](#architecture)
4. [Agent Deep Dives](#agent-deep-dives)
5. [Common Workflows & Playbooks](#common-workflows)
6. [ObligoBoard Integration](#obligoboard-integration)
7. [Data Files & Configuration](#data-files)
8. [Scheduled Automations](#scheduled-automations)
9. [Tips & Best Practices](#tips)
10. [Troubleshooting](#troubleshooting)
11. [Disclaimers](#disclaimers)

---

## 1. Quick Start

### Step 1: Configure Your Organization

```
/legal-os:setup
```

The setup wizard collects your legal profile in 7 steps:

| Step | What It Asks | Why It Matters |
|---|---|---|
| Organization basics | Name, type, size, country, industry, domain | Tailors all agent outputs to your context |
| Jurisdictions & regulations | Operating countries, active frameworks | Determines which rules apply and which DPAs to monitor |
| Data protection | DPO details, supervisory authority | Enables breach notification routing, DPIA workflows |
| ObligoBoard integration | Base URL, sync method | Connects to your compliance tracking data |
| Contract defaults | Governing law, liability caps, required clauses | Your negotiation playbook for contract reviews |
| Risk appetite | Conservative/moderate/aggressive, thresholds | Controls when agents escalate vs. auto-handle |
| Reporting | Report day, scan frequency, board report cadence | Drives scheduled automations |

Configuration is saved to `~/.claude/plugins/data/legal-os/config.json`. You can manage multiple organizations and switch between them.

### Step 2: Talk to the General Counsel

```
/legal-os:general-counsel [your request in plain language]
```

The GC is your single entry point. It reads your org config, checks memory for recent context, classifies your request, and routes to the right specialist(s).

**Try these to get started:**

```
/legal-os:general-counsel What's our current compliance status?
/legal-os:general-counsel Do we need a DPIA for our new analytics system?
/legal-os:general-counsel Review this NDA
/legal-os:general-counsel We had a data breach — a laptop was stolen
/legal-os:general-counsel Run a full legal health check
```

### Step 3: Go Direct When You Know Who to Ask

Every specialist is also invocable directly:

```
/legal-os:compliance-officer   Show me overdue obligations
/legal-os:dpo                  Walk me through a DPIA for [system]
/legal-os:contract-manager     Review this DPA for Article 28 compliance
/legal-os:legal-researcher     Research NIS2 requirements for tech SMEs
/legal-os:regulatory-intel     Scan for GDPR enforcement actions this quarter
/legal-os:incident-response    EMERGENCY: unauthorized access to customer database
/legal-os:vendor-risk          Assess vendor CloudCorp for onboarding
```

---

## 2. The Team

### Agent Roster

| Agent | Role | Domain Expertise | Key Integrations |
|---|---|---|---|
| **General Counsel** | Orchestrator | Routes, synthesizes, resolves conflicts | All agents, built-in legal skills, claude-mem |
| **Compliance Officer** | Compliance monitoring | Obligation tracking, evidence gaps, risk scores, framework management | ObligoBoard snapshot, regulatory calendar |
| **DPO** | Data protection | GDPR, DPIA, DSAR, lawful basis, RoPA, international transfers, consent | ObligoBoard (GDPR-filtered), EDPB guidance |
| **Contract Manager** | Contract lifecycle | Review, redline, NDA triage, DPA review, signature prep, portfolio tracking | `legal:review-contract`, `legal:triage-nda`, `legal:signature-request` |
| **Legal Researcher** | Research & drafting | Regulation lookup, legal memos, case law, due diligence, guidance interpretation | WebSearch, Apify, EUR-Lex, `legal:legal-response` |
| **Regulatory Intel** | Horizon scanning | New laws, enforcement actions, deadlines, EDPB guidance, regulatory landscape | WebSearch, Apify, regulatory calendar |
| **Incident Response** | Breach management | Triage, 72-hour notification, DPA communication, remediation tracking | Incident log, scheduled tasks, notification templates |
| **Vendor Risk** | Third-party risk | Vendor assessment, DPA compliance (Art. 28), sub-processor management | Vendor registry, `legal:vendor-check`, WebSearch |
| **Setup** | Configuration | Organization profile, framework selection, contract playbook | Config files, data directory |

### How Agents Were Chosen

The roster was designed by evaluating your original request against distinctness of responsibilities, workflow boundaries, and EU regulatory requirements:

- **Legal Counsel was merged into General Counsel** — too generic, overlapped every specialist. The GC provides strategic judgment during synthesis, and `legal:legal-risk-assessment` handles standalone risk scoring.
- **Paralegal was renamed to Legal Researcher** — maps more clearly to the specialization (research, memo drafting, due diligence).
- **Three agents were added:** Regulatory Intelligence (no built-in skill covers horizon scanning), Incident Response Manager (72-hour GDPR timeline demands a dedicated time-critical workflow), Vendor Risk Assessor (Article 28 DPA compliance is procedurally distinct from contract management).

---

## 3. Architecture

### Three Delegation Models

The General Counsel uses three models to route your requests:

#### Model A: Hub-and-Spoke (single specialist)

For focused, single-domain questions. The GC routes to one agent and returns the result.

```
You:   "What's our compliance score?"
GC:    Routes to Compliance Officer
       -> Reads ObligoBoard snapshot
       -> Returns: score, framework breakdown, top improvement actions
```

```
You:   "Is legitimate interest valid for our email marketing?"
GC:    Routes to DPO
       -> Analyzes Art. 6(1)(f) balancing test
       -> Returns: assessment with recommendation and conditions
```

#### Model B: Pipeline (sequential specialists)

For workflows where one agent's output feeds the next. The GC orchestrates the handoff.

```
You:   "Onboard vendor CloudCorp as a data processor"
GC:    1. Vendor Risk    -> risk assessment (score, gaps, conditions)
       2. Contract Manager -> DPA review with risk context
       3. Compliance Officer -> obligation tracking setup
       -> Returns: complete onboarding report
```

```
You:   "We discovered a data breach"
GC:    1. Incident Response -> immediate triage, classification, timeline
       2. DPO              -> regulatory assessment, notification decision
       -> Returns: incident report with notification plan
```

#### Model C: Collaborative (parallel specialists)

For broad assessments where multiple perspectives are needed simultaneously. The GC runs agents in parallel and synthesizes.

```
You:   "Prepare the quarterly board compliance report"
GC:    [parallel] Compliance Officer + DPO + Contract Manager + Regulatory Intel
       -> Each produces their domain report
       -> GC synthesizes into unified board report
```

```
You:   "Assess legal implications of expanding to France"
GC:    [parallel] Regulatory Intel + DPO + Compliance Officer
       -> Regulatory Intel: French implementation specifics
       -> DPO: CNIL-specific requirements, local DPA registration
       -> Compliance Officer: framework gap analysis
       -> GC synthesizes into expansion readiness report
```

#### Full Pipeline (Legal Health Check)

The comprehensive assessment runs all seven specialists sequentially:

```
You:   "Run a full legal health check"
GC:    1. Regulatory Intel    -> regulatory landscape and upcoming changes
       2. Compliance Officer  -> obligation completion, evidence gaps, scores
       3. DPO                 -> DPIA status, DSARs, breach readiness, RoPA
       4. Contract Manager    -> expiring contracts, unsigned items, risks
       5. Vendor Risk         -> expired DPAs, unchecked sub-processors
       6. Incident Response   -> playbook completeness, response capability
       7. Legal Researcher    -> industry benchmarking
       -> Returns: Legal Health Report (grade A-F, risk heat map, top 5 priorities)
```

### Built-in Legal Skill Integration

Legal OS wraps nine Anthropic built-in legal skills, adding your organization context:

| Built-in Skill | Wrapping Agent | What Legal OS Adds |
|---|---|---|
| `legal:review-contract` | Contract Manager | Your negotiation playbook (liability caps, required clauses, governing law preference) |
| `legal:triage-nda` | Contract Manager | Tracks GREEN/YELLOW/RED result in your contract index |
| `legal:signature-request` | Contract Manager | Pre-signature checklist from your config (entity verification, exhibits, DPA attached) |
| `legal:compliance-check` | Compliance Officer | Cross-references with your ObligoBoard obligation data |
| `legal:legal-risk-assessment` | General Counsel | Standalone risk scoring with your risk appetite thresholds |
| `legal:vendor-check` | Vendor Risk | Layers your risk matrix + Article 28 analysis |
| `legal:meeting-briefing` | General Counsel | Enriched with matter context + recent regulatory intelligence |
| `legal:brief` | General Counsel | Enriched with regulatory changes + compliance status |
| `legal:legal-response` | Legal Researcher | Templated responses for DSARs, litigation holds, vendor inquiries |

### Cross-Session Memory

All agents store key findings in claude-mem with a `los:` prefix. This means:
- The GC remembers your last compliance score across sessions
- Contract reviews are tracked over time
- Regulatory changes are available for future reference
- Incident history persists

Search past findings anytime:
```
/claude-mem:mem-search los: [your org name]
```

---

## 4. Agent Deep Dives

### 4.1 General Counsel (Orchestrator)

**Invocation:** `/legal-os:general-counsel [request]`

**What it does:**
1. Loads your org config and recent legal memory
2. Classifies your request against a routing table with 50+ intent patterns
3. Delegates to the right specialist(s)
4. Synthesizes outputs, resolves conflicts, prioritizes recommendations
5. Stores key findings in memory

**Routing intelligence:** The GC matches keywords and intent patterns to determine routing. For ambiguous requests, it uses heuristics:
- Mentions data/privacy/personal data → DPO
- Mentions document/agreement/NDA → Contract Manager
- Mentions status/score/dashboard → Compliance Officer
- Mentions vendor/supplier/processor → Vendor Risk
- Mentions news/changes/enforcement → Regulatory Intel
- Mentions incident/breach/leak → Incident Response
- Mentions research/article/interpretation → Legal Researcher

**Emergency bypass:** For data breaches, the GC immediately routes to Incident Response without delay.

**Direct skill invocations:** The GC can also invoke built-in skills directly for:
- `legal:legal-risk-assessment` — "What's the risk of X?"
- `legal:meeting-briefing` — "Prepare me for the compliance meeting"
- `legal:brief` — "Give me a legal briefing on X"

---

### 4.2 Compliance Officer

**Invocation:** `/legal-os:compliance-officer [request]`

**Specialization:** Monitors and manages regulatory compliance across all active frameworks. The direct bridge to your ObligoBoard compliance data.

**Core capabilities:**

| Capability | What It Does | Example Prompt |
|---|---|---|
| **Compliance status** | Framework completion rates, overdue items, risk levels | "What's our overall compliance status?" |
| **Obligation tracking** | Per-obligation status with due dates, owners, evidence | "Show me overdue obligations in GDPR" |
| **Evidence gap analysis** | Missing, incomplete, or outdated evidence | "What evidence are we missing?" |
| **Compliance scoring** | Score breakdown (70% completion + 30% profile) with improvement actions | "Why is our score low? How do we improve it?" |
| **Framework management** | Active frameworks, missing frameworks, recommendations | "Should we add the Data Processor framework?" |
| **Compliance audit** | Cross-framework assessment with orphaned tasks, calendar check | "Run a full compliance audit" |

**ObligoBoard integration:** Reads `compliance-snapshot.json`. Warns if the snapshot is >7 days old.

---

### 4.3 Data Protection Officer (DPO)

**Invocation:** `/legal-os:dpo [request]`

**Specialization:** All GDPR and data protection matters. Procedurally distinct from the Compliance Officer — the DPO handles the deep regulatory analysis, not just tracking.

**Core capabilities:**

| Capability | What It Does | Example Prompt |
|---|---|---|
| **DPIA** | Art. 35 screening + full DPIA walkthrough (8-part template) | "Do we need a DPIA for our new HR analytics?" |
| **DSAR handling** | Validation, scope, search, exemptions, response drafting, deadlines | "We received a DSAR — walk me through it" |
| **Breach assessment** | Risk-to-individuals analysis, notification decisions (Art. 33/34) | "Assess the risk of this breach to data subjects" |
| **Records of processing** | Controller/processor RoPA guidance and review | "Help me build our records of processing" |
| **Lawful basis** | Art. 6(1) analysis across all six bases + special categories | "What's our lawful basis for this processing?" |
| **International transfers** | Adequacy, SCCs, BCRs, TIA, Schrems II supplementary measures | "Can we transfer customer data to the US?" |
| **Privacy by design** | Minimization, purpose limitation, retention, pseudonymization review | "Review our new feature for privacy by design" |
| **Consent assessment** | Art. 7 conditions, withdrawal mechanisms, record-keeping | "Is our consent mechanism GDPR-compliant?" |

**Reference files loaded:** GDPR articles reference (key provisions from Art. 5-84 with fine tiers), DPIA 8-part template, DSAR step-by-step procedures, breach notification 72-hour timeline.

---

### 4.4 Contract Manager

**Invocation:** `/legal-os:contract-manager [request]`

**Specialization:** Full contract lifecycle — from intake through review, negotiation, tracking, and signature.

**Core capabilities:**

| Capability | What It Does | Example Prompt |
|---|---|---|
| **Contract review** | Clause-by-clause analysis against your negotiation playbook | "Review this vendor agreement" |
| **NDA triage** | GREEN/YELLOW/RED classification with embedded risk detection | "Triage this NDA from Acme Corp" |
| **DPA review** | Article 28 compliance check (13 mandatory elements) | "Review this DPA — is it Art. 28 compliant?" |
| **Redline generation** | Specific alternative language for problematic clauses | "Generate redlines for this contract" |
| **Contract tracking** | Portfolio view with expiry dates, risk ratings, status | "What contracts are expiring in the next 90 days?" |
| **Signature prep** | Pre-signature checklist (entity, exhibits, DPA, approvals) | "Prepare this contract for signature" |

**Negotiation playbook:** Every review is measured against your configured `contract_defaults` — governing law, liability caps, required clauses, and unacceptable clauses. Deviations are flagged with risk levels.

**Priority framework for redlines:**
- **P1 (Deal-breaker):** Must be changed — unlimited liability, no DPA, IP assignment of your pre-existing IP
- **P2 (Strong position):** Push hard — one-sided indemnification, no termination for convenience
- **P3 (Preferred):** Negotiate — governing law preferences, liability cap range, notice periods
- **P4 (Accept):** Minor — formatting, definitions, numbering

**Reference files loaded:** Clause library (10 clause categories with standard/acceptable/red flag patterns), redline rules (negotiation playbook with DPA-specific positions and response templates).

---

### 4.5 Legal Researcher

**Invocation:** `/legal-os:legal-researcher [request]`

**Specialization:** Legal research, memo drafting, case law analysis, regulatory guidance interpretation, and due diligence.

**Core capabilities:**

| Capability | What It Does | Example Prompt |
|---|---|---|
| **Regulation research** | Finds and synthesizes guidance from EDPB, DPAs, EUR-Lex | "What does GDPR Article 28 actually require?" |
| **Legal memo (IRAC)** | Issue-Rule-Application-Conclusion structured memo | "Write a memo on data retention requirements" |
| **Case law analysis** | CJEU rulings, national court decisions, enforcement precedents | "Recent CJEU rulings on cookie consent" |
| **Regulatory comparison** | Side-by-side framework comparison matrix | "Compare GDPR and UK GDPR requirements" |
| **Due diligence** | Target entity/transaction legal research | "Legal due diligence on company XYZ" |
| **Guidance interpretation** | Explains official guidance in practical terms | "What does the new EDPB guidance on legitimate interest mean for us?" |

**Source hierarchy (most to least authoritative):**
1. Primary legislation (EUR-Lex)
2. CJEU judgments (Curia)
3. Supervisory authority decisions and guidelines (EDPB, national DPAs)
4. European Commission guidance
5. Academic commentary
6. Industry body guidance

**Reference files loaded:** Research sources directory (primary legislation, court databases, all EU/EEA DPAs, enforcement trackers, AI Act sources, search strategies), memo templates (IRAC, regulatory comparison, due diligence summary, quick brief).

---

### 4.6 Regulatory Intelligence

**Invocation:** `/legal-os:regulatory-intel [request]`

**Specialization:** Horizon scanning for regulatory changes, enforcement monitoring, and compliance deadline tracking.

**Core capabilities:**

| Capability | What It Does | Example Prompt |
|---|---|---|
| **Horizon scan** | Searches EDPB, national DPAs, EUR-Lex for recent developments | "What regulatory changes are coming?" |
| **Enforcement monitoring** | Recent fines, enforcement actions, DPA decisions | "Any GDPR fines in our industry this quarter?" |
| **Regulatory calendar** | Tracks compliance deadlines with impact ratings and status | "What are our key deadlines for Q3?" |
| **Impact assessment** | Evaluates how a new development affects your org specifically | "How does the new EDPB opinion affect us?" |
| **Regulatory briefing** | Structured report of developments by impact level | "Weekly regulatory scan" |

**Pre-loaded regulatory calendar:** Includes AI Act milestones (Feb 2025 prohibitions, Aug 2025 GPAI, Aug 2026 main provisions, Aug 2027 Annex I), NIS2 transposition, DORA application, CSRD phased rollout, Data Act application date.

**Reference files loaded:** EU regulatory calendar (all major framework dates through 2029), authority directory (every EU/EEA DPA with contact details and breach notification portals, plus all 16 German state DPAs).

---

### 4.7 Incident Response Manager

**Invocation:** `/legal-os:incident-response [description]`

**Specialization:** Legal aspects of data breaches — triage, classification, notification, and remediation tracking. The only agent with **emergency direct-invoke privilege** (can bypass the GC).

**Two operating modes:**

#### Active Incident Mode
Triggers when you report an actual breach. Operates on a 5-phase timeline:

| Phase | Timeframe | Actions |
|---|---|---|
| **1. Triage** | 0-15 min | Document, classify severity (S1-S4), assign ID, start timeline |
| **2. Assessment** | 0-12 hours | Data scope, containment status, notification decision |
| **3. Notification prep** | 12-48 hours | Draft authority notification (Art. 33), draft data subject notification (Art. 34) |
| **4. Submission** | 48-72 hours | Submit to supervisory authority, log, schedule follow-up |
| **5. Post-incident** | After resolution | Root cause, lessons learned, process improvements |

#### Assessment Mode
For readiness checks and drills when there is no active incident:

- **Readiness assessment** — scores your breach response capability (1-10) across playbook, process, and contacts
- **Breach response drill** — presents a realistic scenario and walks through the response, identifying gaps

**Severity classification:**
- **S1 (Critical):** Large-scale breach of sensitive data, ongoing exfiltration, >10,000 records
- **S2 (High):** Confirmed breach of personal data, contained, 100-10,000 records
- **S3 (Medium):** Potential breach under investigation, <100 records or non-sensitive
- **S4 (Low):** Near-miss, no personal data involved, failed attack attempt

**Reference files loaded:** Incident classification framework (severity levels with decision tree and escalation matrix), notification templates (supervisory authority notification, data subject letter, public communication, NIS2 incident notification, internal report template).

---

### 4.8 Vendor Risk Assessor

**Invocation:** `/legal-os:vendor-risk [request]`

**Specialization:** Third-party and processor risk assessment, DPA compliance under Article 28, sub-processor management, and vendor lifecycle.

**Core capabilities:**

| Capability | What It Does | Example Prompt |
|---|---|---|
| **Risk assessment** | 8-domain evaluation with weighted scoring | "Assess CloudCorp's data protection maturity" |
| **Vendor onboarding** | Full pipeline: assessment -> DPA -> compliance tracking | "Onboard vendor XYZ as a data processor" |
| **DPA compliance** | 13-element Art. 28 checklist with quality assessment | "Is this vendor's DPA adequate?" |
| **Portfolio audit** | Review all vendors for expired DPAs, stale assessments | "Audit our vendor portfolio" |
| **Sub-processor management** | Track sub-processor lists, approval mechanisms, changes | "What sub-processors does our vendor use?" |

**8-domain assessment framework (with weights):**

| Domain | Weight | What's Evaluated |
|---|---|---|
| Data protection maturity | 20% | DPO, privacy program, audits, privacy by design |
| Security posture | 20% | ISO 27001, SOC 2, pen testing, SIEM |
| DPA compliance | 20% | All 13 Art. 28 elements, quality rating |
| Sub-processor management | 10% | Approval mechanism, current list, back-to-back DPAs |
| International transfers | 10% | Location, SCCs, TIA, supplementary measures |
| Breach notification | 10% | Documented procedure, timeframe, testing |
| Business continuity | 5% | DR/BC plans, RTO/RPO, redundancy |
| Contractual cooperation | 5% | Audit rights, responsiveness, transparency |

**Rating thresholds:**
- 4.0-5.0 = LOW risk (approve)
- 3.0-3.9 = MEDIUM (approve with conditions)
- 2.0-2.9 = HIGH (approve only if business-critical, enhanced monitoring)
- 1.0-1.9 = CRITICAL (reject or require fundamental remediation)

**Reference files loaded:** Vendor assessment criteria (8-domain scoring matrix with evidence requests), DPA checklist (13-element Art. 28 compliance check with quality assessment, key negotiation points for sub-processors, breach notification, audit rights, data location, end-of-contract handling).

---

## 5. Common Workflows & Playbooks

### Weekly Legal Routine

**Monday** — Situational awareness:
```
/legal-os:general-counsel Weekly compliance and regulatory update
```
This triggers the Compliance Officer (status check) and Regulatory Intel (scan) in parallel.

**Tuesday** — Focus work (examples):
```
/legal-os:contract-manager Review the pending vendor agreement from last week
/legal-os:dpo We need to assess the lawful basis for our new marketing automation
```

**Wednesday** — Vendor and contract health:
```
/legal-os:general-counsel Any contracts expiring or vendors needing attention?
```

**Thursday** — Deep work (DPIA, research):
```
/legal-os:dpo Walk me through a DPIA for our new customer analytics platform
/legal-os:legal-researcher Research the AI Act classification for our product
```

**Friday** — Close out and plan:
```
/legal-os:general-counsel Summarize this week's legal actions and priorities for next week
```

---

### New Vendor Onboarding (Pipeline)

```
/legal-os:general-counsel Onboard CloudCorp as a data processor for customer support
```

**What happens:**
1. **Vendor Risk** assesses CloudCorp across 8 domains → produces risk rating
2. **Contract Manager** reviews CloudCorp's DPA against Art. 28 → produces redlines
3. **Compliance Officer** sets up obligation tracking → adds to processor register
4. **GC synthesizes** → complete onboarding report with risk rating, DPA status, conditions, action items

**Follow-up after DPA is signed:**
```
/legal-os:vendor-risk Update vendor CloudCorp — DPA signed 2026-04-10, ISO 27001 cert valid until 2027-03
```

---

### Data Breach Response (Emergency Pipeline)

```
/legal-os:incident-response A laptop containing customer payment data was stolen from an employee's car
```

**What happens immediately:**
1. **Incident Response** classifies severity (likely S2: confirmed breach, financial data)
2. Creates incident log entry with timeline
3. Assesses notification requirements (Art. 33 likely required, Art. 34 likely required due to financial data)
4. Drafts supervisory authority notification
5. Drafts data subject notification letter
6. Hands off to **DPO** for regulatory assessment refinement

**Follow-up prompts:**
```
/legal-os:incident-response Update incident INC-001 — laptop had full disk encryption, PIN was 4 digits
/legal-os:incident-response Draft the supplementary notification for INC-001
/legal-os:incident-response Close incident INC-001 — resolved, device remote-wiped confirmed
```

---

### DPIA Workflow

```
/legal-os:dpo Do we need a DPIA for a new employee monitoring system that tracks productivity metrics?
```

**What happens:**
1. **DPO** runs Art. 35 screening — checks all 3 mandatory triggers and 10 EDPB high-risk criteria
2. If DPIA required: walks through the 8-part template:
   - Part 1: Screening rationale
   - Part 2: Processing description (purpose, scope, data categories, recipients)
   - Part 3: Necessity and proportionality (minimization, purpose limitation, retention)
   - Part 4: Risk assessment (likelihood x severity matrix)
   - Part 5: Mitigation measures (technical and organizational)
   - Part 6: DPO opinion
   - Part 7: Prior consultation decision (Art. 36)
   - Part 8: Approval and review schedule

---

### Contract Review

```
/legal-os:contract-manager Review this SaaS agreement [paste or provide file path]
```

**What happens:**
1. **Contract Manager** reads the document
2. Invokes `legal:review-contract` with your negotiation playbook injected
3. Checks every clause against your configured defaults:
   - Governing law matches? Liability cap acceptable? Required clauses present?
4. Produces a Contract Review Summary:
   - Overall risk: GREEN / YELLOW / RED
   - Key terms table (governing law, term, liability, termination, data protection)
   - Flagged clauses with issue description, risk level, and proposed alternative language
   - Missing clauses
   - Recommendation: Accept / Accept with amendments / Reject / Escalate

---

### Quarterly Board Report (Full Pipeline)

```
/legal-os:general-counsel Prepare the quarterly board compliance report
```

**What happens:**
1. **Compliance Officer** → obligation completion rates, risk scores, evidence status
2. **DPO** → DPIA status, open DSARs, breach history, processing records health
3. **Contract Manager** → contract portfolio (expiring, high-risk, unsigned)
4. **Regulatory Intel** → regulatory changes this quarter, upcoming deadlines
5. **GC synthesizes** → Legal Health Report with:
   - Executive summary (3-5 bullet points)
   - Overall health grade (A-F)
   - Domain scores table (compliance, data protection, contracts, vendor, incident readiness, regulatory)
   - Top 5 priorities ranked by risk and urgency
   - Regulatory landscape outlook
   - Numbered recommendations
   - Next review date

---

### Regulatory Change Impact Assessment

```
/legal-os:general-counsel The AI Act main provisions take effect in August 2026 — what do we need to do?
```

**What happens:**
1. **Regulatory Intel** → AI Act Annex III high-risk classification analysis, key requirements, timeline
2. **Compliance Officer** → gap analysis against current obligations
3. **Legal Researcher** → latest Commission guidance and implementation details
4. **GC synthesizes** → impact assessment with: applicability determination, compliance gap, action plan with deadlines

---

### Multi-Jurisdictional Expansion

```
/legal-os:general-counsel We're expanding operations to France — assess legal implications
```

**What happens (Model C: parallel):**
1. **Regulatory Intel** → French implementation specifics, CNIL requirements, local rules
2. **DPO** → French data protection specifics (employee monitoring stricter, CNIL cookie guidance, language requirements)
3. **Compliance Officer** → framework gap analysis, additional obligations
4. **GC synthesizes** → expansion readiness report with jurisdiction-specific action items

---

## 6. ObligoBoard Integration

### How It Works Today (Phase 1: Snapshot)

Legal OS reads a cached export of your ObligoBoard compliance data:

```
~/.claude/plugins/data/legal-os/compliance-snapshot.json
```

The **Compliance Officer** and **DPO** agents use this snapshot for:
- Obligation completion rates and status
- Risk scores with factor breakdown
- Evidence gap identification
- Framework-specific analysis

### Keeping the Snapshot Fresh

Export your ObligoBoard data periodically and save to the path above. The agents:
- Warn if the snapshot is **7-14 days old** (may be stale)
- Alert strongly if **>14 days old** (recommend refreshing)

### Expected Snapshot Structure

The snapshot should contain:
- Organization metadata
- Compliance score (overall + breakdown)
- Frameworks with their obligations and tasks
- Task status (not started / in progress / completed / overdue)
- Evidence attachments
- Risk score factors
- Overdue summary

### Future (Phase 2: Direct API)

When ObligoBoard exposes token-authenticated API endpoints, agents will call them directly. The config stores `api_key` and `base_url` — no snapshot needed.

---

## 7. Data Files & Configuration

### File Locations

All Legal OS data lives in two locations:

| Location | Purpose |
|---|---|
| `~/.claude/plugins/marketplaces/legal-os/` | Plugin code (skills, references, GUIDE) |
| `~/.claude/plugins/data/legal-os/` | Your data (config, registries, logs, snapshots) |

### Data Files

| File | Read By | Written By | Purpose |
|---|---|---|---|
| `config.json` | All agents | Setup wizard | Organization legal profile |
| `compliance-snapshot.json` | Compliance Officer, DPO | User export | ObligoBoard compliance data |
| `contracts/index.json` | Contract Manager, GC | Contract Manager | Contract portfolio tracking |
| `regulatory-calendar.json` | Regulatory Intel, Compliance Officer | Regulatory Intel | Compliance deadlines |
| `incident-log.json` | Incident Response, GC | Incident Response | Breach/incident history |
| `vendor-registry.json` | Vendor Risk, GC | Vendor Risk | Vendor compliance tracking |
| `matters/{id}.json` | GC, any specialist | GC | Individual legal matter tracking |

### Configuration (config.json)

The config contains your organization's legal profile. Key sections:

| Section | Contents | Used By |
|---|---|---|
| Organization basics | Name, type, size, country, industry, domain | All agents |
| Jurisdictions | Countries where you process data | DPO, Regulatory Intel, Legal Researcher |
| Active frameworks | GDPR, NIS2, AI Act, DORA, CSRD, etc. | Compliance Officer, DPO |
| DPO details | Name, email, internal/external, required status | DPO, Incident Response |
| Supervisory authority | Primary DPA name, country, notification URL | Incident Response, DPO |
| ObligoBoard integration | Base URL, API availability, sync method | Compliance Officer, DPO |
| Contract defaults | Governing law, liability caps, required/unacceptable clauses | Contract Manager |
| Risk appetite | Conservative/moderate/aggressive, escalation threshold | All agents |
| Reporting | Report day, scan frequency, board report cadence | Scheduled tasks |

To edit manually: `~/.claude/plugins/data/legal-os/config.json`
To re-run the wizard: `/legal-os:setup`

### Multiple Organizations

Legal OS supports multiple organization profiles:
- Each profile is independent with its own jurisdictions, DPO, contract defaults
- Switch active profile: `/legal-os:setup switch to [org-slug]`
- Add a new profile: `/legal-os:setup NewOrg`

---

## 8. Scheduled Automations

Legal OS supports recurring tasks. Ask the GC to set these up:

```
/legal-os:general-counsel Set up our standard scheduled legal tasks
```

### Recommended Schedule

| Task | Frequency | Agent | What It Does |
|---|---|---|---|
| **Regulatory scan** | Weekly Monday ~8am | Regulatory Intel | Scans EDPB, national DPAs, EUR-Lex for developments |
| **Compliance sync** | Weekly Monday ~8:30am | Compliance Officer | Refreshes compliance snapshot, flags overdue items |
| **Contract renewal alerts** | Weekly Wednesday | Contract Manager | Flags contracts expiring within 60/30/14 days |
| **DSAR deadline monitor** | Daily weekdays ~9am | DPO | Checks for DSARs approaching 30-day response limit |
| **Vendor DPA expiry check** | Monthly 1st | Vendor Risk | Flags vendors with approaching DPA renewals |
| **Quarterly board report** | Quarterly 1st | GC (full pipeline) | Runs all specialists, produces comprehensive report |
| **Breach readiness drill** | Quarterly | Incident Response | Reviews playbook completeness, template currency |

---

## 9. Tips & Best Practices

### Getting Better Results

**Be specific about what you need:**

| Instead of | Try |
|---|---|
| "Check compliance" | "Show me overdue GDPR obligations with evidence gaps" |
| "Review this" | "Review this DPA for Article 28 compliance — focus on sub-processor and audit provisions" |
| "Legal question" | "Is legitimate interest a valid basis for email marketing to existing customers under Art. 6(1)(f)?" |
| "Vendor check" | "Assess CloudCorp for processing customer payment data — they're based in the US" |
| "What's new" | "Any GDPR enforcement actions against SaaS companies in Germany this quarter?" |

**Reference regulations directly:**
- "Under GDPR Article 35, do we need a DPIA for...?"
- "Check our NIS2 readiness against Article 21 requirements"
- "Does this vendor DPA meet all 13 elements of Article 28(3)?"

**Chain work across sessions** — Legal OS remembers:
```
Session 1: /legal-os:gc Run a full legal health check
Session 2: /legal-os:gc Address the top priority from last week's health check
Session 3: /legal-os:gc Has our compliance improved since the audit?
```

**Provide context when reviewing documents:**
```
/legal-os:contract-manager Review this SaaS agreement.
Context: CloudCorp is a US-based vendor, they'll process customer email and usage data,
we want to cap liability at 12 months and need EU-only data storage.
```

### When to Use the GC vs. Going Direct

| Situation | Use |
|---|---|
| Not sure which agent handles this | GC |
| Need multiple perspectives | GC (will coordinate) |
| Quarterly or board-level reporting | GC (runs full pipeline) |
| Know exactly which domain | Go direct to specialist |
| Active data breach | Go direct to Incident Response |
| Simple compliance status check | Go direct to Compliance Officer |
| Single contract to review | Go direct to Contract Manager |

### Risk Appetite Settings

Your configured risk appetite affects how agents behave:

| Setting | Agent Behavior |
|---|---|
| **Conservative** | Flag everything at LOW+ risk, never auto-approve, recommend human review for all decisions |
| **Moderate** | Flag MEDIUM+ risk, auto-handle LOW risk items, recommend human review for MEDIUM+ |
| **Aggressive** | Flag only HIGH+ risk, auto-handle LOW and MEDIUM, recommend human review for HIGH+ |

Configure via: `/legal-os:setup` (Step 6: Risk Appetite)

---

## 10. Troubleshooting

| Issue | Solution |
|---|---|
| **"Config not found"** | Run `/legal-os:setup` to create your organization profile |
| **"Snapshot is stale"** | Re-export your ObligoBoard data to `compliance-snapshot.json` |
| **Wrong org context** | Check active profile: `/legal-os:setup show current config` |
| **Agent doesn't have my contract defaults** | Verify `contract_defaults` section in config.json |
| **GC routes to wrong specialist** | Go direct to the specialist you want, or be more specific in your request |
| **Missing regulatory deadlines** | Ask: `/legal-os:regulatory-intel Update the regulatory calendar` |
| **Vendor not in registry** | Ask: `/legal-os:vendor-risk Add vendor XYZ to the registry` |
| **Need to edit config** | Direct path: `~/.claude/plugins/data/legal-os/config.json` |
| **Skills not loading** | Start a new Claude Code session — plugins load at session start |
| **Memory not found** | Search with: `/claude-mem:mem-search los: [org name]` |

---

## 11. Disclaimers

All Legal OS outputs include this notice:

> **This is AI-assisted legal guidance. It does not constitute legal advice. Consult qualified legal counsel for binding decisions, formal opinions, and regulatory filings.**

### What Legal OS Is

- An AI-powered legal operations **assistant** that augments your workflow
- A structured system for tracking compliance, managing contracts, monitoring regulations, and responding to incidents
- A tool that applies your organization's configured policies and playbooks consistently
- A cross-session memory system that maintains legal context over time

### What Legal OS Is Not

- A replacement for qualified legal counsel
- A certified compliance auditor
- A legally privileged source of advice
- An official channel for regulatory communication

### Always Have Human Review For

- Supervisory authority breach notifications (formal filing)
- Data subject notification letters (legal communication)
- Contract execution (binding agreements)
- DPIA approval (regulatory documentation)
- Board reports (corporate governance)
- Formal legal opinions (professional liability)
- Public-facing privacy policies (legal obligations)

Legal OS does the heavy lifting — research, analysis, drafting, tracking — so you and your legal counsel can focus on judgment, decisions, and accountability.
