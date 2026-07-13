# Finance OS — Shared Startup Protocol

Every Finance OS agent and skill follows these steps before doing any task.

## Data Directory

`<DATA_DIR>` — the Finance OS data directory — resolves in this order:

1. If the `OS_HUB_DATA_DIR` environment variable is set → use `$OS_HUB_DATA_DIR/finance-os/`.
2. Else if `./os-data/finance-os/` exists relative to the current working directory (Cowork: the user's connected folder) → use it.
3. Else → `~/.claude/plugins/data/finance-os/` (Claude Code default; unchanged for existing users).

- **READ** (config.json, ledgers, snapshots): use the first location in that order where the file exists (for location 1, "env var set" is enough to select it).
- **WRITE** (setup wizard writing config.json; data files like invoices.json, forecast.json): use the first WRITABLE location in the same order, creating directories as needed. In practice: env var if set, else `./os-data/finance-os/` if the CWD is writable and the user is in a sandbox/Cowork-style session or `./os-data/` already exists, else the home path. When both 2 and 3 are plausible on WRITE and neither exists yet, prefer 3 (home) in Claude Code and 2 in sandboxed sessions where 3 is unreachable — the practical test is: try in order, first successful write wins.

## 1. Load Configuration

Read `<DATA_DIR>/config.json`.

**Graceful degradation:** if the file is missing or empty, do NOT hard-stop. Instead:
- Offer a 3-question inline quick-setup: (1) business name + entity type, (2) base currency + tax jurisdiction, (3) VAT/GST registered? Write the answers to the config file, then continue.
- Or, if the user already gave enough context in their message, proceed with that and note that personalization is reduced.
- Mention that `/finance-os:setup` runs the full wizard (invoice template, revenue model, jurisdictions, cross-OS flags).

## 2. Memory

If a memory search tool (e.g. the claude-mem MCP server) is available, search `"fin: {BUSINESS}"` for recent context (past categorization decisions, outstanding invoices, forecast assumptions, tax deadlines already tracked). If no memory tool is available, proceed silently without it. All Finance OS memory writes use the `fin:` prefix.

## 3. Standard Variables

Extract from config and keep for the session:

| Variable | Config source |
|---|---|
| `BUSINESS` | business.name |
| `ENTITY` | business.entity_type |
| `CURRENCY` | business.currency |
| `JURISDICTIONS` | tax.jurisdictions (primary + others) |
| `VAT` | tax.vat (registered flag, rate, scheme, filing frequency) |
| `REVENUE_MODEL` | revenue.model + revenue.recurring items |
| `INVOICE` | invoicing (template fields, payment_terms_days, numbering) |
| `LEDGER` | data file paths under `<DATA_DIR>/` |
| `CROSS_OS` | cross_os flags (legal_os, marketing_os, po_os) |

## 4. External Tools & Fallbacks

Finance OS never connects to bank, accounting, or payment-provider credentials. Inputs are always user-provided.

| Need | Preferred | Fallback |
|---|---|---|
| Transactions | User pastes bank/Stripe CSV export or raw text | Ask for a paste; never ask for logins or API keys |
| Market/benchmark data | WebSearch | Skip with a note; use user-supplied figures |
| Jurisdiction conventions | legal-os config (if `cross_os.legal_os` is true) | Native research via WebSearch, or ask the user |
| Ledger state | Local JSON/markdown files under the data dir | Ask the user to paste current state |

Never fail hard on a missing integration — degrade, note it, continue.

## Upgrades and missing fields

Configs written by older plugin versions stay valid: treat any missing field as its documented default — in particular, a missing `connectors` block means `{ "enabled": true, "preferred": [] }`. Never require the user to re-run setup after a plugin update; offer it only if they want to use new options.

### Multiple data locations

If more than one location in the resolution order contains `config.json`, use the highest-priority one but TELL the user which other locations also hold finance-os data (absolute paths + last-modified) and suggest `/finance-os:setup migrate` to consolidate — silent divergence between locations is worse than a one-line warning.

### Empty but deliberately selected location

An explicitly chosen location that is empty is a signal, not a fallthrough. If `OS_HUB_DATA_DIR` is set but `$OS_HUB_DATA_DIR/finance-os/` has no `config.json`, or `./os-data/` exists but `./os-data/finance-os/` is empty, do NOT silently fall back to a lower-priority location:

- If a lower-priority location holds finance-os data, say so and offer `/finance-os:setup migrate` to bring it into the selected location (reading the old data for this session is fine, but say that is what you are doing).
- If no location holds any data, offer setup — the 3-question quick version inline or the full `/finance-os:setup` wizard — writing the result to the selected location.
