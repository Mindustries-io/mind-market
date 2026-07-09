---
name: setup
description: "Support OS configuration and onboarding. Use when the user asks to 'set up support', 'configure support os', 'support os setup', 'sup setup', 'configure my support desk', 'change support settings', 'update refund policy', 'set SLA', 'support settings', or mentions setting up support-os for the first time."
---

# Support-OS Setup Wizard

You configure the Support OS. Be conversational — ask in small batches, accept "skip" for anything optional, and confirm before writing.

## Steps

### 1. Check for an existing config

Read `~/.claude/plugins/data/support-os/config.json`.
- **Exists:** show a compact summary of the active profile and ask what to change (jump to that section only), or offer to add a new profile.
- **Missing:** create the directory if needed and run the full wizard below.

### 2. Full wizard (ask in this order, 2-3 questions per message)

1. **Product** — product name, one-line pitch, domain. (Example placeholders: "Aurora", "compliance tracking for small teams", "aurora.example.com".)
2. **Channels** — where support arrives: email, in-app chat, social/X, community, other. Multiple allowed.
3. **Helpdesk tool** — which tool, if any (Zendesk, Help Scout, Freshdesk, Intercom, plain inbox, none). Explain: used only to interpret CSV exports — support-os never connects to it or asks for credentials.
4. **Tone / voice** — 2-3 adjectives or a sentence ("friendly, direct, no corporate filler"), preferred signoff, and whether to reuse the marketing-os brand voice if that plugin is installed.
5. **SLA expectations** — target first-response time (hours) and target resolution time; business days only?
6. **Refund policy summary** — 1-3 sentences of the actual policy, the refund window (days), and the amount/conditions above which the user personally decides.
7. **Escalation list** — what always needs the user's own decision (default: refunds above threshold, legal threats, security reports).
8. **Knowledge base** — where the help center lives (URL or repo path), and an optional local inventory file path.
9. **Cross-OS flags** — is `po-os` installed? Is `marketing-os` installed? (Set `cross_os.po_os` / `cross_os.marketing_os`.)

### 3. Write the config

Build the JSON per `references/config-schema.md`, show it to the user for confirmation, then write it to `~/.claude/plugins/data/support-os/config.json`. Set `created_at`/`updated_at` (ISO dates). Validate it parses as JSON after writing.

### 4. Wrap up

Confirm what was saved and suggest a first task: "Paste a few recent tickets and ask me to triage them, or say 'reply to this customer' with an email."

## Quick-setup mode

If another support-os skill sent the user here mid-task, or the user is in a hurry: ask only product name, channels, and tone; write a minimal config with all other fields at schema defaults; tell the user the rest can be added later via `/support-os:setup`.

## Rules

- Never invent policy: if the user doesn't know their refund policy yet, store an honest placeholder ("no formal policy — decide case by case; escalate all refunds") and flag it.
- Config lives locally under `~/.claude/plugins/data/support-os/` — mention this when writing it.
- Do not overwrite an existing profile without explicit confirmation.
