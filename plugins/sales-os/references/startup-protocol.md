# Sales OS — Shared Startup Protocol

Every Sales OS agent and skill follows these steps before doing any task.

## Data directory

`<DATA_DIR>` (config.json and data files) resolves in this order:

1. If the `OS_HUB_DATA_DIR` environment variable is set → use `$OS_HUB_DATA_DIR/sales-os/`.
2. Else if `./os-data/` exists relative to the current working directory (Cowork: the user's connected folder) → use `./os-data/sales-os/` inside it, creating the subdirectory if needed.
3. Else → `~/.claude/plugins/data/sales-os/` (Claude Code default; unchanged for existing users).

- **READ** (config.json, the pipeline file, snapshots): first SELECT the active location — 1 if `OS_HUB_DATA_DIR` is set, else 2 if `./os-data/` exists in the working directory, else 3 — and read from it. If a deliberately selected location (1 or 2) contains no `config.json`, do NOT silently fall back — apply the "Empty but deliberately selected location" section below. If location 3 is selected and empty, offer the quick setup per the configuration-load step.
- **WRITE** (setup wizard writing config.json; data files like the pipeline file): write to the SELECTED location (same selection rule as READ), creating directories as needed. If it is not writable, try the remaining locations in priority order (1 → 2 → 3, skipping the one already tried) and tell the user where the file actually landed. If no location is writable (rare — e.g. a fully read-only sandbox), do not fail: keep the data in the session, show it to the user to save manually, and say that persistence was skipped.

## 1. Load Configuration

Read `<DATA_DIR>/config.json` (resolved per the Data directory section above) and extract the `active_profile`.

**Graceful degradation:** if the file is missing or empty, do NOT hard-stop. Offer the user a 3-question inline quick-setup:

1. "What do you sell, and to whom?" (offer + ICP)
2. "What are your typical rates or price range?" (pricing)
3. "Do you track deals anywhere today — CRM, spreadsheet, or nothing?" (pipeline/CRM situation)

Write the answers to the config file as a minimal profile, or proceed with whatever context the user already gave, noting reduced personalization. Mention that `/sales-os:setup` runs the full wizard.

## 2. Memory

If a memory search tool (e.g. the claude-mem MCP server) is available, search `"sal: {BUSINESS}"` for recent context — open deals, past win/loss reasons, outreach threads, prior proposals. If no memory tool is available, proceed silently without it. All Sales OS memory writes use the `sal:` prefix.

## 3. Standard Variables

Extract from the active profile and keep for the session:

| Variable | Source field | Meaning |
|---|---|---|
| `BUSINESS` | `business_name` | The user's business/brand name |
| `OFFER` | `offer` | Services/products and positioning |
| `RATES` | `pricing` | Currency, rates, minimum engagement |
| `ICP` | `icp` | Ideal customer profile + red flags |
| `STAGES` | `pipeline.stages` | Ordered pipeline stage names |
| `PIPELINE_FILE` | `pipeline.file_path` | Local pipeline file the user owns |
| `TONE` | `outreach.tone` | Outreach voice/tone settings |
| `TERMS` | `proposal.standard_terms` | Standard proposal/SOW terms |
| `CRM` | `crm` | CRM situation: none / csv / hubspot / pipedrive / attio |
| `CROSS_OS` | `cross_os` | Which sibling OS plugins are installed |

## 4. External Tools (all optional — never fail hard)

| Integration | Used for | Fallback if unavailable |
|---|---|---|
| CRM MCP server (HubSpot, Pipedrive, or Attio — discover exact tool names at runtime via ToolSearch or the tool list) | Reading deal/contact exports | Ask the user to paste a CSV export; never ask for credentials |
| WebSearch | Prospect and company research for personalization | Ask the user for what they know about the prospect; skip with a note |
| Memory (claude-mem MCP) | Cross-session deal context, win/loss log | Proceed without memory; rely on the pipeline file |
| Sibling OS plugins (`CROSS_OS` flags) | Brand voice (marketing-os), contract terms (legal-os), VoC feed (po-os) | Native research / standard template; see `${CLAUDE_PLUGIN_ROOT}/references/cross-os-integration.md` |

## Upgrades and missing fields

Configs written by older plugin versions stay valid: treat any missing field as its documented default — in particular, a missing `connectors` block means `{ "enabled": true, "preferred": [] }`. Never require the user to re-run setup after a plugin update; offer it only if they want to use new options.

### Multiple data locations

If more than one location in the resolution order contains `config.json`, use the highest-priority one but TELL the user which other locations also hold sales-os data (absolute paths + last-modified) and suggest `/sales-os:setup migrate` to consolidate — silent divergence between locations is worse than a one-line warning.

### Empty but deliberately selected location

An explicitly chosen location that is empty is a signal, not a fallthrough. If `OS_HUB_DATA_DIR` is set but `$OS_HUB_DATA_DIR/sales-os/` has no `config.json`, or `./os-data/` exists but `./os-data/sales-os/` is empty, do NOT silently fall back to a lower-priority location:

- If a lower-priority location holds sales-os data, say so and offer `/sales-os:setup migrate` to bring it into the selected location (reading the old data for this session is fine, but say that is what you are doing).
- If no location holds any data, offer setup — the 3-question quick version inline or the full `/sales-os:setup` wizard — writing the result to the selected location.
