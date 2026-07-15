---
name: setup
description: Delivery OS configuration and onboarding. Use when the user asks to 'set up delivery os', 'configure delivery os', 'delivery os setup', 'configure my delivery org', 'change delivery settings', 'set KPI targets', 'define account tiers', or mentions setting up delivery-os for the first time.
---

# Delivery OS Setup

You configure the organization profile that both Delivery OS skills rely on. Config lives at
`<DATA_DIR>/config.json` (`<DATA_DIR>` = resolved per the Data Directory section of
`${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`). Read it first if it exists and
preserve anything already set; create the directory and file if not.

If the user asked for one specific change ("set CSAT target to 4.2", "add a tier"), skip the
wizard and make the targeted edit, then show the updated section.

## Wizard Steps

1. **Organization** — org name, the unit this instance covers (default "Delivery"), reporting currency.
2. **Account tiers** — how Tier 1 / Tier 2 / Tier 3 are defined in this organization (revenue thresholds, strategic flags). These drive which accounts get full ADPs.
3. **KPI targets** — confirm or override the defaults: CSAT ≥ 4.0/5.0; delivery-margin deviation (actual vs. plan) within ±5%; PoC-to-Production conversion > 30%; expansion revenue > 70% of total; 2+ qualified expansion opportunities per account per quarter. Record any organization-specific metrics.
4. **Review cadence** — scorecard refresh (default weekly), ADP refresh (default monthly), exec report (default monthly).
5. **Connectors** — "Do you use any connected tools I should pull live data from (project tracker, CRM, spreadsheet)?" Store under `connectors` (`enabled` default true; named tools into `connectors.preferred`). Exports and pasted data remain the fallback; no credentials are ever stored.
6. **Write config** — resolve `<DATA_DIR>` per the startup protocol, create the directory, write `config.json`, and initialize `scorecard.json` (`{ "history": [] }`) and the `adp/` directory if absent.
7. **Confirm** — show a summary table and state the absolute path where the configuration was written. If it fell back to the home-directory location, note that Cowork users should connect a working folder containing `os-data/` and re-run setup. Remind: data files contain client-confidential information — keep `<DATA_DIR>` in the organization's own folder or drive, and gitignore `os-data/` in git repositories.

Then point the user at `/delivery-os:delivery-scorecard` and `/delivery-os:adp-generator`.

## Migrating (`/delivery-os:setup migrate`)

On `migrate`, scan all three resolution locations for delivery-os data, report what exists
where, copy (never move) to the user's chosen target, verify by reading config.json back, and
only then offer to rename originals with a `.migrated` suffix. Never delete without an explicit yes.
