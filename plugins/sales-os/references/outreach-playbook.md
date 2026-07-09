# Outreach Playbook (1-to-1)

For `outreach-writer`. All outreach is to a named human about a specific, plausible problem. If a draft would work with the name swapped out, it isn't personalized enough — start over.

## Cold opener structure (50-120 words)

1. **Relevance hook (1 sentence):** something true and specific about *them* — a recent post, hire, launch, or observable pain. Researched, never invented.
2. **Bridge (1 sentence):** why that connects to what you do (`OFFER` positioning).
3. **Credibility (optional, 1 sentence):** one concrete outcome, anonymized if needed ("helped a 12-person ops team cut audit prep from 3 days to 4 hours").
4. **Low-friction ask:** a question they can answer from their phone — not "book 30 minutes". Good: "Worth a short chat, or not a priority this quarter?"
5. **Honest sign-off:** real name, real business, easy way to decline.

**Subject lines:** 2-6 words, specific, no clickbait, no fake "Re:". Offer 2-3 options.

## Angle catalog (rotate across touches — never repeat an angle)

| Angle | Use when |
|---|---|
| Observed trigger | Funding, hire, launch, regulation hitting their industry |
| Peer proof | You've solved this exact problem for a similar business |
| Useful artifact | You share something genuinely useful (checklist, teardown) with no strings |
| Direct question | Their public content implies the pain — ask about it |
| Referral/context | A real shared context (event, community, mutual) — only if true |

## Follow-up sequence rules

- Default cadence: day 0 (opener) · day 3 · day 7 · day 14 (breakup). Adjust for deal size — bigger deals get more patience, not more volume.
- **Each touch must add something new** — a different angle, a useful link, a sharper question. "Just floating this to the top" adds nothing; banned.
- **Breakup email:** short, warm, zero guilt. State you'll stop reaching out, leave one door open ("if this becomes a priority, here's where to find me"). Breakups get the highest reply rates — write them well.
- Warm follow-ups (after a call/meeting) lead with the commitment made, then the next step.

## Tone application

- Apply `TONE` from config; if marketing-os is installed, merge its `brand_voice` (tone, avoid_words, style_notes) — sales-os `TONE` wins on conflict since 1-to-1 voice may legitimately differ from campaign voice.
- Write at the reader's altitude: operators get concrete process talk, executives get outcome talk.
- Contractions yes, exclamation marks almost never, emojis only if `TONE` is casual AND the channel is a DM.

## Compliance quick reference (note, not legal advice)

| Jurisdiction | Cold B2B email posture |
|---|---|
| EU/EEA + UK | ePrivacy/GDPR: B2B outreach needs a lawful basis (usually legitimate interest); must identify sender, be relevant to the role, and honor opt-out instantly. Some member states are stricter (opt-in). |
| US | CAN-SPAM: no deceptive subjects/headers, identify yourself, honor opt-outs. |
| Canada | CASL is strict: implied/express consent rules — flag before drafting volume outreach to Canadian prospects. |

When the prospect is in a strict jurisdiction, add a one-line caveat under the draft. The user sends and owns compliance — see `${CLAUDE_PLUGIN_ROOT}/SAFETY.md`.
