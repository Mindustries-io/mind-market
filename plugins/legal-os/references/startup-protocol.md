# Legal OS — Shared Startup Protocol

Follow these steps before starting any Legal OS task. Do not repeat them mid-task.

## 1. Load configuration

Read `~/.claude/plugins/data/legal-os/config.json` and extract the `active_profile`.

**Graceful degradation — do NOT hard-stop if the config is missing or empty:**
- Offer a 3-question inline quick-setup: (1) organization name and country, (2) active
  compliance frameworks (default: GDPR), (3) operating jurisdictions. Write the answers
  to the config file and continue.
- Or, if the user already provided enough context in their request, proceed with that,
  noting that personalization is reduced.
- Mention that `/legal-os:setup` runs the full configuration wizard.

## 2. Memory

If a memory search tool (e.g. the claude-mem MCP server) is available, search for
`"los: {ORG}"` plus task-relevant keywords, and store significant findings with the
`los:` prefix (e.g. `los: {ORG} DPIA completed for {system}`). If no memory tool is
available, proceed silently without it.

## 3. Standard variables

Extract from the active profile (use empty/sensible defaults for missing fields):

| Variable | Config field |
|---|---|
| `ORG` | `org_name` |
| `COUNTRY` | `country` |
| `INDUSTRY` | `industry` |
| `JURISDICTIONS` | `jurisdictions` |
| `FRAMEWORKS` | `active_frameworks` |
| `DPO_DETAILS` | `dpo` |
| `AUTHORITY` | `supervisory_authority` |
| `CONTRACT_DEFAULTS` | `contract_defaults` |
| `RISK_LEVEL` | `risk_appetite.level` |
| `ESCALATION` | `risk_appetite.escalation_threshold` |

## 4. External tools and fallbacks

Never fail hard on a missing integration — always use the fallback.

| Integration | Use | Fallback if unavailable |
|---|---|---|
| Compliance tracker export (`~/.claude/plugins/data/legal-os/compliance-snapshot.json`) | Obligation status, scores, evidence gaps | **Lite mode:** ask the user to paste status data, or estimate from conversation context and note the limitation |
| Web-scraping MCP server (e.g. an Apify-style RAG web browser) | Deep extraction from legal sources | `WebSearch` + `WebFetch` |
| Memory MCP (claude-mem) | Cross-session legal context | Proceed without memory |
| Data files (`contracts/index.json`, `vendor-registry.json`, `incident-log.json`, `regulatory-calendar.json`) | Registries and logs | Create the file with an empty structure, or work from user-provided data |

For MCP servers, discover the exact tool names at runtime (ToolSearch or the tool list);
never assume hardcoded tool IDs.
