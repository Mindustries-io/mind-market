# Marketing OS — User Guide

A full-stack marketing operating system for Claude Code: one orchestrator skill (the CMO) that delegates to seven specialist **agents**, all driven from your CLI.

---

## Architecture

```
/marketing-os:cmo  (orchestrator skill — runs in your conversation)
        |
        |  Agent tool: subagent_type "marketing-os:<agent>"
        v
  agents/                          model    effort
    seo-strategist.md              sonnet   medium
    content-director.md            sonnet   medium
    competitive-intel.md           inherit  high
    email-campaigns.md             sonnet   medium
    social-media.md                sonnet   medium
    analytics-reporter.md          haiku    low
    brand-guardian.md              sonnet   medium
```

- The **CMO** routes your request. Single-domain requests go to exactly ONE agent (fast mode) and its answer is relayed directly. Only genuinely multi-domain requests (e.g. "plan a product launch") run a pipeline — and the CMO tells you which agents will run first.
- Each specialist also has a **thin trigger skill** (`/marketing-os:seo-strategist ...`) that delegates straight to its agent, skipping the CMO.
- All agents share one startup protocol (`references/startup-protocol.md`): config load, `mos:` memory namespace, and integration fallbacks.
- Deep-dive references (Ahrefs playbook, content frameworks, channel/platform specs, sequence templates, KPI definitions, Brand Radar guide) live in plugin-root `references/` and are loaded lazily — only when a task needs them.

## Quick start

### 1. Set up your first brand

```
/marketing-os:setup
```

The wizard captures: brand name and domain, **which integrations are connected (Ahrefs, claude-mem, nano-banana)**, competitors (auto-discovered from Ahrefs when connected, manual otherwise), target countries, active channels, brand voice, and reporting preferences. Config is saved to `~/.claude/plugins/data/marketing-os/config.json`; multiple brand profiles are supported.

No config yet? Agents don't hard-stop — they offer a 3-question inline quick-setup or proceed with the context you gave.

### 2. Talk to the CMO

```
/marketing-os:cmo What are my best keyword opportunities?
/marketing-os:cmo Write a blog post about cloud migration
/marketing-os:cmo Design a welcome email sequence
/marketing-os:cmo Give me a weekly performance report
/marketing-os:cmo Plan a product launch campaign for [product]
```

### 3. Or go direct to a specialist

```
/marketing-os:seo-strategist Find easy keyword wins (positions 4-20)
/marketing-os:content-director Write a landing page for [product]
/marketing-os:competitive-intel Build a battlecard for [competitor]
/marketing-os:email-campaigns Design a cart abandonment sequence
/marketing-os:social-media Write a LinkedIn post about [topic]
/marketing-os:analytics-reporter Monthly performance report
/marketing-os:brand-guardian Check my AI share of voice
```

## The team

| Agent | Domain | When to use |
|---|---|---|
| **CMO** (skill) | Orchestration | You're not sure who to ask, or need multiple specialists |
| **SEO Strategist** | Organic search | Keywords, backlinks, audits, SERP analysis, technical SEO |
| **Content Director** | Content creation | Blog posts, landing pages, calendars, briefs |
| **Competitive Intel** | Market analysis | Competitor research, gaps, battlecards, positioning |
| **Email Campaigns** | Email marketing | Sequences, newsletters, drip flows, subject lines |
| **Social Media** | Social channels | Platform-specific posts, calendars, hashtags |
| **Analytics Reporter** | Performance data | Traffic reports, dashboards, KPI tracking |
| **Brand Guardian** | Brand health | AI visibility, share of voice, voice consistency |

## Models & cost

Each agent declares a model tier in its frontmatter so routine work never burns premium tokens:

| Agent | Model | Effort | Why |
|---|---|---|---|
| analytics-reporter | `haiku` | low | Mechanical data pulls and report formatting |
| social-media | `sonnet` | medium | Playbook-driven platform copywriting |
| email-campaigns | `sonnet` | medium | Template-driven sequence design and copy |
| content-director | `sonnet` | medium | Structured drafting and editorial planning |
| seo-strategist | `sonnet` | medium | Playbook-driven data analysis and synthesis |
| brand-guardian | `sonnet` | medium | Scorecard-driven monitoring and voice review |
| competitive-intel | `inherit` | high | Judgment-heavy strategic analysis; errors are costly |

**Portability note:** Model aliases resolve on official Anthropic backends. If you run Claude Code against a proxy (LiteLLM, Ollama, Bedrock/Vertex), map the aliases via `ANTHROPIC_DEFAULT_HAIKU_MODEL` / `ANTHROPIC_DEFAULT_SONNET_MODEL` (etc.) or edit the agent frontmatter to `model: inherit`. The `effort` field is Anthropic-specific and is ignored elsewhere. Step-by-step proxy setup: see "Using the plugins on a proxied backend" in the [marketplace README](https://github.com/Mindustries-io/mind-market#using-the-plugins-on-a-proxied-backend).

## Integrations (all optional)

Marketing OS names its MCP servers and discovers the exact tool names at runtime — nothing is hardcoded, and nothing fails hard when a server is missing.

| Integration | Config flag | Provides | Fallback when unavailable |
|---|---|---|---|
| **Ahrefs MCP server** | `integrations.ahrefs` | Site Explorer, Keywords Explorer, Site Audit, Rank Tracker, Brand Radar (AI visibility), Web Analytics, GSC data | WebSearch + data you paste (exports/CSV), output marked **"Degraded insights"** |
| **claude-mem** (memory MCP) | `integrations.claude_mem` | Cross-session memory under the `mos:` namespace | Memory steps skipped silently |
| **nano-banana** (image MCP) | `integrations.nano_banana` | AI-generated marketing images | You get a detailed image prompt instead of a generated image |
| **WebSearch** | — (built in) | Trends, news, competitor research, hashtag research | Always available |

Reconnected an integration later? `/marketing-os:setup ahrefs is now connected` flips the flag.

## How conversations work

**Single-task (fast mode):** "Write a blog post about X" → CMO delegates to content-director only → relays the draft. No pipeline, no synthesis pass.

**Multi-domain:** "Create a content calendar with distribution plan" → CMO announces it will run seo-strategist (keywords) → content-director (calendar) → social-media (distribution), then synthesizes one unified plan.

**Full campaign pipeline** (only for launches / comprehensive plans): competitive-intel → seo-strategist → content-director → email-campaigns → social-media → analytics-reporter → brand-guardian → unified campaign plan.

## Tips for better results

| Instead of | Try |
|---|---|
| "Do SEO" | "Find keywords I can rank for with KD under 30 and volume over 500" |
| "Write content" | "Write a 2000-word how-to guide targeting [keyword]" |
| "Check competitors" | "Compare my backlink profile to [competitor.com]" |
| "Email campaign" | "Design a 5-email welcome sequence for SaaS trial signups" |

Mention the output format you want ("give me a table of...", "write a full draft...", "create a brief for..."), and reference competitors by name — configured or ad-hoc (`/marketing-os:competitive-intel Analyze hubspot.com vs salesforce.com`).

With claude-mem connected, findings persist across sessions: run keyword research today, ask for "a content calendar based on the keyword research" tomorrow.

## Managing multiple brands

```
/marketing-os:setup NewBrand              # add a second brand
/marketing-os:setup switch to [brand]     # change active profile
/marketing-os:setup add competitor example.com
/marketing-os:setup change tone to casual
```

Or edit `~/.claude/plugins/data/marketing-os/config.json` directly (`active_profile` selects the brand).

## Troubleshooting

- **"Config not found"** — run `/marketing-os:setup`, or answer the inline quick-setup an agent offers.
- **"Degraded insights" notes on outputs** — the Ahrefs MCP server isn't connected (or `integrations.ahrefs` is false); connect it and update the flag via setup.
- **No data returned with Ahrefs connected** — check your API quota: `/marketing-os:cmo Check my Ahrefs API usage`.
- **Wrong brand context** — `/marketing-os:setup show current config`.
