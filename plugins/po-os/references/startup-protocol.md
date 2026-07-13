# PO-OS — Shared Startup Protocol

Follow these steps before starting any PO-OS task. Do not repeat them mid-task.

## Data directory

`<DATA_DIR>` (the PO-OS data directory) resolves in this order:

1. If the `OS_HUB_DATA_DIR` environment variable is set → use `$OS_HUB_DATA_DIR/po-os/`.
2. Else if `./os-data/` exists relative to the current working directory (Cowork: the user's connected folder) → use `./os-data/po-os/` inside it, creating the subdirectory if needed.
3. Else → `~/.claude/plugins/data/po-os/` (Claude Code default; unchanged for existing users).

- **READ** (config.json, backlog index, discovery log): first SELECT the active location — 1 if `OS_HUB_DATA_DIR` is set, else 2 if `./os-data/` exists in the working directory, else 3 — and read from it. If a deliberately selected location (1 or 2) contains no `config.json`, do NOT silently fall back — apply the "Empty but deliberately selected location" section below. If location 3 is selected and empty, offer the quick setup per the configuration-load step.
- **WRITE** (setup wizard writing config.json; data files like backlog/index.json, discovery-log.json): write to the SELECTED location (same selection rule as READ), creating directories as needed. If it is not writable, try the remaining locations in priority order (1 → 2 → 3, skipping the one already tried) and tell the user where the file actually landed. If no location is writable (rare — e.g. a fully read-only sandbox), do not fail: keep the data in the session, show it to the user to save manually, and say that persistence was skipped.

## 1. Load configuration

Read `<DATA_DIR>/config.json` (resolved per the Data directory section above) and extract the `active_profile`.

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
| `gh` CLI (GitHub Issues) | Creating/reading backlog items | Append to `<DATA_DIR>/backlog/index.json` and show the draft |
| Web-scraping MCP server (e.g. an Apify-style RAG web browser) | Review/forum extraction | `WebSearch` + `WebFetch` on public pages |
| Cloud drive / file MCP server | Reading VoC logs stored remotely | Ask the user to paste or export the log |
| Memory MCP (claude-mem) | Cross-session product context | Proceed without memory |
| Sibling plugins (`legal-os`, `marketing-os`) | Shared regulatory / market intel | Native research via `WebSearch`; note the limitation |

For MCP servers, discover the exact tool names at runtime (ToolSearch or the tool list);
never assume hardcoded tool IDs.

## Upgrades and missing fields

Configs written by older plugin versions stay valid: treat any missing field as its documented default — in particular, a missing `connectors` block means `{ "enabled": true, "preferred": [] }`. Never require the user to re-run setup after a plugin update; offer it only if they want to use new options.

### Multiple data locations

If more than one location in the resolution order contains `config.json`, use the highest-priority one but TELL the user which other locations also hold po-os data (absolute paths + last-modified) and suggest `/po-os:setup migrate` to consolidate — silent divergence between locations is worse than a one-line warning.

### Empty but deliberately selected location

An explicitly chosen location that is empty is a signal, not a fallthrough. If `OS_HUB_DATA_DIR` is set but `$OS_HUB_DATA_DIR/po-os/` has no `config.json`, or `./os-data/` exists but `./os-data/po-os/` is empty, do NOT silently fall back to a lower-priority location:

- If a lower-priority location holds po-os data, say so and offer `/po-os:setup migrate` to bring it into the selected location (reading the old data for this session is fine, but say that is what you are doing).
- If no location holds any data, offer setup — the 3-question quick version inline or the full `/po-os:setup` wizard — writing the result to the selected location.
