# Support-OS Startup Protocol (shared)

Every Support-OS agent and skill follows these steps before doing any task.

## Data directory

`<DATA_DIR>` — where config.json and data files live — is resolved in this order:

1. If the `OS_HUB_DATA_DIR` environment variable is set → use `$OS_HUB_DATA_DIR/support-os/`.
2. Else if `./os-data/` exists relative to the current working directory (Cowork: the user's connected folder) → use `./os-data/support-os/` inside it, creating the subdirectory if needed.
3. Else → `~/.claude/plugins/data/support-os/` (Claude Code default; unchanged for existing users).

- **READ** (config.json, snapshots): first SELECT the active location — 1 if `OS_HUB_DATA_DIR` is set, else 2 if `./os-data/` exists in the working directory, else 3 — and read from it. If a deliberately selected location (1 or 2) contains no `config.json`, do NOT silently fall back — apply the "Empty but deliberately selected location" section below. If location 3 is selected and empty, offer the quick setup per the configuration-load step.
- **WRITE** (setup wizard writing config.json; data files): write to the SELECTED location (same selection rule as READ), creating directories as needed. If it is not writable, try the remaining locations in priority order (1 → 2 → 3, skipping the one already tried) and tell the user where the file actually landed. If no location is writable (rare — e.g. a fully read-only sandbox), do not fail: keep the data in the session, show it to the user to save manually, and say that persistence was skipped.

## 1. Load Configuration

Read `<DATA_DIR>/config.json` (resolved per the Data directory section above) and extract the `active_profile`.

**Graceful degradation:** if the file is missing or empty, do NOT hard-stop. Instead:
- Offer a 3-question inline quick-setup: (1) product name, (2) support channels (email / chat / social), (3) preferred tone in one sentence. Write the answers to the config file and continue.
- Or, if the user already gave enough context in the request, proceed with that and note that personalization is reduced.
- Mention that `/support-os:setup` runs the full wizard (helpdesk tool, SLA, refund policy, cross-OS flags).

## 2. Standard Variables

From the active profile, store:

| Variable | Config field |
|---|---|
| `PRODUCT` | `product_name` (+ `pitch`, `domain`) |
| `CHANNELS` | `channels` |
| `HELPDESK` | `helpdesk` (tool name + export format — reference only, never credentials) |
| `VOICE` | `voice` (tone, signoff, `use_marketing_os_voice` flag) |
| `SLA` | `sla` (first-response / resolution targets) |
| `REFUND_POLICY` | `refund_policy` (summary, window, escalation threshold) |
| `KB` | `kb` (help-center location, inventory path) |
| `ESCALATION` | `escalation.user_decides` (decisions reserved for the user) |
| `CROSS_OS` | `cross_os` (`po_os`, `marketing_os` flags) |

## 3. Memory

Support-OS uses the `sup:` prefix. If a memory search tool (e.g. claude-mem MCP) is available, search `"sup: {PRODUCT}"` for recent context (open threads, canned responses, KB inventory, flagged trends). If no memory tool is available, proceed silently without it — never mention the failure as an error.

## 4. External Tools and Fallbacks

Support-OS never asks for helpdesk credentials or API keys. Pasted text and CSV exports are the default inputs; the one exception is a read-only pull from a helpdesk/CRM MCP connector already present in the session, offered only when `connectors.enabled` is true (see the connector-aware agents):

| Input | Primary | Fallback |
|---|---|---|
| Tickets | Pasted email/ticket text | CSV export from the helpdesk tool (Zendesk, Help Scout, Freshdesk, Intercom, plain inbox export) |
| Brand voice | marketing-os config (if `cross_os.marketing_os`) | `voice` section of this config; else neutral-friendly default |
| Product signal hand-off | po-os `discovery-voc` (if `cross_os.po_os`) | Present findings directly to the user |
| Web lookups | WebSearch/WebFetch for public docs | Ask the user to paste the relevant text |

Never fail hard on a missing integration — state the fallback used and continue.

## Upgrades and missing fields

Configs written by older plugin versions stay valid: treat any missing field as its documented default — in particular, a missing `connectors` block means `{ "enabled": true, "preferred": [] }`. Never require the user to re-run setup after a plugin update; offer it only if they want to use new options.

### Multiple data locations

If more than one location in the resolution order contains `config.json`, use the highest-priority one but TELL the user which other locations also hold support-os data (absolute paths + last-modified) and suggest `/support-os:setup migrate` to consolidate — silent divergence between locations is worse than a one-line warning.

### Empty but deliberately selected location

An explicitly chosen location that is empty is a signal, not a fallthrough. If `OS_HUB_DATA_DIR` is set but `$OS_HUB_DATA_DIR/support-os/` has no `config.json`, or `./os-data/` exists but `./os-data/support-os/` is empty, do NOT silently fall back to a lower-priority location:

- If a lower-priority location holds support-os data, say so and offer `/support-os:setup migrate` to bring it into the selected location (reading the old data for this session is fine, but say that is what you are doing).
- If no location holds any data, offer setup — the 3-question quick version inline or the full `/support-os:setup` wizard — writing the result to the selected location.
