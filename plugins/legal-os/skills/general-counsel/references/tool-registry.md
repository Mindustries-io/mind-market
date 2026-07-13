# Legal OS Tool Registry

Catalog of tools available to Legal OS, organized by function. For MCP servers, discover
the exact tool names at runtime (ToolSearch or the tool list); if a server is not
connected, use the listed fallback — never fail hard on a missing integration.

## Orchestration

| Tool | Purpose |
|---|---|
| `Agent` (subagent_type: `legal-os:<agent>`) | Delegate to specialist agents: `compliance-officer`, `dpo`, `contract-manager`, `legal-researcher`, `regulatory-intel`, `incident-response`, `vendor-risk` |

## Core tools (all agents)

| Tool | Purpose | Fallback |
|---|---|---|
| `Read` / `Write` / `Glob` / `Grep` | Config, data files, user documents | — |
| `WebSearch` | Regulatory news, guidance, enforcement, vendor research | Ask the user to paste sources |
| `WebFetch` | Fetch and analyze specific pages (EUR-Lex, EDPB, DPA sites) | `WebSearch` summaries |

## Optional MCP integrations

| Integration | Used by | Purpose | Fallback |
|---|---|---|---|
| Memory server (e.g. claude-mem) | All | Cross-session legal memory under the `los:` prefix | Proceed without memory |
| Web-scraping server (e.g. an Apify-style RAG web browser) | legal-researcher, regulatory-intel | Deep extraction from legal databases and DPA portals | `WebSearch` + `WebFetch` |
| Scheduled-tasks server | incident-response, GC | Deadline tracking (e.g. 72-hour notification) | Manual reminders noted in output |

## Built-in legal skills (optional, via Skill tool)

If Anthropic's `legal:*` marketplace skills are installed, agents may invoke them; if
not, each agent covers the task natively.

| Skill | Primary agent | Purpose |
|---|---|---|
| `legal:review-contract` | contract-manager | Clause-by-clause contract analysis |
| `legal:triage-nda` | contract-manager | GREEN/YELLOW/RED NDA classification |
| `legal:signature-request` | contract-manager | Pre-signature checklist |
| `legal:compliance-check` | compliance-officer | Compliance screening of actions/features |
| `legal:legal-risk-assessment` | General Counsel | Standalone risk scoring |
| `legal:vendor-check` | vendor-risk | Vendor agreement status |
| `legal:meeting-briefing` | General Counsel | Meeting prep with legal context |
| `legal:brief` | General Counsel | Legal briefings |
| `legal:legal-response` | legal-researcher | Templated responses (DSAR, litigation hold, vendor) |

## Data files

All under `<DATA_DIR>/` (created on demand; `<DATA_DIR>` = resolved per the Data
directory section of `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md`):

| File | Read/Write | Used by |
|---|---|---|
| `config.json` | Read (all), Write (setup) | All agents |
| `compliance-snapshot.json` | Read | compliance-officer, dpo — see `${CLAUDE_PLUGIN_ROOT}/references/compliance-data-sources.md` |
| `contracts/index.json` | Read/Write | contract-manager |
| `regulatory-calendar.json` | Read/Write | regulatory-intel, compliance-officer |
| `incident-log.json` | Read/Write | incident-response |
| `vendor-registry.json` | Read/Write | vendor-risk |
| `matters/{id}.json` | Read/Write | General Counsel, any specialist |

## Notes

1. Every agent follows `${CLAUDE_PLUGIN_ROOT}/references/startup-protocol.md` first.
2. Memory entries use the `los:` prefix.
3. Snapshot older than 7 days → warn the user (see compliance-data-sources.md).
4. Risk above the org's `ESCALATION` threshold → flag for human review.
5. Disclaimers: link to `${CLAUDE_PLUGIN_ROOT}/SAFETY.md` — do not restate per output.
