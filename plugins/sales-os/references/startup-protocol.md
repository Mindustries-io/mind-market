# Sales OS — Shared Startup Protocol

Every Sales OS agent and skill follows these steps before doing any task.

## 1. Load Configuration

Read `~/.claude/plugins/data/sales-os/config.json` and extract the `active_profile`.

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
