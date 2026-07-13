# Mindustries OS Hub

A [Claude Code plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces) of **Agentic Operating Systems for solopreneurs**. Each OS is a plugin that gives you a virtual department: an orchestrator you talk to, and a team of specialist subagents tuned (model and effort) to the work they do.

## Install

```bash
claude plugin marketplace add Mindustries-io/mind-market
claude plugin install legal-os@mindustries-os-hub        # or any OS below
```

Then run the OS's setup wizard once (e.g. `/legal-os:setup`) to create your local profile.

## The Operating Systems

| OS | Orchestrator | What it covers |
|---|---|---|
| **marketing-os** | CMO | SEO, content, email, social, competitive intel, analytics, brand monitoring |
| **legal-os** | General Counsel | Compliance, GDPR, contracts, regulatory intel, incident response, vendor risk |
| **po-os** | Lead PO | Regulatory-driven features, jurisdictional localization, voice-of-customer |
| **security-os** | vCISO | AppSec review, threat modeling, hardening, vendor security, incident response |
| **finance-os** | CFO | Bookkeeping, invoicing & collections, cash-flow forecasting, tax prep, pricing |
| **sales-os** | Sales Director | Pipeline, 1-to-1 outreach, proposals & SOWs, CRM hygiene |
| **support-os** | Support Lead | Ticket triage, response drafting, knowledge base, support insights |

The OSs are independent but integrate when installed together: security incidents hand off to legal-os for breach notification, support and sales signals feed po-os discovery, sales reuses marketing-os brand voice, tax planning reuses legal-os jurisdictions. Every integration degrades gracefully if the sibling OS isn't installed.

## Architecture

Each plugin follows the same shape:

- **Orchestrator skill** — the entry point (`/marketing-os:cmo`, `/legal-os:general-counsel`, ...). Routes single-task requests straight to one specialist (fast mode) and only runs multi-agent pipelines for genuinely multi-domain work.
- **Specialist agents** (`agents/*.md`) — each declares its own `model` and `effort` in frontmatter: `haiku`/low for mechanical work (lookups, triage, categorization), `sonnet`/medium for playbook-driven drafting, and `inherit`/high for judgment-heavy work (contracts, DPIAs, threat models, tax planning).
- **Thin trigger skills** — every specialist also has a slash-command skill that delegates to its agent, so `/legal-os:dpo`-style direct invocation still works.
- **Local config** — your profile lives in a per-OS data directory (see "Where your data lives" below), created by each OS's setup wizard. No credentials are ever stored; integrations work from exports and pasted data — or live from connected MCP tools when available.

## Where your data lives

Every OS resolves its data directory (config, ledgers, logs) in this order:

1. `$OS_HUB_DATA_DIR/<os>/` — if the `OS_HUB_DATA_DIR` environment variable is set.
2. `./os-data/<os>/` — relative to the working directory. This is the Cowork path: connect a business folder to your session and data lands inside it.
3. `~/.claude/plugins/data/<os>/` — the Claude Code default (unchanged for existing users).

The OS first *selects* the active location — 1 if `OS_HUB_DATA_DIR` is set, else 2 if `./os-data/` exists in the working folder, else 3 — and reads and writes there, creating directories as needed. A deliberately selected location (1 or 2) that turns out to be empty is never silently skipped in favor of old data elsewhere: the OS offers to migrate your existing data in, or runs setup fresh (see below). **If your working folder is a git repository, add `os-data/` to its `.gitignore`** — it will contain business data (finances, clients, tickets).

**Centralising data across sessions:** point `OS_HUB_DATA_DIR` at an `os-data` folder inside a synced location (e.g. `<OneDrive>/business/os-data`) and persist it in the Claude Code settings file at `~/.claude/settings.json` under `"env"` so every future session resolves there. In Cowork — where the env var isn't available — connect the **parent** folder that contains `os-data/` (e.g. `<OneDrive>/business`), not the `os-data` folder itself: location 2 checks for `./os-data/` inside the working folder, so connecting the parent makes `./os-data/<os>/` resolve to the very same files. Already have data scattered across locations? Run `/<os>:setup migrate` — it scans all three locations, copies everything to the target you choose, verifies, walks you through confirming any stale or incomplete fields, and only then offers to retire the originals. If an OS ever finds data in more than one location, it warns you instead of silently picking one. And if you point `OS_HUB_DATA_DIR` (or a connected folder) at an *empty* location, the OS doesn't silently fall back to old data — it offers to migrate your existing data in, or runs setup fresh if there's nothing to migrate.

## Live data when connected, exports otherwise

The OSs are exports-first: everything works from pasted text and CSV exports. But when a relevant MCP connector is available in your session — a bank or payments tool for finance-os (e.g. Qonto, Stripe), a CRM for sales-os, a helpdesk for support-os, an SEO suite for marketing-os — agents discover it at runtime and offer to read live data instead. Discovery is by product name, read-only, never stores credentials, and can be disabled per OS with `connectors.enabled: false` in the config (or pinned with `connectors.preferred`).

## Models, cost, and third-party backends

Model aliases (`haiku`, `sonnet`) in agent frontmatter resolve on the official Anthropic backends. If you run Claude Code through a proxy or alternative backend (LiteLLM, Ollama, Bedrock, Vertex):

- Map the aliases to your models via environment variables (`ANTHROPIC_DEFAULT_HAIKU_MODEL`, `ANTHROPIC_DEFAULT_SONNET_MODEL`, ...), **or** edit the agent frontmatter to `model: inherit`, which always uses your session model on any backend.
- The `effort` field is Anthropic-specific and is silently ignored on other backends.
- Judgment-critical agents already use `model: inherit`, so they follow your session model everywhere.

### Using the plugins on a proxied backend

The specialist agents request models by alias: `haiku` (mechanical work), `sonnet` (drafting), or `inherit` (judgment-heavy work — always follows your session model and needs no configuration). On a non-Anthropic backend an unmapped alias fails with a hard error, so before using the plugins, map both aliases to models your backend actually serves:

```bash
# 1. Point Claude Code at your proxy (LiteLLM, Ollama, any Anthropic-compatible endpoint)
export ANTHROPIC_BASE_URL="http://localhost:4000"
export ANTHROPIC_AUTH_TOKEN="<whatever your proxy expects>"

# 2. Map the two aliases the agents use to models your backend serves
export ANTHROPIC_DEFAULT_HAIKU_MODEL="<your fast/cheap model>"     # e.g. a small local model
export ANTHROPIC_DEFAULT_SONNET_MODEL="<your mid-tier model>"

# 3. (Bedrock/Vertex) use provider-native IDs — ARNs / deployment names — not Anthropic model IDs
```

On PowerShell, use `$env:ANTHROPIC_DEFAULT_HAIKU_MODEL = "..."` instead of `export`, or persist the values in your Claude Code `settings.json` under `env`.

Notes:

- **`effort` is ignored on non-Anthropic backends** unless your provider supports it and you declare the capability (e.g. `ANTHROPIC_DEFAULT_SONNET_MODEL_SUPPORTED_CAPABILITIES='effort'`). Ignoring it is harmless — agents just run at the backend's default reasoning behavior.
- **No mapping? Use `inherit`.** If you'd rather not maintain alias mappings, change `model: haiku` / `model: sonnet` to `model: inherit` in the plugin's `agents/*.md` frontmatter. You lose the cost tiering (everything runs on your session model) but nothing breaks. Note that plugin updates overwrite local edits, so the env-var route is the durable one.
- **Quality caveat:** the tiering assumes Claude-class models. Small local models may struggle with the judgment-heavy agents (contract redlining, DPIAs, threat modeling); point at least the session model (used by all `inherit` agents) to the strongest model you have.

See [model configuration](https://code.claude.com/docs/en/model-config) in the Claude Code docs for the full alias/override reference.

## Development

```bash
claude plugin validate . --strict                 # marketplace manifest
claude plugin validate plugins/<os> --strict      # individual plugin
```

CI runs both on every push (`.github/workflows/validate.yml`).

## License

MIT © Mindustries
