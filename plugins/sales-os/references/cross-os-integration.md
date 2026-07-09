# Cross-OS Integration Guide

Sales OS runs alongside sibling OS plugins. Check the `cross_os` flags in config before any cross-invocation; **if a sibling is missing, fall back gracefully — never fail.** This file mirrors the convention of po-os's `cross-os-integration.md`.

## The sales-os ↔ marketing-os boundary

**sales-os is 1-to-1** (named prospects, individual deals, personal outreach, proposals). **marketing-os is 1-to-many** (audience, content, campaigns, newsletters). The handoff points:

| Signal | Direction | How |
|---|---|---|
| Brand voice | marketing → sales | `outreach-writer` reads marketing-os config (`~/.claude/plugins/data/marketing-os/config.json`, active profile → `brand_voice`) and merges tone/avoid-words; sales-os `TONE` wins on conflict. Fallback: sales-os `TONE` alone. |
| A prospect asks to join the newsletter / campaign requests | sales → marketing | Redirect the user to `/marketing-os:cmo`. |
| Marketing-qualified lead becomes a named deal | marketing → sales | User adds it as a deal; `pipeline-manager` takes over. |

## Feed-forward: win/loss reasons → po-os discovery-voc

Win/loss reasons are voice-of-customer signal — losses especially are ground truth about unmet needs. Convention:

1. **Always** (regardless of po-os): `pipeline-manager` stores each closed deal in memory as
   `sal: {BUSINESS} win/loss: {deal} — {won|lost} — {category}: {detail}`
   using the fixed category vocabulary from `pipeline-template.md` (`price`, `timing`, `fit`, `competitor`, `feature-gap`, `no-decision`, `relationship`, `other`).
2. **When `cross_os.po_os` is true**, also emit each loss reason as a VoC signal block that `po-os:discovery-voc` can consume directly (it clusters signals by theme, source type, and recency):

   ```
   VoC signal — source: sales win/loss log
   Date: {ISO} · Source type: lost-deal debrief · Persona: {buyer role from ICP}
   Pain/reason: {category} — {detail}
   Verbatim: "{prospect's stated reason, anonymized}" (if known)
   Strength: single deal — treat as weak signal until pattern (3+ similar losses)
   ```

   Present these blocks in the win/loss output (and in memory) so a later `po-os:discovery-voc` run finds them via cross-prefix search (e.g. `"win/loss reason feature-gap"`). `feature-gap` and `competitor` losses are the highest-value feed — they are direct backlog candidates.
3. **Anonymize:** strip contact names/emails from anything emitted; company name only if the user is comfortable.
4. **If po-os is not installed:** skip step 2 silently; the `sal:` memory entries preserve the data for later.

## When to point to legal-os contract-manager

**Point there when:** a proposal/SOW needs terms beyond the configured standard template — custom liability caps, indemnities, IP assignment, non-standard jurisdiction, DPAs.

**Example:** "Client wants unlimited liability and IP transfer" → `proposal-drafter` drafts the commercial sections, then: "For these clauses, run `/legal-os:contract-manager review these terms` (legal-os detected)." If legal-os is missing: recommend human lawyer review — do not draft bespoke clauses natively.

**Don't point there when:** the standard terms from config cover it — that's normal proposal work.

## When NOT to cross-invoke

- Sibling not installed (`cross_os` flag false) → native fallback, one-line note, move on.
- The query is already in a sales-os specialist's scope — don't bounce through marketing-os for a 1-to-1 email.
- It would duplicate work already done this session.

## Memory namespace separation

| Plugin | Prefix | Example |
|---|---|---|
| sales-os | `sal:` | `sal: Acme Studio win/loss: Aurora rollout — lost — feature-gap: no SSO` |
| marketing-os | `mos:` | `mos: Acme Studio keyword cluster added for Q3` |
| po-os | `po:` | `po: Aurora discovery pattern: SSO demand — strength: medium` |
| legal-os | `los:` | `los: Acme Studio MSA reviewed for client X` |

Each OS searches its own namespace by default; cross-prefix searches are allowed for shared context (e.g. po-os searching for `"win/loss"` signals emitted by sales-os).
