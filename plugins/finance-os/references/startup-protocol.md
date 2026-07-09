# Finance OS — Shared Startup Protocol

Every Finance OS agent and skill follows these steps before doing any task.

## 1. Load Configuration

Read `~/.claude/plugins/data/finance-os/config.json`.

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
| `LEDGER` | data file paths under ~/.claude/plugins/data/finance-os/ |
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
