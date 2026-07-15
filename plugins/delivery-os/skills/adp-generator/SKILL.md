---
name: adp-generator
description: Generate or update an Account Development Plan (ADP) for a client account — stakeholder map with Business/Personal/Trust Value scoring, expansion pipeline, next-best actions, and account health. Use whenever the user mentions an ADP, account plan, account development, stakeholder mapping, expansion opportunities for an account, QBR prep, or asks to review or grow a specific client account. Also trigger when the user says "account review", "Tier 1 account", or names a client account and asks what to do next with it.
---

# Account Development Plan (ADP) Generator

## Startup

Read `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md` and follow it before any task: resolve `<DATA_DIR>`, load `config.json` (offering the inline quick-setup if missing), and honour the data-sensitivity rules. Persist outputs to `<DATA_DIR>` (`scorecard.json` / `adp/<account>.md`) as well as presenting them.

Create a living ADP for one client account. The ADP is the operating document an Account Delivery Manager (or the CDO personally) uses to convert delivery quality into expansion revenue. It is reviewed and refreshed at least monthly — so favour concrete, dated, actionable content over generic strategy prose.

## Why this structure

The ADP system rests on three ideas:

1. **Relationships are multithreaded or fragile.** A single champion is a risk, not an asset. The stakeholder map forces explicit coverage across the client's org.
2. **Value is scored per stakeholder, three ways.** *Business Value* (what the engagement delivers for their P&L/KPIs), *Personal Value* (what it does for their career, credibility, workload), and *Trust Value* (how much they'd vouch for us today). A stakeholder can be high on one and dangerously low on another — the plan must show this.
3. **Expansion is a pipeline, not a hope.** Target: at least 2 qualified expansion opportunities per account per quarter, each with a next-best action, owner, and date.

## Gathering inputs

Ask for (or pull from connected tools — CRM, Slack, meeting transcripts, project trackers — if available):

- Account name, tier (Tier 1 / Tier 2), current monthly revenue or retainer, contract/renewal dates
- Active projects, their status and margin position
- Known stakeholders: name, role, disposition
- Recent CSAT or sentiment signals, escalations, open risks
- Any existing expansion ideas or client-stated ambitions

If the user provides only partial information, generate the ADP with clearly marked `[TO CONFIRM]` placeholders rather than inventing facts — the plan will be refined in account reviews. Never fabricate stakeholder names, revenue figures, or client statements.

## Output format

Produce a single Markdown document using exactly this template:

```markdown
# Account Development Plan — [Account]
**Tier:** [1/2] · **Owner (ADM):** [name] · **Exec sponsor:** [name] · **Last updated:** [date] · **Next review:** [date]

## 1. Account snapshot
Current engagement, monthly revenue/retainer, contract dates, strategic context in ≤5 lines.

## 2. Stakeholder map
| Stakeholder | Role | Business Value | Personal Value | Trust Value | Relationship holder | Notes / next touch |
|---|---|---|---|---|---|---|
Score each value H/M/L with a one-line justification in Notes. Flag single-threaded relationships explicitly.

## 3. Account health
- CSAT (latest, target ≥ 4.0/5.0), open risks by severity, escalation history, delivery margin position
- One-line RAG verdict: Green / Yellow / Red — and why

## 4. Expansion pipeline
| Opportunity | Value (est.) | Stage | Qualified? | Champion | Next-best action | Owner | Due |
|---|---|---|---|---|---|---|---|
Minimum 2 qualified opportunities per quarter. "Qualified" means: named champion, articulated client problem, and plausible budget path.

## 5. Next-best actions (30 days)
Numbered list. Each: action, owner, date, which stakeholder or opportunity it advances.

## 6. Risks to the relationship
What could shrink or lose this account, with mitigation per item.
```

## Update mode

When given an existing ADP, don't regenerate from scratch: preserve history, update scores and stages, move completed actions to a short changelog at the bottom, and call out what changed since the last review — that delta is what the account review meeting discusses.

## Tone

Write like an operator, not a consultant: short sentences, no filler, every claim tied to a fact or marked `[TO CONFIRM]`.
