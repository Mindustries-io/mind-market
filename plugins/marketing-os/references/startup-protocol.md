# Marketing OS — Shared Startup Protocol

Every Marketing OS agent and skill follows this protocol once at the start of a task. Do not restate it elsewhere.

## 1. Load configuration

Read `~/.claude/plugins/data/marketing-os/config.json` and select the `active_profile`.

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
