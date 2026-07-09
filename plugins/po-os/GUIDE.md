# PO-OS — User Guide

The **Product Owner Operating System** is a multi-agent Claude Code plugin that turns regulatory changes, jurisdictional requirements, and customer signals into a prioritized, DoR-compliant product backlog.

Built for compliance SaaS teams — particularly ObligoBoard — where "what should we build next?" is answered by triangulating three hard-to-reach lenses: what regulators require, what local markets demand, and what customers actually want.

## Architecture

```
┌──────────────────────────────────────────┐
│               Lead PO                    │   ← entry point; routes + synthesizes
│         (orchestrator skill)             │
└─────────────┬────────────────────────────┘
              │
      ┌───────┼────────────────┐
      ▼       ▼                ▼
 regulatory  localization   discovery-voc
    -po         -po              
      │
      │  cross-OS delegation (when enabled)
      ▼
┌────────────────┐     ┌────────────────────────┐
│  legal-os      │     │   marketing-os         │
│  regulatory-   │     │   analytics-reporter   │
│  intel / dpo   │     │   competitive-intel    │
└────────────────┘     └────────────────────────┘
```

Output of every specialist is a **Backlog Item** in a shared format. The Lead PO scores, deduplicates, and (with approval) writes to GitHub Issues.

## Agents

| Agent | Purpose | Slash command |
|---|---|---|
| **Lead PO** | Orchestrator — routes and synthesizes | `/po-os:lead-po` |
| **Regulatory PO** | GDPR / CSRD-ESRS / AI Act / NIS2 / DORA / ePrivacy → backlog | `/po-os:regulatory-po` |
| **Localization PO** | National DPA guidance + jurisdictional overlays → backlog | `/po-os:localization-po` |
| **Discovery & VoC PO** | Customer signals (support, churn, reviews, forums, interviews) → backlog | `/po-os:discovery-voc` |
| **Setup** | Configuration wizard | `/po-os:setup` |

## Getting Started

### 1. Install the plugin

Already installed if `po-os@po-os` appears in `~/.claude/settings.json` under `enabledPlugins`.

### 2. Configure your product

```
/po-os:setup
```

Walk through 11 steps covering product basics, personas, markets, frameworks, competitors, backlog home, DoR, VoC sources, prioritization weights, and cross-OS integration.

Output: `~/.claude/plugins/data/po-os/config.json`

### 3. Run the Lead PO

```
/po-os:lead-po  "Quarterly backlog refresh for Q3"
```

Or ask directly — the Lead PO classifies and routes:

- "What does the AI Act mean for our product?"
- "What's the Italian market missing?"
- "Why are people churning?"

### 4. Approve backlog item creation

The Lead PO drafts Backlog Items and **always asks before writing** to GitHub Issues. Review the draft, tweak, confirm.

## Backlog Item Format

Every specialist produces this format. The Lead PO scores and writes it.

```markdown
# {Title}
**Specialist:** {agent}
**Priority signal:** P0 / P1 / P2 / P3

## User Story
As a {persona}, I want {capability}, so that {outcome}.

## Problem Statement
{2-3 sentences}

## Source Signal
- Type: {regulatory / localization / voc-...}
- Source(s): {citations with URLs / IDs / dates}
- Strength: {weak / medium / strong}

## Acceptance Criteria
- [ ] Given ... when ... then ...
- [ ] ...

## Risks & Assumptions
- Risks: {...}
- Assumptions: {...}
- Open questions: {...}

## Priority Signals
| Dimension | Level | Notes |
| Regulatory urgency | HIGH/MED/LOW | |
| Customer pain | HIGH/MED/LOW | |
| Strategic fit | HIGH/MED/LOW | |
| Effort | XS/S/M/L/XL | |
| Revenue impact | HIGH/MED/LOW | |

## Dependencies
{list or 'none'}

## Suggested Labels
{po:regulatory / po:localization / po:voc}, {p0/p1/p2/p3}, ready / needs-clarification
```

## Prioritization Formula

Configurable via `prioritization_weights` in `config.json` (integers 1-5):

```
score =
    regulatory_urgency * urgency_signal (0-1)
  + customer_pain      * pain_signal    (0-1)
  + strategic_fit      * fit_signal     (0-1)
  + effort_inverse     * (1 - effort_normalized)
  + revenue_impact     * revenue_signal (0-1)

P0: score ≥ 0.80 × max_score
P1: score ≥ 0.60 × max_score
P2: score ≥ 0.40 × max_score
P3: score <  0.40 × max_score
```

## Definition of Ready (DoR)

Every backlog item created by PO-OS must pass the DoR before receiving the `ready` label:

1. User story in "As a … I want … so that …" form
2. At least 2 acceptance criteria (Given/When/Then preferred)
3. Source signal cited (regulation article, ticket ID, review URL, interview note)
4. Risks and assumptions listed
5. T-shirt size estimate (XS/S/M/L/XL)
6. No unresolved open questions

Items with unresolved open questions get `needs-clarification` instead of `ready`.

## Cross-OS Integration

PO-OS is designed to **reuse** knowledge held by sibling plugins rather than duplicate. If `legal-os` is installed:

- `regulatory-po` can call `legal-os:regulatory-intel` for the same horizon scan data, then re-interpret it through a product lens
- Heavy GDPR-procedural questions can be punted to `legal-os:dpo`

If `marketing-os` is installed:

- `discovery-voc` can pull competitive positioning from `marketing-os:competitive-intel`
- Funnel drop-off metrics from `marketing-os:analytics-reporter`

**Configure in setup step 8** — PO-OS specialists check `cross_os.legal_os` / `cross_os.marketing_os` before delegating.

### Namespace separation

| Plugin | Memory prefix |
|---|---|
| legal-os | `los:` |
| marketing-os | `mos:` |
| po-os | `po:` |

Each searches its own prefix by default; cross-plugin searches are opt-in.

## Delegation Models

Borrowed from legal-os:

- **Hub-and-Spoke (A):** Lead PO → one specialist → done. Most common.
- **Pipeline (B):** Lead PO → specialist 1 → specialist 2. Rare for PO-OS MVP.
- **Collaborative (C):** Lead PO → 2-3 specialists in parallel → synthesis. Used for "should we build X?" and full-pipeline quarterly runs.

## Typical Workflows

### A. Regulatory change lands

1. User: "AI Act Art. 50 transparency rules take effect in 6 months — what do we need?"
2. Lead PO → `regulatory-po`
3. `regulatory-po` consults `ai-act-product-mapping.md`, optionally calls `legal-os:regulatory-intel` for the latest
4. Returns 2-3 Backlog Items (e.g., "End-user AI disclosure component", "Synthetic-content watermark API", "Localized disclosure copy library")
5. Lead PO scores; high urgency due to deadline
6. User approves → GitHub Issues created

### B. Market expansion assessment

1. User: "Can we launch in Germany next quarter?"
2. Lead PO → `localization-po`
3. `localization-po` runs through `localization-checklist.md` against current product state
4. Returns **Localization Brief** with GO/CONDITIONAL/NO-GO + derived items
5. Lead PO presents with per-item effort and dependencies

### C. Customer signal synthesis

1. User: "What are users saying this month?"
2. Lead PO → `discovery-voc`
3. `discovery-voc` reads enabled sources (support email log, G2 reviews for Iubenda, r/gdpr)
4. Clusters into named patterns with strength scores
5. Returns **Discovery Insight** with derived items
6. Strong-signal items go to the backlog; weak hypotheses go to memory for future triangulation

### D. Full quarterly refresh

1. User: "Run a Q3 backlog refresh"
2. Lead PO → all three specialists in parallel
3. Synthesizes into **Quarterly Product Plan** output with top-10 prioritized items

## Configuration File

`~/.claude/plugins/data/po-os/config.json` — schema documented in `skills/setup/references/config-schema.md`.

## Data Directory

- `~/.claude/plugins/data/po-os/config.json` — product profile
- `~/.claude/plugins/data/po-os/backlog/index.json` — local backlog cache (mirror of GitHub issues)
- `~/.claude/plugins/data/po-os/discovery-log.json` — VoC observation append-log

## Evolving the Plugin

MVP roster is 4 agents (Lead + 3 specialists). Candidate v2 additions if/when pain emerges:

- **Backlog Strategist** — dedicated RICE/WSJF scoring, sprint planning (currently handled by Lead PO)
- **Competitive Product** — feature-gap-focused, complements marketing-os's positioning lens
- **UX & Accessibility** — WCAG audits + usability heuristics
- **Technical Product** — API strategy, integration roadmap
- **Metrics & Activation** — KPI ownership

Don't add until there's evidence of bottleneck.

## Known Limitations

- **Regulatory interpretations are AI-assisted, not legal advice.** For binding items, route ambiguous calls through `legal-os:legal-researcher` or qualified counsel.
- **VoC signal quality is stage-dependent.** Early-stage products with thin data will get hypotheses, not facts. PO-OS is honest about sample sizes and biases.
- **GitHub Issues is the default backlog home.** Linear / Jira support is a future addition; for now, use `gh` CLI or the Linear plugin as a complement.
- **No auto-scheduled runs yet.** Use the `schedule` skill or cron configuration to run quarterly refreshes automatically.

## Disclaimer

PO-OS produces AI-assisted product recommendations. Human PO review is required before committing engineering effort. For regulatory-driven items, human legal review is especially important — product acceptance criteria that bake in a wrong interpretation become compliance risk for customers.
