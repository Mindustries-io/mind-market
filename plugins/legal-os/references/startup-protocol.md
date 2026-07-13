# Legal OS — Shared Startup Protocol

Follow these steps before starting any Legal OS task. Do not repeat them mid-task.

## Data directory

Wherever Legal OS files say `<DATA_DIR>`, resolve it in this order:

1. If the `OS_HUB_DATA_DIR` environment variable is set → use `$OS_HUB_DATA_DIR/legal-os/`.
2. Else if `./os-data/` exists relative to the current working directory (Cowork: the user's connected folder) → use `./os-data/legal-os/` inside it, creating the subdirectory if needed.
3. Else → `~/.claude/plugins/data/legal-os/` (Claude Code default; unchanged for
   existing users).

- **READ** (config.json, registries, snapshots): first SELECT the active location — 1 if `OS_HUB_DATA_DIR` is set, else 2 if `./os-data/` exists in the working directory, else 3 — and read from it. If a deliberately selected location (1 or 2) contains no `config.json`, do NOT silently fall back — apply the "Empty but deliberately selected location" section below. If location 3 is selected and empty, offer the quick setup per the configuration-load step.
- **WRITE** (setup wizard writing config.json; data files): write to the SELECTED location (same selection rule as READ), creating directories as needed. If it is not writable, try the remaining locations in priority order (1 → 2 → 3, skipping the one already tried) and tell the user where the file actually landed. If no location is writable (rare — e.g. a fully read-only sandbox), do not fail: keep the data in the session, show it to the user to save manually, and say that persistence was skipped.

## 1. Load configuration

Read `<DATA_DIR>/config.json` and extract the `active_profile`.

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
| Compliance tracker export (`<DATA_DIR>/compliance-snapshot.json`) | Obligation status, scores, evidence gaps | **Lite mode:** ask the user to paste status data, or estimate from conversation context and note the limitation |
| Web-scraping MCP server (e.g. an Apify-style RAG web browser) | Deep extraction from legal sources | `WebSearch` + `WebFetch` |
| Memory MCP (claude-mem) | Cross-session legal context | Proceed without memory |
| Data files (`contracts/index.json`, `vendor-registry.json`, `incident-log.json`, `regulatory-calendar.json`) | Registries and logs | Create the file with an empty structure, or work from user-provided data |

For MCP servers, discover the exact tool names at runtime (ToolSearch or the tool list);
never assume hardcoded tool IDs.

## Upgrades and missing fields

Configs written by older plugin versions stay valid: treat any missing field as its documented default — in particular, a missing `connectors` block means `{ "enabled": true, "preferred": [] }`. Never require the user to re-run setup after a plugin update; offer it only if they want to use new options.

### Multiple data locations

If more than one location in the resolution order contains `config.json`, use the highest-priority one but TELL the user which other locations also hold legal-os data (absolute paths + last-modified) and suggest `/legal-os:setup migrate` to consolidate — silent divergence between locations is worse than a one-line warning.

### Empty but deliberately selected location

An explicitly chosen location that is empty is a signal, not a fallthrough. If `OS_HUB_DATA_DIR` is set but `$OS_HUB_DATA_DIR/legal-os/` has no `config.json`, or `./os-data/` exists but `./os-data/legal-os/` is empty, do NOT silently fall back to a lower-priority location:

- If a lower-priority location holds legal-os data, say so and offer `/legal-os:setup migrate` to bring it into the selected location (reading the old data for this session is fine, but say that is what you are doing).
- If no location holds any data, offer setup — the 3-question quick version inline or the full `/legal-os:setup` wizard — writing the result to the selected location.
