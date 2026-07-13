---
name: setup
description: "Support OS configuration and onboarding. Use when the user asks to 'set up support', 'configure support os', 'support os setup', 'sup setup', 'configure my support desk', 'change support settings', 'update refund policy', 'set SLA', 'support settings', or mentions setting up support-os for the first time."
---

# Support-OS Setup Wizard

You configure the Support OS. Be conversational — ask in small batches, accept "skip" for anything optional, and confirm before writing.

## Steps

### 1. Check for an existing config

Read `<DATA_DIR>/config.json` (`<DATA_DIR>` = resolved per the Data directory section of `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`).
- **Exists:** show a compact summary of the active profile and ask what to change (jump to that section only), or offer to add a new profile.
- **Missing:** create the directory if needed and run the full wizard below.

### 2. Full wizard (ask in this order, 2-3 questions per message)

1. **Product** — product name, one-line pitch, domain. (Example placeholders: "Aurora", "compliance tracking for small teams", "aurora.example.com".)
2. **Channels** — where support arrives: email, in-app chat, social/X, community, other. Multiple allowed.
3. **Helpdesk tool** — which tool, if any (Zendesk, Help Scout, Freshdesk, Intercom, plain inbox, none). Explain: used to interpret CSV exports, or for read-only pulls when the tool's MCP connector is present in the session and `connectors.enabled` is true — support-os never asks for credentials or API keys.
4. **Tone / voice** — 2-3 adjectives or a sentence ("friendly, direct, no corporate filler"), preferred signoff, and whether to reuse the marketing-os brand voice if that plugin is installed.
5. **SLA expectations** — target first-response time (hours) and target resolution time; business days only?
6. **Refund policy summary** — 1-3 sentences of the actual policy, the refund window (days), and the amount/conditions above which the user personally decides.
7. **Escalation list** — what always needs the user's own decision (default: refunds above threshold, legal threats, security reports).
8. **Knowledge base** — where the help center lives (URL or repo path), and an optional local inventory file path.
9. **Connectors (optional)** — "Do you use any connected tools I should pull live data from? (optional, Enter to skip)" — e.g. Zendesk, Intercom, HubSpot. Store names in `connectors.preferred`; `connectors.enabled` defaults to true. Skipping is fine — exports/paste remain the default.
10. **Cross-OS flags** — is `po-os` installed? Is `marketing-os` installed? (Set `cross_os.po_os` / `cross_os.marketing_os`.)

### 3. Write the config

Build the JSON per `references/config-schema.md`, show it to the user for confirmation, then write it to `<DATA_DIR>/config.json`. Set `created_at`/`updated_at` (ISO dates). Validate it parses as JSON after writing.

### 4. Wrap up

In the final confirmation, state the absolute path the config was actually written to. If it fell back to location 3 (the home directory), add: "If you're running in Cowork, connect a business folder and re-run setup — approve creating `os-data/` inside it, since location 2 is only selected once `./os-data/` exists; your data then lands in `./os-data/support-os/`." When setup re-runs inside a connected folder, offer to create `./os-data/` before writing so location 2 is deliberately selected.

Confirm what was saved and suggest a first task: "Paste a few recent tickets and ask me to triage them, or say 'reply to this customer' with an email."

## Quick-setup mode

If another support-os skill sent the user here mid-task, or the user is in a hurry: ask only product name, channels, and tone; write a minimal config with all other fields at schema defaults; tell the user the rest can be added later via `/support-os:setup`.

## Rules

- Never invent policy: if the user doesn't know their refund policy yet, store an honest placeholder ("no formal policy — decide case by case; escalate all refunds") and flag it.
- Config lives locally under `<DATA_DIR>` — mention the resolved path when writing it.
- Do not overwrite an existing profile without explicit confirmation.

## Migrating your data (`/support-os:setup migrate`)

When invoked with `migrate` — or whenever the user asks to move, consolidate, or centralise their data:

1. **Scan** all three resolution locations for support-os data files (config.json plus any data files listed in the schema/GUIDE). Report what exists where: absolute path, files found, last-modified dates.
2. **Ask for the target**, recommending in this order:
   - `$OS_HUB_DATA_DIR/support-os/` — best for "same data in every future session". Suggest pointing it at an `os-data` folder inside a synced location (e.g. `<OneDrive>/business/os-data`) and remind the user to persist the variable in `~/.claude/settings.json` under `"env"` so every future session sees it. For Cowork — where the env var is not available — have them connect the folder that CONTAINS `os-data/` as the working folder: location 2 (`./os-data/support-os/`) then resolves to the very same files the env var points to elsewhere.
   - `./os-data/support-os/` in the current working folder — right when this folder IS the business folder they connect in Cowork.
   - `~/.claude/plugins/data/support-os/` — fine for single-machine, Claude Code-only use.
3. **Copy** (never move yet) every data file to the target, creating directories as needed. On a name collision, keep the newer file and say so. List exactly what will be copied before doing it.
4. **Verify** by reading `config.json` back from the target.
5. Only after verification, **offer** to rename the originals with a `.migrated` suffix so future sessions resolve unambiguously. Never delete without an explicit yes.
6. If the target is inside a git repository, remind the user to add `os-data/` to `.gitignore`.
7. **Post-migration review:** walk through the migrated `config.json` and confirm anything vague — empty fields, fields introduced by newer plugin versions (e.g. `connectors`), or values that look stale (old dates, placeholder text, competitors/rates that may have changed). One short confirm-or-update question per flagged item; write the refreshed config to the new location.
8. **Empty target, no source:** if the chosen target is empty and the scan found no support-os data in any location, skip migration and run the normal setup wizard instead, writing its output to the target.
