---
name: setup
description: "Sales OS configuration and onboarding. Use when the user asks to 'set up sales', 'configure sales os', 'sales os setup', 'sos setup', 'configure my offer', 'set my rates', 'change pipeline stages', 'update outreach tone', 'sales settings', 'add to do-not-contact list', or mentions setting up sales-os for the first time."
---

# Sales OS Setup Wizard

You configure Sales OS for a one-person business. Config lives at `~/.claude/plugins/data/sales-os/config.json` (schema: `references/config-schema.md`). Create the directory if needed. Read the existing file first — never clobber other profiles.

## Modes

- **Full wizard** (no args / first run): walk through all sections below.
- **Targeted change** ("change tone to casual", "add acme.example.com to do-not-contact"): read config, apply the one change, confirm what changed.
- **Show config**: print the active profile (mask nothing — it's the user's own local file).

## Wizard flow (one section at a time, suggest sensible defaults, accept "skip")

1. **Business & offer** — business name; services/products sold (name + one-line description each); positioning sentence ("I help X do Y").
2. **Pricing & rates** — currency; rate per service (hourly / daily / fixed / retainer); minimum engagement value; discount floor if any.
3. **ICP (ideal customer profile)** — who buys: industries, company size, buyer roles, geographies; red flags that disqualify a prospect.
4. **Pipeline** — stages (default: `lead → contacted → qualified → proposal → negotiation → won/lost`); pipeline file path (default `~/sales/pipeline.md`); stale threshold in days (default 14); weekly review day (default monday).
5. **Outreach** — tone (e.g. professional / friendly-direct / casual); sign-off; channels (email, LinkedIn, other); daily send cap; do-not-contact list (emails/domains).
6. **Proposals** — standard terms (payment terms, revision rounds, validity days); default tier count (default 3); anything always out of scope.
7. **CRM situation** — none / spreadsheet / HubSpot / Pipedrive / Attio; if a CRM: note that Sales OS works from exports or a read-only MCP server, never credentials.
8. **Cross-OS flags** — detect or ask which siblings are installed: `marketing_os` (brand voice reuse), `legal_os` (contract escalation), `po_os` (win/loss → discovery feed). Set booleans.

## Finish

1. Write the config with `created_at`/`updated_at` (ISO dates) and set `active_profile`.
2. Offer to create the pipeline file from `${CLAUDE_PLUGIN_ROOT}/references/pipeline-template.md` if it doesn't exist.
3. Show a 5-line summary and suggest a first command: `/sales-os:sales-director pipeline review`.

Multiple profiles are supported (`profiles` map keyed by slug); `setup <name>` adds a profile, `setup switch to <name>` changes `active_profile`.
