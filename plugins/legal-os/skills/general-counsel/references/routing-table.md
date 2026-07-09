# General Counsel Routing Table

Complete mapping of user intents to Legal OS specialist agents. Delegate via the Agent
tool with the scoped name, e.g. `Agent(subagent_type: "legal-os:dpo", prompt: ...)`.
Remember FAST MODE: a single-domain request goes to exactly one agent — the "Secondary"
entries below are only for genuinely multi-domain requests.

## Route: legal-os:compliance-officer

**Triggers:** compliance, obligations, compliance status, obligation tracking, compliance dashboard, evidence, evidence pack, compliance report, compliance score, risk score, overdue tasks, compliance gaps, framework, frameworks, compliance audit, compliance review, compliance monitoring, regulatory compliance, compliance checklist

**Example prompts:**
- "What's our current compliance status?"
- "Show me overdue obligations"
- "Generate an evidence pack for GDPR"
- "What's our compliance score?"
- "What obligations are due this month?"
- "Show me compliance gaps"
- "Which frameworks do we have active?"
- "Run a compliance audit"
- "What evidence is missing for our obligations?"
- "Update me on task completion rates"

**Secondary:** None (standalone)

## Route: legal-os:dpo

**Triggers:** GDPR, data protection, DPIA, data protection impact assessment, DSAR, data subject access request, subject access request, SAR, lawful basis, legal basis, processing, records of processing, RoPA, data mapping, data inventory, special categories, sensitive data, consent, legitimate interest, breach notification, DPO, data protection officer, privacy by design, data minimization, purpose limitation, storage limitation, right to erasure, right to be forgotten, data portability, automated decision making, profiling, international transfer, adequacy decision, standard contractual clauses, SCC, binding corporate rules, BCR

**Example prompts:**
- "Do we need a DPIA for this new system?"
- "Walk me through a DPIA"
- "We received a DSAR — what do we do?"
- "What's our lawful basis for this processing?"
- "Help me create records of processing"
- "Do we need a DPO?"
- "Review our consent mechanism"
- "Can we transfer data to the US?"
- "What are the rules for processing children's data?"
- "How long can we retain customer data?"
- "Explain the right to erasure"
- "Are we compliant with privacy by design?"

**Secondary:** compliance-officer (only if obligation-tracking context is explicitly needed)

## Route: legal-os:contract-manager

**Triggers:** contract, contracts, NDA, non-disclosure, confidentiality agreement, DPA review, data processing agreement, terms, terms and conditions, agreement, redline, redlining, clause, clauses, amendment, renewal, signature, signing, counterparty, SLA, service level agreement, license agreement, employment contract, consulting agreement, master service agreement, MSA, statement of work, SOW

**Example prompts:**
- "Review this contract"
- "Triage this NDA"
- "Draft a DPA for our vendor"
- "Redline this agreement"
- "What contracts are expiring soon?"
- "Flag risky clauses in this document"
- "Prepare this contract for signature"
- "Review the liability clause"
- "Is this indemnification acceptable?"
- "Compare our standard terms to this counterparty draft"
- "Track this new contract"

**Secondary:** legal-researcher (only if precedent/guidance research is explicitly needed)

## Route: legal-os:legal-researcher

**Triggers:** research, legal research, memo, legal memo, opinion, legal opinion, case law, precedent, regulation, directive, article, statute, law, legislation, guidance, interpretation, ruling, court decision, CJEU, ECJ, court of justice, analysis, due diligence, legal due diligence, legal analysis, regulatory guidance, official guidance, authority guidance, EDPB guidelines

**Example prompts:**
- "Research the current state of AI regulation in the EU"
- "Write a legal memo on data retention requirements"
- "What does GDPR Article 28 actually require?"
- "Find EDPB guidance on legitimate interest"
- "Summarize recent CJEU rulings on cookie consent"
- "What are the key differences between GDPR and UK GDPR?"
- "Research NIS2 obligations for our industry"
- "Prepare a legal analysis of this situation"
- "What are the penalties for non-compliance with [regulation]?"
- "Find the latest regulatory guidance on international transfers"

**Secondary:** None (standalone)

## Route: legal-os:regulatory-intel

**Triggers:** regulatory, regulation news, new law, new regulation, enforcement, fine, penalty, sanction, regulatory update, regulatory change, horizon scanning, regulatory radar, EDPB, DPA decision, authority decision, enforcement action, compliance deadline, transition period, implementation date, AI Act timeline, NIS2 deadline, DORA implementation, CSRD reporting, ePrivacy regulation, regulatory landscape, regulatory risk

**Example prompts:**
- "What regulatory changes are coming?"
- "Any recent GDPR enforcement actions?"
- "When does NIS2 apply to us?"
- "What's the AI Act timeline?"
- "Scan for regulatory news this week"
- "What fines have been issued in our industry?"
- "Is there new EDPB guidance?"
- "Update the regulatory calendar"
- "What are the key compliance deadlines for Q2?"
- "How does the latest DPA decision affect us?"

**Secondary:** compliance-officer (only if impact on tracked obligations is explicitly requested)

## Route: legal-os:incident-response

**Triggers:** breach, data breach, incident, security incident, breach notification, data leak, unauthorized access, cybersecurity incident, ransomware, phishing, data loss, compromised, attack, intrusion, personal data breach, notification to authority, notification to data subjects, 72 hours, containment, remediation, forensics

**Example prompts:**
- "We have a data breach"
- "Report a security incident"
- "A laptop with customer data was stolen"
- "We discovered unauthorized access to our database"
- "How do we notify the DPA?"
- "Do we need to notify affected individuals?"
- "What's our breach notification timeline?"
- "Assess this incident's severity"
- "Draft a breach notification letter"
- "Review our incident response readiness"

**IMPORTANT:** For active breaches, route IMMEDIATELY without delay. The 72-hour clock is ticking.

**Secondary:** dpo (for regulatory assessment after triage — Pipeline model)

## Route: legal-os:vendor-risk

**Triggers:** vendor, supplier, third party, third-party, processor, sub-processor, subprocessor, vendor risk, vendor assessment, vendor onboarding, vendor due diligence, DPA, data processing agreement, vendor compliance, supply chain, outsourcing, cloud provider, SaaS provider, service provider, vendor management, vendor audit, processor obligations

**Example prompts:**
- "Assess this vendor's data protection practices"
- "Review our vendor's DPA"
- "Onboard a new vendor"
- "Check if our vendor is compliant"
- "What sub-processors does our vendor use?"
- "Is our vendor's DPA adequate under Article 28?"
- "Audit our vendor portfolio"
- "Flag vendors with expiring DPAs"
- "Rate this vendor's risk level"
- "Review the vendor's security certifications"

**Secondary:** contract-manager (for DPA paper review during onboarding — Pipeline model)

## Route: Full Pipeline (GC orchestrates all)

**Triggers:** legal health check, legal audit, comprehensive audit, full compliance review, legal assessment, annual review, board report, quarterly report, legal dashboard, legal health, legal status, how are we doing legally, overall legal position

**Example prompts:**
- "Run a full legal health check"
- "Prepare the quarterly board compliance report"
- "Give me a comprehensive compliance overview"
- "What's our overall legal risk position?"
- "Annual legal review"
- "Legal status report for management"

**Pipeline order:** regulatory-intel -> compliance-officer -> dpo -> contract-manager -> vendor-risk -> incident-response -> legal-researcher

Announce the plan (which agents will run) before running it.

## Ambiguous Requests

If the request is ambiguous, use these heuristics — then still route to ONE agent:
1. If it mentions data, personal data, or privacy -> `legal-os:dpo`
2. If it mentions a document, agreement, or NDA -> `legal-os:contract-manager`
3. If it mentions status, score, or dashboard -> `legal-os:compliance-officer`
4. If it mentions a vendor, supplier, or processor -> `legal-os:vendor-risk`
5. If it mentions news, changes, or enforcement -> `legal-os:regulatory-intel`
6. If it mentions an incident, breach, or leak -> `legal-os:incident-response`
7. If it mentions research, article, or interpretation -> `legal-os:legal-researcher`
8. If truly unclear -> ask the user to clarify
