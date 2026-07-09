# Response Patterns — De-escalation & Hard-Case Templates

Templates use `{placeholders}`; adapt to the configured voice, never send raw. "I" = the founder; a one-person business should sound like one person, not a department.

## De-escalation core (applies to every hard case)

1. **Validate first.** Name their experience before anything else: "You're right to be frustrated — losing an hour to this is not okay."
2. **Never mirror anger, never argue tone.** No "there's no need for that language."
3. **No policy-quoting in sentence one.** Policy comes after empathy and facts, if at all.
4. **Own it plainly.** "This is a bug on our side" beats "we apologize for any inconvenience this may have caused."
5. **One concrete next step + a time.** "I'll have an answer for you by Thursday" beats "we'll look into it."
6. **Give them an out that isn't churn.** A workaround, a credit, a direct line back to you.

## Angry customer

```text
Hi {name},

You're right, and I'm sorry — {restate their problem in their terms, no softening}.
{What happened, in one honest sentence. If it's our fault, say so.}

Here's what I'm doing about it:
- {immediate fix or workaround}
- {prevention / follow-up, with a date}

{If warranted: small make-good — extended trial, credit, month free.}
If anything else is off, reply here and it comes straight to me.

{signoff}
```

Avoid: "we apologize for any inconvenience", "as per our policy", "valued customer", conditional apologies ("if you felt…").

## Refund request

**Within policy → approve (draft directly):**
```text
Hi {name},
Done — I've refunded {amount}; it should reach your account within {3-5} business days.
{Optional, only if sincere: one-line ask about what didn't fit.}
If you ever want to give {PRODUCT} another try, this address reaches me directly.
{signoff}
```

**Outside policy or above `user_decides_above` → draft BOTH, user picks:**
- *Approve version:* as above, optionally noting it's an exception: "Our window is {window_days} days, but I'm happy to make an exception here."
- *Decline version:*
```text
Hi {name},
Thanks for asking straight. Our refund window is {window_days} days and this purchase is from {date}, so I can't refund it — but here's what I can do: {alternative: pro-rated credit, cancel-with-no-further-charges, extended access}.
{If their reason was a product gap: acknowledge it honestly.}
{signoff}
```
Never decide the exception yourself; present both with a one-line recommendation.

## Outage / incident apology

```text
Subject: {PRODUCT} outage on {date} — what happened

Hi {name},

Between {start} and {end} {timezone}, {plain-language impact — what customers couldn't do}.
{If data was affected, say exactly what; if not: "No data was lost."}

What happened: {1-2 sentences, no jargon-hiding}.
What's changed: {the fix + the prevention}.

I'm sorry — you rely on {PRODUCT} and {day} we let you down. {Make-good if warranted.}

{signoff}
```

Rules: no minimizing ("minor issue", "brief disruption", "some users may have experienced"). State scope precisely. Send proactively — an apology that arrives before the complaint is worth double. If personal data was breached, that's a `user-decision` with legal implications — do not draft breach notifications as routine support mail.

## Bug acknowledgment

Confirm it's a bug, thank them concretely, give the honest status ("reproduced, in the queue" / "can't promise a date yet"), offer the workaround if one exists, promise to notify on fix — then actually track that promise (`sup: {PRODUCT} promised-followup: {requester-id} — {issue} — {date}`).

## Feature request decline (or "not yet")

Thank + restate the ask to show it was understood; be honest about status ("not on the near-term list" beats silence or fake "great idea, soon!"); mention the closest existing capability; log the request for `insights-analyst` clustering.

## Canned-response library conventions

- **Memory key:** `sup: {PRODUCT} canned: {slug} — {one-line purpose}` with the full template text in the body.
- **Slugs:** kebab-case, intent-shaped: `refund-within-policy`, `bug-ack-with-workaround`, `password-reset-steps`.
- **Placeholders:** `{name}`, `{amount}`, `{date}`, `{article_link}` — always curly-braced so unfilled ones are visible before sending.
- **Hygiene:** on library review, flag entries older than 6 months or referencing changed features; merge near-duplicates; keep the library under ~25 entries — past that, the top recurring answers belong in the KB instead.
