---
name: setup
description: "Finance OS configuration and onboarding. Use when the user asks to 'set up finance', 'configure finance os', 'finance os setup', 'fin setup', 'set up my business finances', 'configure cfo', 'change finance settings', 'update invoice template', 'add tax jurisdiction', or mentions setting up finance-os for the first time."
---

# Finance OS Setup Wizard

You configure the business profile that every Finance OS agent relies on. Config lives at `<DATA_DIR>/config.json` (`<DATA_DIR>` = resolved per the Data directory section of `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`) — read it first if it exists and preserve anything already set; create the directory and file if not.

Use AskUserQuestion for structured steps. If the user asked for one specific change ("add jurisdiction FR", "change payment terms to 14 days"), skip the wizard and make the targeted edit, then show the updated section.

## Wizard Steps

### 1. Business Basics
1. **Business name** and **legal form** (e.g. "Acme Studio", sole proprietorship).
2. **Entity type** — `sole_prop` / `llc_equivalent` / `corporation` / `other` (drives tax-planner).
3. **Base currency** — ISO code (EUR, USD, GBP, ...).
4. **Business address** and **email** (appear on invoices).
5. **Tax ID(s)** — label + value per jurisdiction (e.g. "VAT ID: …"). Optional; invoices get a placeholder if missing.

### 2. Tax Jurisdictions & VAT
1. **Primary jurisdiction** (country, plus state/canton if relevant) and any **additional jurisdictions**.
2. **VAT/GST registered?** If yes: standard rate, scheme (standard / cash accounting / flat-rate / small-business exemption), filing frequency (monthly/quarterly/annual).
3. **Known tax calendar entries** — any deadline dates the user's accountant already gave them (optional; tax-planner fills the rest).

### 3. Revenue Model
1. **Model** — services (hourly/day rate/project), products/SaaS, mixed.
2. **Recurring revenue items** — name, amount, billing frequency, billing day (e.g. "Aurora Pro subscriptions — 2,400/mo MRR", "Retainer, Acme Studio — 1,500/mo on the 1st").
3. **Typical rates** — day rate / hourly rate / typical project size (feeds pricing-analyst).

### 4. Recurring Costs
For the cash-flow model: name, amount, frequency, billing day for each recurring cost (software, hosting, insurance, contractors, owner draw). Rough is fine — refinable later.

### 5. Invoicing
1. **Payment terms** in days (default 14).
2. **Numbering scheme** (default `{YYYY}-{seq}`) and the **next number**.
3. **Payment details** for the invoice footer (IBAN/account/payment link — stored locally only).
4. **Footer notes** — late-fee clause, exemption wording from the accountant, etc.
5. **Chase timing** — days after due for reminder steps (default 3 / 14 / 30).

### 6. Cash-Flow Settings
1. **Minimum cash buffer** — amount or "1 month of costs" (default) below which forecasts alarm.
2. **Typical payment lag** — days clients take beyond the due date (default 7).

### 7. Pricing Context (optional)
Competitors/alternatives with pricing-page URLs, and a **max discount** percentage guardrail (default 15%).

### 8. Cross-OS Flags
Which sibling OSs are installed: `legal_os` (jurisdiction conventions, escalation), `marketing_os`, `po_os`. Yes/no each; specialists fall back to native research when absent.

### 9. Connectors (optional)
"Do you use any connected tools I should pull live data from? (optional, Enter to skip)" — e.g. Qonto, Stripe. Store the answer in `connectors.preferred`; `connectors.enabled` defaults to true (set false to disable probing entirely). Exports/paste remain the default input either way.

### 10. Write Config & Initialize Data Files
1. Resolve the write location for `<DATA_DIR>` per the Data directory section of the startup protocol and create it if needed.
2. Write `config.json` per `references/config-schema.md`.
3. Initialize empty data files if absent:
   - `invoices.json` → `{ "invoices": [], "updated_at": "<today>" }`
   - `forecast.json` → `{ "assumptions": {}, "updated_at": "<today>" }`

### 11. Confirm
Show a summary table (business, currency, jurisdictions, VAT, recurring revenue/cost counts, payment terms, buffer, cross-OS flags). State the absolute path actually used for `<DATA_DIR>`; if you fell back to location 3 (home), add: "If you're running in Cowork, connect a business folder and re-run setup — approve creating `os-data/` inside it, since location 2 is only selected once `./os-data/` exists; your data then lands in `./os-data/finance-os/`." When setup re-runs inside a connected folder, offer to create `./os-data/` before writing so location 2 is deliberately selected. Then point the user at:
- `/finance-os:cfo` — main entry point
- Direct specialists: `/finance-os:bookkeeper`, `/finance-os:invoicing`, `/finance-os:cashflow-forecaster`, `/finance-os:tax-planner`, `/finance-os:pricing-analyst`
- Re-run `/finance-os:setup` anytime to change settings.

## Notes
- Everything stays local under `<DATA_DIR>/` — no credentials are ever requested or stored (payment details on invoices are the user's own public remittance info).
- See `${CLAUDE_PLUGIN_ROOT}/SAFETY.md` for the standing disclaimer.

## Migrating your data (`/finance-os:setup migrate`)

When invoked with `migrate` — or whenever the user asks to move, consolidate, or centralise their data:

1. **Scan** all three resolution locations for finance-os data files (config.json plus any data files listed in the schema/GUIDE). Report what exists where: absolute path, files found, last-modified dates.
2. **Ask for the target**, recommending in this order:
   - `$OS_HUB_DATA_DIR/finance-os/` — best for "same data in every future session". Suggest pointing it at an `os-data` folder inside a synced location (e.g. `~/OneDrive/business/os-data`) and remind the user to persist the variable in `~/.claude/settings.json` under `"env"` so every future session sees it. For Cowork — where the env var is not available — have them connect the folder that CONTAINS `os-data/` as the working folder: location 2 (`./os-data/finance-os/`) then resolves to the very same files the env var points to elsewhere.
   - `./os-data/finance-os/` in the current working folder — right when this folder IS the business folder they connect in Cowork.
   - `~/.claude/plugins/data/finance-os/` — fine for single-machine, Claude Code-only use.
3. **Copy** (never move yet) every data file to the target, creating directories as needed. On a name collision, keep the newer file and say so. List exactly what will be copied before doing it.
4. **Verify** by reading `config.json` back from the target.
5. Only after verification, **offer** to rename the originals with a `.migrated` suffix so future sessions resolve unambiguously. Never delete without an explicit yes.
6. If the target is inside a git repository, remind the user to add `os-data/` to `.gitignore`.
7. **Post-migration review:** walk through the migrated `config.json` and confirm anything vague — empty fields, fields introduced by newer plugin versions (e.g. `connectors`), or values that look stale (old dates, placeholder text, competitors/rates that may have changed). One short confirm-or-update question per flagged item; write the refreshed config to the new location.
8. **Empty target, no source:** if the chosen target is empty and the scan found no finance-os data in any location, skip migration and run the normal setup wizard instead, writing its output to the target.
