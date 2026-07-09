# PO-OS — Shared Startup Protocol

Follow these steps before starting any PO-OS task. Do not repeat them mid-task.

## 1. Load configuration

Read `~/.claude/plugins/data/po-os/config.json` and extract the `active_profile`.

**Graceful degradation — do NOT hard-stop if the config is missing or empty:**
- Offer a 3-question inline quick-setup: (1) product name and one-line pitch, (2) primary
  markets (country codes), (3) covered compliance frameworks (default: GDPR). Write the
  answers to the config file and continue.
- Or, if the user already provided enough context in their request, proceed with that,
  noting that personalization is reduced.
- Mention that `/po-os:setup` runs the full configuration wizard.

## 2. Memory

If a memory search tool (e.g. the claude-mem MCP server) is available, search for
`"po: {PRODUCT}"` plus task-relevant keywords, and store significant findings with the
`po:` prefix (e.g. `po: {PRODUCT} regulatory → backlog: {article} → {count} items`).
If no memory tool is available, proceed silently without it.

## 3. Standard variables

Extract from the active profile (use empty/sensible defaults for missing fields):

| Variable | Config field |
|---|---|
| `PRODUCT` | `product_name` |
| `DOMAIN` | `domain` |
| `STAGE` | `stage` |
| `REPO` | `repo` |
| `PERSONAS` | `personas` |
| `MARKETS` | `markets` (primary + secondary) |
| `FRAMEWORKS` | `covered_frameworks` + `local_frameworks` |
| `COMPETITORS` | `competitors` |
| `BACKLOG` | `backlog` (location, repo, labels) |
| `DOR` | `dor_criteria` |
| `VOC` | `voc_sources` |
| `WEIGHTS` | `prioritization_weights` |
| `CROSS_OS` | `cross_os` (legal_os, marketing_os flags) |

## 4. External tools and fallbacks

Never fail hard on a missing integration — always use the fallback.

| Integration | Use | Fallback if unavailable |
|---|---|---|
| `gh` CLI (GitHub Issues) | Creating/reading backlog items | Append to `~/.claude/plugins/data/po-os/backlog/index.json` and show the draft |
| Web-scraping MCP server (e.g. an Apify-style RAG web browser) | Review/forum extraction | `WebSearch` + `WebFetch` on public pages |
| Cloud drive / file MCP server | Reading VoC logs stored remotely | Ask the user to paste or export the log |
| Memory MCP (claude-mem) | Cross-session product context | Proceed without memory |
| Sibling plugins (`legal-os`, `marketing-os`) | Shared regulatory / market intel | Native research via `WebSearch`; note the limitation |

For MCP servers, discover the exact tool names at runtime (ToolSearch or the tool list);
never assume hardcoded tool IDs.
