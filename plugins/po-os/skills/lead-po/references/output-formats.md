# PO-OS Output Formats

Standard response templates. Use the appropriate format based on output type. All specialists must return the **Backlog Item** format as their primary output for composability.

---

## 1. Backlog Item (primary specialist output)

This is the atomic unit of PO-OS work. Every specialist produces this format; the Lead PO scores and writes it to the backlog home (GitHub Issues by default).

```markdown
# {Title — imperative action phrase, ≤80 chars}

**Specialist:** {regulatory-po / localization-po / discovery-voc}
**Date:** {YYYY-MM-DD}
**Product:** {PRODUCT}
**Priority signal (specialist-proposed):** {P0 / P1 / P2 / P3}

## User Story
As a **{persona name}**, I want **{capability}**, so that **{outcome/benefit}**.

## Problem Statement
{2-3 sentences. What pain / obligation / gap does this address? Why does it hurt now?}

## Source Signal
- **Type:** {regulatory / localization / voc-support / voc-churn / voc-review / voc-interview / voc-forum}
- **Source(s):** {Specific citations — regulation article, ticket ID, review URL, interview note, forum post}
- **Date observed:** {YYYY-MM-DD}
- **Strength:** {one source / pattern across 3+ sources / dominant theme}

## Acceptance Criteria
- [ ] **Given** {context}, **when** {action}, **then** {observable outcome}
- [ ] **Given** {context}, **when** {action}, **then** {observable outcome}
- [ ] {Add more as needed — at least 2 required}

## Risks & Assumptions
- **Risks:** {what could go wrong — technical, legal, UX}
- **Assumptions:** {what we're taking as true without validation}
- **Open questions:** {blocking unknowns — triggers `needs-clarification` label if non-empty}

## Priority Signals (for Lead PO scoring)
| Dimension | Level | Notes |
|---|---|---|
| Regulatory urgency | {HIGH/MED/LOW/NONE} | {deadline date if any} |
| Customer pain | {HIGH/MED/LOW/NONE} | {# sources, # affected} |
| Strategic fit | {HIGH/MED/LOW} | {roadmap alignment} |
| Effort | {XS/S/M/L/XL} | {rough estimate} |
| Revenue impact | {HIGH/MED/LOW} | {conversion/retention/expansion} |

## Dependencies
- {Other backlog item #}, {external integration}, {legal review}, {none}

## Suggested Labels
`{category}`, `{priority}`, `{status}`

---
*AI-assisted product recommendation. Human PO review required before commitment.*
```

### How the Lead PO transforms this

1. **Score** using `prioritization_weights` from config (see `${CLAUDE_PLUGIN_ROOT}/skills/setup/references/config-schema.md` for the formula)
2. **Override** the specialist's proposed priority if the score disagrees (with a note)
3. **Check DoR** — any unchecked criterion → `needs-clarification` label
4. **Deduplicate** against existing backlog + memory
5. **Create issue** via `gh issue create` (with user approval)

---

## 2. Quarterly Product Plan (Lead PO synthesis)

For full-pipeline runs.

```markdown
# Quarterly Product Plan — {PRODUCT} — {quarter}
**Date:** {YYYY-MM-DD} | **Personas in scope:** {count} | **Markets:** {list}

## Executive Summary
- {bullet — theme of the quarter}
- {bullet — biggest regulatory pressure}
- {bullet — strongest customer signal}
- {bullet — biggest market opportunity}
- {bullet — top risk if we do nothing}

## Top 10 Prioritized Items

| # | Item | Specialist | Score | Priority | Effort | One-line rationale |
|---|---|---|---|---|---|---|
| 1 | {title} | {spec} | {0-1} | P0 | M | {why now} |

## Regulatory Landscape
{Summary from regulatory-po — key dates, enforcement trends, what's changing}

## Customer Signal Summary
{Summary from discovery-voc — top 3 pain patterns with source counts and examples}

## Localization Gaps
{Summary from localization-po — per-market asks}

## Conflicts & Trade-offs
{Where specialists disagreed — e.g., "Regulatory urgency says ship X, VoC says users don't care yet"}

## Open Questions Requiring Human Decision
1. {question}
2. {question}

## Recommended Next Actions
- {action with owner}
- {action with owner}

---
*AI-assisted quarterly plan. Human PO and stakeholder review required before commitment.*
```

---

## 3. Discovery Insight (discovery-voc standalone output)

When the specialist produces a pattern report rather than individual items.

```markdown
# Discovery Insight — {theme}
**Date:** {YYYY-MM-DD} | **Sources analyzed:** {count} across {types}

## Pattern
{1-sentence description of the pattern}

## Evidence
| Source | Type | Date | Excerpt / signal |
|---|---|---|---|
| {id} | {support/review/etc.} | {date} | {short quote or summary} |

## Interpretation
{2-3 paragraphs — what it means, why it's happening, how strong the signal is}

## Backlog Implications
Derived items (each rendered as a separate Backlog Item for the Lead PO to score):
1. {title of derived item}
2. {title of derived item}

## Validation Needed
{Any claims that need further research before acting — e.g., "Sample too small, interview 5 more to confirm"}

---
*AI-assisted synthesis. Sample sizes and selection biases are disclosed when known.*
```

---

## 4. Regulatory Brief (regulatory-po standalone output)

When the regulation is too big for a single backlog item and needs a digestion pass first.

```markdown
# Regulatory Brief — {regulation/topic}
**Date:** {YYYY-MM-DD} | **Frameworks:** {list} | **Effective date:** {date or "in force"}

## TL;DR
{2-sentence summary for a non-lawyer PO}

## What Changed / What Applies
{Key articles/provisions — plain language}

## Who It Affects
{Which of our personas, markets, verticals}

## Product Implications
{How this maps to features, data models, UX}

## Derived Backlog Items
1. {title} — {one-line rationale}
2. {title} — {one-line rationale}

## Timeline
| Date | Milestone | Implication |
|---|---|---|
| {date} | {milestone} | {what we need ready by then} |

## Sources
- {citation with link}
- {citation with link}

---
*AI-assisted regulatory interpretation. Consult qualified legal counsel for binding advice.*
```

---

## 5. Localization Brief (localization-po standalone output)

When assessing a new market or a jurisdictional deep-dive.

```markdown
# Localization Brief — {country/jurisdiction}
**Date:** {YYYY-MM-DD} | **DPA:** {name} | **Language(s):** {list}

## Market Readiness: {GO / CONDITIONAL / NO-GO}

## Local Framework Overlay
{What this jurisdiction adds/modifies on top of EU baseline}

## DPA Enforcement Priorities
{What the authority is currently fining for / focusing on}

## Language & Locale Requirements
{Which of our strings / policies / emails need translation; any mandatory local wording}

## Derived Backlog Items
1. {title} — {one-line rationale}

## Dependencies
- {Legal review needed on X}
- {Translation partner required}
- {Local DPO consultation}

## Sources
- {citation with link}

---
*AI-assisted jurisdictional analysis. Local counsel review recommended for market launch.*
```

---

## General Rules

1. **Every output ends with the AI-assisted disclaimer** — PO outputs shape engineering work; human judgment is the safety net
2. **Cite sources with date** — freshness matters; regulations evolve
3. **Prefer tables for comparatives** — scannable
4. **Bold the decision points** — most readers skim
5. **Never mark `ready`** if any DoR criterion is unchecked — use `needs-clarification` instead
6. **Use the user's feedback rules** — `ready` label only when truly ready; see `dor_criteria` in config
