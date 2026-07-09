# Finance OS — Virtual CFO for a One-Person Business

Finance OS gives a solopreneur the finance function they don't have headcount for: bookkeeping tidy-ups, invoicing and payment chasing, a 13-week cash-flow forecast, a tax-deadline calendar, and pricing analysis — all coordinated by a virtual CFO orchestrator.

It works entirely from data you paste in and config you control. It never connects to your bank, Stripe, or accounting software, and never asks for credentials.

## Quick Start

1. Install the plugin from the Mindustries marketplace.
2. Run `/finance-os:setup` — the wizard captures your business entity, currency, jurisdictions, VAT status, revenue model, recurring costs, and invoice template fields (~5 minutes).
3. Talk to the CFO: `/finance-os:cfo` — or just ask naturally ("what's my runway?", "invoice Acme Studio for the Aurora onboarding", "here's my June bank export, categorize it").

No setup yet? Agents still work — they'll run a 3-question quick-setup inline or proceed with whatever context you give them.

## Architecture

```
                    /finance-os:cfo  (orchestrator — routing, fast mode, synthesis)
                          │
    ┌──────────┬──────────┼───────────────┬──────────────┐
bookkeeper  invoicing  cashflow-       tax-planner   pricing-analyst
                       forecaster
```

- **Fast mode (default):** a request that maps to one specialist goes to that one agent, and its answer comes straight back. No ceremony.
- **Pipelines** for chained questions ("should I raise prices?" → pricing impact → forecast scenario).
- **Collaborative fan-out** only for genuinely multi-domain requests (monthly finance review).
- Each specialist also has a direct trigger skill (`/finance-os:bookkeeper`, etc.), so you can skip the orchestrator.

## The Specialists

| Agent | What it does |
|---|---|
| `bookkeeper` | Categorizes pasted bank/Stripe/CSV transactions, monthly close, duplicate & anomaly flags |
| `invoicing` | Drafts invoices from your config, 3-step late-payment chase sequence, outstanding-invoice ledger |
| `cashflow-forecaster` | 13-week rolling forecast, runway, low-water mark, what-if scenarios (lose top client, hire, price change) |
| `tax-planner` | Tax-deadline calendar per jurisdiction, prepayment estimates, tax-reserve math, accountant prep packs |
| `pricing-analyst` | Value-based pricing, competitor benchmarks (web research), price-change impact estimates, discount guardrails |

## Models & Cost

Each agent runs on the cheapest model tier that does its job well:

| Agent | Model | Effort | Why |
|---|---|---|---|
| `bookkeeper` | haiku | low | Mechanical categorization and formatting — high volume, low judgment |
| `invoicing` | haiku | low | Template-driven drafting and ledger upkeep |
| `cashflow-forecaster` | sonnet | medium | Playbook-driven modeling and scenario synthesis |
| `pricing-analyst` | sonnet | medium | Research + analysis against defined frameworks |
| `tax-planner` | inherit | high | Judgment-heavy, error-costly jurisdiction work — uses your session model |

**Portability note:** Model aliases resolve on official Anthropic backends. If you run Claude Code against a proxy (LiteLLM, Ollama, Bedrock/Vertex), map the aliases via `ANTHROPIC_DEFAULT_HAIKU_MODEL` / `ANTHROPIC_DEFAULT_SONNET_MODEL` (etc.) or edit the agent frontmatter to `model: inherit`. The `effort` field is Anthropic-specific and is ignored elsewhere. Step-by-step proxy setup: see "Using the plugins on a proxied backend" in the [marketplace README](https://github.com/Mindustries-io/mind-market#using-the-plugins-on-a-proxied-backend).

## Data & Integrations

Everything lives locally under `~/.claude/plugins/data/finance-os/`:

| File | Contents |
|---|---|
| `config.json` | Business profile, tax settings, revenue/costs, invoice template fields |
| `invoices.json` | Outstanding-invoice ledger (you confirm every change) |
| `forecast.json` | Cash-flow model assumptions between runs |

Integrations and fallbacks — nothing fails hard:

| Need | How | Fallback |
|---|---|---|
| Transactions | You paste bank/Stripe CSV exports | Agent tells you exactly what to export |
| Market & benchmark data | WebSearch | Your own figures, with a note |
| Memory across sessions | claude-mem MCP server if installed (`fin:` prefix) | Proceeds silently without |
| Jurisdiction conventions | legal-os config if installed (`cross_os.legal_os`) | Native research via WebSearch |

**Never used:** bank logins, Stripe API keys, accounting-software credentials. If an agent ever seems to ask for one, that's a bug — it shouldn't.

## Example Sessions

- *"Here's my June bank CSV"* → bookkeeper categorizes, totals per category, flags a duplicate SaaS charge and a subscription price increase.
- *"Acme Studio hasn't paid invoice 2026-014"* → invoicing checks the ledger, sees step 1 was sent, drafts the firm step-2 reminder.
- *"Can I afford a contractor at 2,000/month?"* → cashflow-forecaster runs the hire scenario: runway drops from 26 to 19 weeks, low-water mark stays above buffer — conditional yes.
- *"Should I raise Aurora prices 15%?"* → pipeline: pricing-analyst benchmarks and estimates churn impact, cashflow-forecaster models it; CFO returns one report.
- *"What does my accountant need for Q2?"* → tax-planner builds the prep pack: documents, categorized totals, open questions to ask.

## Memory

With a memory tool installed, Finance OS remembers under the `fin:` prefix: categorization rules you confirmed, chase steps sent, forecast findings, deadlines, discounts granted. Without one, each session works from config + your pastes.

## Boundaries

Finance OS prepares, estimates, and drafts — it does not file taxes, send emails, move money, or make decisions. Figures are only as good as the data you provide. Read `SAFETY.md` for the full disclaimer, and take anything tax-shaped to a qualified professional.
