# Proposal / Quote / SOW Templates

For `proposal-drafter`. Build from config: `OFFER`, `RATES` (`pricing`), `TERMS` (`proposal`). Currency and rates exactly as configured — never invent numbers.

## Proposal structure

```markdown
# Proposal — {Client} × {Business Name}
_{date} · valid until {date + validity_days}_

## The problem
2-4 sentences in the client's words. If you can't write this section concretely, the deal isn't ready for a proposal.

## Proposed outcome
What will be true when the work is done — outcomes, not activities.

## Options
### Option A — {lean name} · {currency}{price}
What's included (countable deliverables) · timeline
### Option B — {recommended name} · {currency}{price}  ← Recommended
...
### Option C — {premium name} · {currency}{price}
...

## What's not included
Explicit out-of-scope list (from `always_out_of_scope` + deal-specific extras).

## How we'd work
Cadence, communication channel, revision rounds (capped), who does what.

## Investment & terms
Payment terms ({payment_terms}) · validity · what happens on acceptance.

## Next step
One sentence, one action.
```

### Tiering rules

- Default 3 tiers (`tiers_default`). Lean = smallest honest version; Recommended = what you'd actually advise; Premium = recommended + high-margin extras. Price gap between tiers: roughly 1.6-2.2x.
- Present premium first in the table order (anchoring), but mark Recommended clearly.
- Never pad a tier with filler deliverables to justify price — cut price or add real value.

## Quote structure (lightweight)

Scope summary (3 lines) · line items table (item, qty/unit, rate, subtotal) · total · validity date · payment terms. Use when the client already knows what they want.

## SOW structure

```markdown
# Statement of Work — {project} for {Client}
1. Parties & effective date
2. Deliverables — each with a "Done means:" acceptance line
3. Out of scope — explicit list
4. Timeline & milestones — dates or dependency-relative
5. Client responsibilities — inputs, access, review turnaround (with deadline defaults)
6. Fees & payment schedule — tied to milestones ({payment_terms})
7. Revisions & change requests — N rounds included; extra work via written CR with price/timeline delta before work starts
8. Standard terms — from config `standard_terms`
```

### Scope-creep defenses (must appear in every SOW)

- Revision cap with a per-extra-round rate.
- "Client review turnaround: X business days; delays shift the timeline day-for-day."
- CR procedure: no unpriced work, ever; verbal agreements confirmed in writing before execution.
- Meeting load cap for retainers/longer engagements.

## Legal boundary

Sections 1-8 are commercial scaffolding, not bespoke contract drafting. Custom liability, indemnity, IP assignment, or jurisdiction clauses → `legal-os:contract-manager` if installed (`CROSS_OS.legal_os`), otherwise recommend human lawyer review. See `${CLAUDE_PLUGIN_ROOT}/references/cross-os-integration.md`.
