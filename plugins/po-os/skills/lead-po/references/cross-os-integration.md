# Cross-OS Integration Guide

The PO-OS runs alongside `legal-os` and `marketing-os`. These siblings hold knowledge that would be expensive to duplicate. Delegate to their agents where appropriate, then translate their output through a product lens.

**Mechanics:** cross-OS calls go through the **Agent tool** with the sibling's scoped agent name, e.g. `Agent(subagent_type: "legal-os:regulatory-intel", prompt: "...")`. Check the matching `cross_os` flag in config first. **Graceful fallback (always):** if the sibling plugin is not installed, the flag is false, or the call fails, fall back to the PO-OS specialist's native research (`WebSearch` on primary sources) and note the limitation — never fail.

## When to call `legal-os:regulatory-intel`

**Call it when:** you need the raw regulatory horizon scan — what's new, from which authority, with what enforcement signal.

**Example:**
```
Agent(subagent_type: "legal-os:regulatory-intel",
      prompt: "Scan for EU privacy/AI developments in the last 30 days relevant to {FRAMEWORKS}")
```

**How to translate:** `legal-os:regulatory-intel` asks *"is our company compliant?"*. The PO-OS `regulatory-po` re-interprets the same output through the lens *"what does our product need to do so that our customers can be compliant?"*. Same signal, different synthesis.

**Don't call it when:** the user's question is already scoped to product implications ("what AI Act features do we need?") — go direct to `po-os:regulatory-po`, which will decide whether to pull the horizon scan itself.

## When to call `legal-os:dpo`

**Call it when:** you need GDPR-specific deep procedural interpretation (DPIA, DSAR mechanics, lawful basis analysis) to inform a backlog item.

**Example:**
```
Agent(subagent_type: "legal-os:dpo",
      prompt: "What are the minimum DPIA workflow steps under EDPB guidelines? We want to build a DPIA wizard.")
```

**How to translate:** the DPO returns procedural requirements. The PO translates those into UX states, forms, data models, and acceptance criteria.

## When to call `marketing-os:competitive-intel`

**Call it when:** you need competitor *positioning and traffic* signal — what they rank for, their SEO footprint, their share of voice.

**Example:**
```
Agent(subagent_type: "marketing-os:competitive-intel",
      prompt: "Competitive overlap with {COMPETITORS} for {primary market} SME keywords")
```

**How to translate:** marketing-os looks at positioning (what competitors *say*). PO-OS `discovery-voc` looks at customer reviews (what users *complain about*). Combined, you see the delta between promise and delivery — a fertile source of backlog items.

**Don't call it when:** you need product feature gaps — that's `po-os:discovery-voc` mining G2/Capterra, not `competitive-intel`.

## When to call `marketing-os:analytics-reporter`

**Call it when:** you need funnel / activation / retention metrics to test whether a proposed backlog item actually matters to users in-product.

**Example:**
```
Agent(subagent_type: "marketing-os:analytics-reporter",
      prompt: "Onboarding funnel drop-off by step for the last 30 days")
```

**How to translate:** steep drop-offs become problem statements for backlog items. E.g., "70% of users abandon at the scan-config step" → backlog item "Simplify scan configuration".

## When to call `marketing-os:content-director`

Not for backlog creation, but useful as a sanity check: if content-director is already planning an article about a feature we haven't shipped, that's a positioning-vs.-delivery mismatch worth flagging.

## When NOT to cross-invoke

- **If the sibling plugin isn't installed** (check the `cross_os` flag in config). Don't fail — fall back to native research.
- **If the query is already within a specialist's scope.** Don't bounce through legal-os if `regulatory-po` can answer with its own references.
- **If it would duplicate work.** If the Lead PO already ran `regulatory-po`, don't also run `legal-os:regulatory-intel` unless the specialist explicitly asked for it.

## Memory Namespace Separation

| Plugin | Prefix | Example |
|---|---|---|
| legal-os | `los:` | `los: {org} GDPR DPIA completed for feature X` |
| marketing-os | `mos:` | `mos: {brand} keyword cluster added for Q3` |
| po-os | `po:` | `po: {product} backlog #123 created for AI Act deadline` |

Each OS searches only its own namespace by default. When the PO-OS wants shared context (e.g., "did legal already analyze this?"), it can search across prefixes: `query: "GDPR Article 30 product implication"`.

## Example Flow: "AI Act general-purpose rules take effect on {date}"

```
User: "AI Act GP rules kick in on {date} — what do we need to ship?"
Lead PO:
  1. Route → po-os:regulatory-po (primary), po-os:localization-po (secondary)
  2. regulatory-po:
     a. Checks cross_os.legal_os
     b. If true: Agent(subagent_type: "legal-os:regulatory-intel",
                       prompt: "AI Act general-purpose obligations in force {date} — scope and enforcement signal")
        If false/unavailable: WebSearch on European AI Office + EDPB primary sources
     c. Gets back: obligations summary, enforcement likelihood, guidance status
     d. Re-interprets: "For our customers who are GP AI providers, here's what the product
        needs to help them do: A, B, C"
     e. Returns 3 Backlog Items
  3. localization-po:
     a. Checks national competent authorities' stated priorities per market
     b. Returns 0-2 additional items per market if there's jurisdictional variation
  4. Lead PO:
     a. Scores items
     b. Flags the ones with highest (regulatory_urgency * effort_inverse) score
     c. Presents the prioritized list with a "create issues?" confirmation
```

This is the value multiplier: `legal-os` did the regulatory parse once, `po-os` reused it for a different question, and nobody rewrote the regulation from scratch.
