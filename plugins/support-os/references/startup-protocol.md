# Support-OS Startup Protocol (shared)

Every Support-OS agent and skill follows these steps before doing any task.

## 1. Load Configuration

Read `~/.claude/plugins/data/support-os/config.json` and extract the `active_profile`.

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

Support-OS never asks for helpdesk credentials or connects to live helpdesk APIs. Inputs are always local:

| Input | Primary | Fallback |
|---|---|---|
| Tickets | Pasted email/ticket text | CSV export from the helpdesk tool (Zendesk, Help Scout, Freshdesk, Intercom, plain inbox export) |
| Brand voice | marketing-os config (if `cross_os.marketing_os`) | `voice` section of this config; else neutral-friendly default |
| Product signal hand-off | po-os `discovery-voc` (if `cross_os.po_os`) | Present findings directly to the user |
| Web lookups | WebSearch/WebFetch for public docs | Ask the user to paste the relevant text |

Never fail hard on a missing integration — state the fallback used and continue.
