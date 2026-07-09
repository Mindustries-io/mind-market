# Lead PO Routing Table

Complete mapping of user intents to PO-OS specialists.

## Route: `regulatory-po`

**Triggers:** regulation, regulatory, GDPR, CSRD, ESRS, AI Act, NIS2, DORA, ePrivacy, Data Act, new law, EDPB, compliance feature, compliance requirement, "what does {regulation} require", deadline, obligation, horizon scan, regulatory change, new guidance, article, recital

**Example prompts:**
- "The AI Act general-purpose rules kick in on 2026-08-02 — what do we need to ship?"
- "EDPB just issued guidance on legitimate interest — any backlog implications?"
- "What GDPR Article 30 records-of-processing features are we missing?"
- "Translate CSRD ESRS E1 disclosure requirements into a product spec"
- "Regulatory horizon scan for the next 6 months"
- "New NIS2 requirements for our target sector?"
- "Do we need to add a DPIA wizard? What regulations drive it?"

**Secondary:** `localization-po` (when a regulation has jurisdictional variation)

## Route: `localization-po`

**Triggers:** localization, Italy, Italian, Garante, France, French, CNIL, UK, British, ICO, Germany, German, BfDI, Spain, Spanish, AEPD, Netherlands, Dutch, AP, country-specific, national, jurisdiction, local law, translation, language, locale, regional, member state, non-EU, Swiss, UK GDPR, Turkish KVKK, cookie law by country, localised

**Example prompts:**
- "What does Garante require that's different from generic GDPR?"
- "What do we need to support the French market beyond GDPR?"
- "ICO cookie guidance is stricter — what features does that imply?"
- "AEPD released new DPO guidance — product implications?"
- "Can we sell into Switzerland? What localization do we need?"
- "Italian-language requirements for privacy policy — what features cover it?"
- "UK post-Brexit divergence — any backlog items?"

**Secondary:** `regulatory-po` (when the jurisdiction implements an EU framework)

## Route: `discovery-voc`

**Triggers:** customer, customers, user, users, feedback, support, tickets, complaint, complaints, pain, pain point, churn, cancellation, cancel, retention, interview, prospect, review, reviews, G2, Capterra, Trustpilot, Reddit, forum, community, onboarding, drop-off, funnel, NPS, CSAT, voice of customer, VoC, survey, signal, signals, insight, insights, want, need, wish

**Example prompts:**
- "What are users complaining about this week?"
- "Synthesize the last 30 days of support emails"
- "Why are people cancelling?"
- "What do G2 reviews of Iubenda say we should build?"
- "Mine r/gdpr for unmet needs"
- "Top pain patterns across all VoC sources"
- "Do we have signal for a <feature> ask?"
- "What did prospects say they'd pay for in the last round of interviews?"

**Secondary:** None (standalone for MVP)

## Route: Full Pipeline (All Three)

**Triggers:** quarterly plan, quarterly backlog, roadmap refresh, backlog refinement, grooming session, product plan, product strategy, "what should we focus on next quarter", strategic review, health check

**Example prompts:**
- "Run a quarterly backlog refresh"
- "Give me a Q3 product plan"
- "Full backlog grooming session"
- "What's the biggest thing we should work on next?"
- "Strategic review of the backlog"

## Route: Collaborative (Model C)

**Triggers:** "should we build X", "is X worth it", "pros and cons of shipping Y", "what would it take to add Z"

Each specialist contributes their lens:
- `regulatory-po`: does this serve a regulatory need? is it mandatory or optional?
- `localization-po`: does this unlock a new market? is it market-specific?
- `discovery-voc`: is there customer signal? how strong?

Lead PO synthesizes into a go/no-go with a reasoned priority.

## Direct Invocation (Skip the Lead PO)

Users can invoke specialists directly for focused queries:
- `/po-os:regulatory-po` — e.g., "What does AI Act Article 50 require?"
- `/po-os:localization-po` — e.g., "Garante enforcement priorities 2026"
- `/po-os:discovery-voc` — e.g., "Synthesize this week's support emails"

Direct invocation skips routing logic but still uses the full Startup Protocol (config load + memory).

## Keywords That Go To Lead PO (default)

Any of these trigger the orchestrator, which then routes:
- "backlog", "prioritize", "priority", "product owner", "PO", "refinement"
- "what should we build", "what's next", "roadmap"
- "user story", "acceptance criteria", "PRD"
