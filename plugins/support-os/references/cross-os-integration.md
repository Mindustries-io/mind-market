# Cross-OS Integration Guide

Support-OS runs alongside sibling OSs from the same family. Check the `cross_os` flags in the config before any cross-invocation. **If a sibling is missing, always fall back natively — never fail, never block the task.**

## marketing-os → brand voice reuse

**Who:** `response-drafter`, `kb-writer`.

**When `cross_os.marketing_os` is true AND `voice.use_marketing_os_voice` is true** (the config's explicit opt-in — when it is false, support-os's own `voice` settings apply even with marketing-os installed): resolve the marketing-os data directory with the same resolution order (Data directory section of `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`) with `marketing-os` in place of `support-os` — the selection triggers (env var, `./os-data/`) are shared, so the selected base matches support-os's. Read the marketing-os `config.json` from that selected location first. If it is missing there, check the lower-priority marketing-os locations only to detect legacy data; if found, use it but say so explicitly (e.g. "using the marketing-os config from its home-directory location") and suggest `/marketing-os:setup migrate`. Take the active brand's voice/tone settings (tone, vocabulary, do/don't lists) and use them for replies and articles. State the source: "Using marketing-os brand voice."

**Fallback (file missing, unparseable, or no voice fields):** use the `voice` section of the support-os config; if that's empty too, default to friendly-professional first-person and suggest `/support-os:setup`. Note the fallback in one line, then proceed.

**Deeper marketing questions** (positioning, messaging strategy) are marketing-os's job — suggest its `cmo` skill rather than improvising.

## po-os → Discovery Insight feed-forward

**Who:** `insights-analyst`.

po-os's `discovery-voc` agent synthesizes voice-of-customer signal into backlog input. Support tickets are its richest source — so when `cross_os.po_os` is true, `insights-analyst` emits product-relevant findings in the exact shape `discovery-voc` consumes:

### Discovery Insight format

```markdown
## Discovery Insight: {verb-phrase pattern name}
- **Type:** pain point | feature request | bug hotspot | churn signal
- **Signal strength:** weak (n=1-2, hypothesis) | medium (n=3-5, pattern forming) | strong (n=6+, confirmed)
- **Occurrences:** {n} across {source count} sources — {date range}
- **Sources:** support tickets (anonymized refs: ticket #{id} {date}, requester-{nn})
- **Persona fit:** {which configured persona, if known — else "unknown"}
- **Addressability:** product fix | docs fix | process fix | not addressable
- **Representative quote (anonymized, max 1):** "{quote}"
- **Suggested framing:** {one sentence: the problem statement a PO would write}
```

Conventions mirrored from po-os (`discovery-voc`): verb-phrase pattern names, sample-size-based strength rubric, mandatory source citation, addressable/non-addressable separation, anonymized quotes, "no signal" as a valid answer.

### Hand-off mechanics

1. **In the response:** output Discovery Insight blocks in a "Product signal" section so the user can paste them into a po-os session ("give this to discovery-voc").
2. **In memory:** store each as `sup: {PRODUCT} discovery-insight: {pattern} — n={count} — {strength} — for: po-os`. po-os agents searching across namespaces will find them; support-os keeps writing only under `sup:` (namespace discipline — each OS owns its prefix).
3. **Live delegation (optional):** if the user asks to push signal into the backlog now, the Support Lead may delegate: `Agent(subagent_type: "po-os:discovery-voc", prompt: "Synthesize these support-ticket Discovery Insights into backlog candidates: <insights>")`. Only with user approval — backlog creation is po-os's decision surface, not ours.

**Fallback (`cross_os.po_os` false or agent unavailable):** present the same findings in the same format directly to the user, labeled "Product signal (po-os not installed — keep for your backlog)". The analysis never degrades; only the hand-off does.

## Memory namespace separation

| Plugin | Prefix | Example |
|---|---|---|
| support-os | `sup:` | `sup: Aurora trend: confused-by-invoice-line-items — n=7 — strong — 2026-06` |
| po-os | `po:` | `po: Aurora discovery pattern: … — strength: strong` |
| marketing-os | `mos:` | `mos: Aurora voice: friendly, direct` |

Each OS writes only under its own prefix. Cross-namespace *reads* are allowed when hunting shared context ("has po-os already synthesized this pattern?").

## When NOT to cross-invoke

- Sibling flag is `false` in config — use the fallback, don't probe.
- The question is already within a support-os specialist's scope (don't bounce a reply draft through marketing-os).
- It would duplicate work the sibling already did — search memory first.
