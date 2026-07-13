# Marketing OS — Shared Startup Protocol

Every Marketing OS agent and skill follows this protocol once at the start of a task. Do not restate it elsewhere.

## Data directory

`<DATA_DIR>` (config.json and data files) resolves in this order:

1. If the `OS_HUB_DATA_DIR` environment variable is set → use `$OS_HUB_DATA_DIR/marketing-os/`.
2. Else if `./os-data/` exists relative to the current working directory (Cowork: the user's connected folder) → use `./os-data/marketing-os/` inside it, creating the subdirectory if needed.
3. Else → `~/.claude/plugins/data/marketing-os/` (Claude Code default; unchanged for existing users).

- **READ** (config.json, snapshots): first SELECT the active location — 1 if `OS_HUB_DATA_DIR` is set, else 2 if `./os-data/` exists in the working directory, else 3 — and read from it. If a deliberately selected location (1 or 2) contains no `config.json`, do NOT silently fall back — apply the "Empty but deliberately selected location" section below. If location 3 is selected and empty, offer the quick setup per the configuration-load step.
- **WRITE** (setup wizard writing config.json; data files): use the first WRITABLE location in the same order, creating directories as needed. In practice: env var if set, else `./os-data/marketing-os/` if the CWD is writable and the user is in a sandbox/Cowork-style session or `./os-data/` already exists, else the home path. When both 2 and 3 are plausible on WRITE and neither exists yet, prefer 3 (home) in Claude Code and 2 in sandboxed sessions where 3 is unreachable — the practical test is: try in order, first successful write wins.

## 1. Load configuration

Read `<DATA_DIR>/config.json` (resolved per the Data directory section above) and select the `active_profile`.

**Graceful degradation:** if the file is missing or empty, do NOT hard-stop. Offer the user a 3-question inline quick-setup (brand name, domain, main competitor), write those answers to the config, and continue. Alternatively proceed with whatever context the user already gave, noting reduced personalization. Mention `/marketing-os:setup` for the full wizard.

## 2. Memory

Namespace prefix: `mos:`

If a memory search tool (e.g. the claude-mem MCP server) is available, search `"mos: <BRAND>"` for recent context (metrics, campaigns, prior findings) and store important new findings under the same prefix. If no memory tool is available, proceed silently without it.

## 3. Standard variables

Extract from the active profile:

| Variable | Config field |
|---|---|
| `BRAND` | `brand_name` |
| `DOMAIN` | `domain` |
| `PROTOCOL` | `protocol` (default `https`) |
| `COMPETITORS` | `competitors` (array of `{name, domain}`) |
| `COUNTRIES` | `target_countries` (default `["us"]`) |
| `LANGUAGE` | `primary_language` (default `en`) |
| `CHANNELS` | `channels` |
| `VOICE` | `brand_voice` |
| `REPORTING` | `reporting` |
| `INTEGRATIONS` | `integrations` (booleans: `ahrefs`, `claude_mem`, `nano_banana`) |

## 4. External tools & fallbacks

All integrations are OPTIONAL. Check the `integrations` config flags AND actual tool availability (discover exact tool names at runtime via ToolSearch or the tool list — never assume hardcoded IDs). Never fail hard on a missing integration.

| Integration | Provides | Fallback when unavailable |
|---|---|---|
| The Ahrefs MCP server | SEO metrics, keywords, backlinks, SERP, Brand Radar, web analytics, GSC | WebSearch + data the user pastes (CSV/exports). Prefix the output with a **"Degraded insights"** note stating live SEO data was unavailable. |
| claude-mem (memory MCP) | Cross-session memory | Skip memory steps silently. |
| nano-banana (image MCP) | AI-generated marketing images | Provide a detailed image prompt/description the user can use elsewhere; note the image was not generated. |

## Upgrades and missing fields

Configs written by older plugin versions stay valid: treat any missing field as its documented default — in particular, a missing `connectors` block means `{ "enabled": true, "preferred": [] }`. Never require the user to re-run setup after a plugin update; offer it only if they want to use new options.

### Multiple data locations

If more than one location in the resolution order contains `config.json`, use the highest-priority one but TELL the user which other locations also hold marketing-os data (absolute paths + last-modified) and suggest `/marketing-os:setup migrate` to consolidate — silent divergence between locations is worse than a one-line warning.

### Empty but deliberately selected location

An explicitly chosen location that is empty is a signal, not a fallthrough. If `OS_HUB_DATA_DIR` is set but `$OS_HUB_DATA_DIR/marketing-os/` has no `config.json`, or `./os-data/` exists but `./os-data/marketing-os/` is empty, do NOT silently fall back to a lower-priority location:

- If a lower-priority location holds marketing-os data, say so and offer `/marketing-os:setup migrate` to bring it into the selected location (reading the old data for this session is fine, but say that is what you are doing).
- If no location holds any data, offer setup — the 3-question quick version inline or the full `/marketing-os:setup` wizard — writing the result to the selected location.
