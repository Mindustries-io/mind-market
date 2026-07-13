---
name: setup
description: "Sales OS configuration and onboarding. Use when the user asks to 'set up sales', 'configure sales os', 'sales os setup', 'sos setup', 'configure my offer', 'set my rates', 'change pipeline stages', 'update outreach tone', 'sales settings', 'add to do-not-contact list', or mentions setting up sales-os for the first time."
---

# Sales OS Setup Wizard

You configure Sales OS for a one-person business. Config lives at `<DATA_DIR>/config.json` (`<DATA_DIR>` = resolved per the Data directory section of `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`; schema: `references/config-schema.md`). Create the directory if needed. Read the existing file first — never clobber other profiles.

## Modes

- **Full wizard** (no args / first run): walk through all sections below.
- **Targeted change** ("change tone to casual", "add acme.example.com to do-not-contact"): read config, apply the one change, confirm what changed.
- **Show config**: print the active profile (mask nothing — it's the user's own local file).

## Wizard flow (one section at a time, suggest sensible defaults, accept "skip")

1. **Business & offer** — business name; services/products sold (name + one-line description each); positioning sentence ("I help X do Y").
2. **Pricing & rates** — currency; rate per service (hourly / daily / fixed / retainer); minimum engagement value; discount floor if any.
3. **ICP (ideal customer profile)** — who buys: industries, company size, buyer roles, geographies; red flags that disqualify a prospect.
4. **Pipeline** — stages (default: `lead → contacted → qualified → proposal → negotiation → won/lost`); pipeline file path (default `<DATA_DIR>/pipeline.md`); stale threshold in days (default 14); weekly review day (default monday).
5. **Outreach** — tone (e.g. professional / friendly-direct / casual); sign-off; channels (email, LinkedIn, other); daily send cap; do-not-contact list (emails/domains).
6. **Proposals** — standard terms (payment terms, revision rounds, validity days); default tier count (default 3); anything always out of scope.
7. **CRM situation** — none / spreadsheet / HubSpot / Pipedrive / Attio; if a CRM: note that Sales OS works from exports or a read-only MCP server, never credentials.
8. **Connectors (optional)** — "Do you use any connected tools I should pull live data from? (optional, Enter to skip)". Store the answer under `connectors` (`enabled` defaults to true; names the user mentions go into `connectors.preferred`).
9. **Cross-OS flags** — detect or ask which siblings are installed: `marketing_os` (brand voice reuse), `legal_os` (contract escalation), `po_os` (win/loss → discovery feed). Set booleans.

## Finish

1. Write the config to `<DATA_DIR>/config.json` with `created_at`/`updated_at` (ISO dates) and set `active_profile`.
2. Offer to create the pipeline file from `${CLAUDE_PLUGIN_ROOT}/references/pipeline-template.md` if it doesn't exist.
3. Show a 5-line summary, **stating the absolute path where the config was actually written**. If it fell back to location 3 (the home path), add: "If you're running in Cowork, connect a business folder and re-run setup so your data lands in `./os-data/sales-os/` inside it." Then suggest a first command: `/sales-os:sales-director pipeline review`.

Multiple profiles are supported (`profiles` map keyed by slug); `setup <name>` adds a profile, `setup switch to <name>` changes `active_profile`.

## Migrating your data (`/sales-os:setup migrate`)

When invoked with `migrate` — or whenever the user asks to move, consolidate, or centralise their data:

1. **Scan** all three resolution locations for sales-os data files (config.json plus any data files listed in the schema/GUIDE). Report what exists where: absolute path, files found, last-modified dates.
2. **Ask for the target**, recommending in this order:
   - `$OS_HUB_DATA_DIR/sales-os/` — best for "same data in every future session". Suggest pointing it at a synced folder (OneDrive, Dropbox, etc.) and remind the user to persist the variable in Claude Code `settings.json` under `"env"` so every future session sees it. In Cowork, connect that same synced folder and the `./os-data/` path resolves to the same data.
   - `./os-data/sales-os/` in the current working folder — right when this folder IS the business folder they connect in Cowork.
   - `~/.claude/plugins/data/sales-os/` — fine for single-machine, Claude Code-only use.
3. **Copy** (never move yet) every data file to the target, creating directories as needed. On a name collision, keep the newer file and say so. List exactly what will be copied before doing it.
4. **Verify** by reading `config.json` back from the target.
5. Only after verification, **offer** to rename the originals with a `.migrated` suffix so future sessions resolve unambiguously. Never delete without an explicit yes.
6. If the target is inside a git repository, remind the user to add `os-data/` to `.gitignore`.
7. **Post-migration review:** walk through the migrated `config.json` and confirm anything vague — empty fields, fields introduced by newer plugin versions (e.g. `connectors`), or values that look stale (old dates, placeholder text, competitors/rates that may have changed). One short confirm-or-update question per flagged item; write the refreshed config to the new location.
8. **Empty target, no source:** if the chosen target is empty and the scan found no sales-os data in any location, skip migration and run the normal setup wizard instead, writing its output to the target.
