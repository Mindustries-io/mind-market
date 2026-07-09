# Sales Director — Output Formats

Use these when synthesizing multi-agent results. Fast-mode relays keep the specialist's own format.

## Weekly Pipeline Review

```
# Pipeline Review — {week of}

**Total open:** {currency}{value} across {n} deals · **Won:** {n} ({value}) · **Lost:** {n}

| Stage | Deals | Value | Δ vs last week |
|---|---|---|---|

## Needs attention
| Deal | Issue | Suggested move |
|---|---|---|

## Top 3 actions this week
1. ...
```

## Call-Prep Briefing

```
# Call Prep — {prospect} @ {company}, {date}

**Deal:** {stage} · {value} · last touch {date}
**History:** 3-5 bullets from pipeline + memory
**Their likely agenda:** ...
**Our goal for the call:** one sentence
**Talking points:** 3 bullets (research-backed)
**Landmines:** objections/risks
**Next step to propose:** ...
```

## Win/Loss Pattern Summary

```
# Win/Loss Analysis — last {period}

**Record:** {won}W / {lost}L · win rate {pct}

| Reason category | Wins | Losses | Signal |
|---|---|---|---|

**Pattern:** 1-2 sentences.
**Recommendation:** one concrete change.
{if po-os installed: "Feed-forward: {n} loss reasons emitted for po-os discovery-voc."}
```

## Deal Handoff Block (between agents)

When piping context from one specialist to another, pass:

```
Deal: {name} · Stage: {stage} · Value: {value}
Contact: {name, role} · Last touch: {date}
Context: {2-3 lines}
Ask: {what the receiving agent must produce}
```
