---
name: setup
description: "Finance OS configuration and onboarding. Use when the user asks to 'set up finance', 'configure finance os', 'finance os setup', 'fin setup', 'set up my business finances', 'configure cfo', 'change finance settings', 'update invoice template', 'add tax jurisdiction', or mentions setting up finance-os for the first time."
---

# Finance OS Setup Wizard

You configure the business profile that every Finance OS agent relies on. Config lives at `~/.claude/plugins/data/finance-os/config.json` — read it first if it exists and preserve anything already set; create the directory and file if not.

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

### 9. Write Config & Initialize Data Files
1. Create `~/.claude/plugins/data/finance-os/` if needed.
2. Write `config.json` per `references/config-schema.md`.
3. Initialize empty data files if absent:
   - `invoices.json` → `{ "invoices": [], "updated_at": "<today>" }`
   - `forecast.json` → `{ "assumptions": {}, "updated_at": "<today>" }`

### 10. Confirm
Show a summary table (business, currency, jurisdictions, VAT, recurring revenue/cost counts, payment terms, buffer, cross-OS flags). Then point the user at:
- `/finance-os:cfo` — main entry point
- Direct specialists: `/finance-os:bookkeeper`, `/finance-os:invoicing`, `/finance-os:cashflow-forecaster`, `/finance-os:tax-planner`, `/finance-os:pricing-analyst`
- Re-run `/finance-os:setup` anytime to change settings.

## Notes
- Everything stays local under `~/.claude/plugins/data/finance-os/` — no credentials are ever requested or stored (payment details on invoices are the user's own public remittance info).
- See `${CLAUDE_PLUGIN_ROOT}/SAFETY.md` for the standing disclaimer.
