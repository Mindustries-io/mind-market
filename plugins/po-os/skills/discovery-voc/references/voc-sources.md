# Voice-of-Customer Sources — Playbook

For each source type: what to pull, how to pull it, and how to interpret. All sources are opt-in (enabled in `config.voc_sources`).

---

## 1. Support Email / Tickets

**Enabled flag:** `voc_sources.support_email.enabled`
**Address:** `voc_sources.support_email.address` (usually matches `CONTACT_EMAIL` env var)
**Log path:** `voc_sources.support_email.log_path` (if the user exports manually)

### What to pull
- Last 7 / 30 / 90 days (per user request)
- Thread subjects, first-message bodies, resolution notes
- Tags / categories (if the user has a system)
- Response latencies (signal on ops capacity, not product — but worth noting)

### How to pull
- **If log file exists:** read it via `Read` or `mcp__185c596a-...__read_file_content`
- **If IMAP/API integration** is configured: fetch via provided credentials (never prompt for creds in this skill)
- **If nothing exists:** ask the user to paste or forward a sample; build a local log going forward

### How to interpret
- **High-frequency topics** = likely pain pattern or docs gap
- **Emotional language** (frustrated, confused, broken) = UX pain signal
- **"Can you add X?" explicit asks** = direct backlog input (but weigh — vocal minority)
- **Repeat senders** = power users; their feedback is biased toward advanced use
- **New-user vs. established-user** patterns differ; split them

### Biases
- Selection bias: only people who contact support
- Survivor bias: churners don't write in
- Severity bias: small annoyances under-represented (users work around silently)

---

## 2. Stripe Cancellation Reasons

**Enabled flag:** `voc_sources.stripe_churn.enabled`
**Collection method:** `voc_sources.stripe_churn.collection_method` — options:
- `stripe_metadata` — reason captured as metadata on cancelled subscriptions
- `billing_portal` — Stripe Billing Portal's cancellation-reason survey
- `manual_email` — ask via email on cancel
- `none` — not collected (suggest enabling)

### What to pull
- Cancelled subscriptions in time window
- Cancellation reason codes + free-text
- Subscription age at cancel (early churn vs. mature churn — different causes)
- Plan tier at cancel

### How to pull
- **Stripe API** if credentials are available (never in this skill — defer to a Stripe-authenticated surface)
- **Manual:** ask the user to paste a CSV export from Stripe Dashboard

### How to interpret
- **Early churn (< 30 days)** = onboarding / fit problem
- **Mid churn (30-90 days)** = value-realization gap
- **Late churn (> 90 days)** = sustained value decline, competitor switch, or business change
- **Cluster by reason:** price / missing feature / too complex / no longer need / competitor

### Red flags
- > 20% citing a single missing feature = clear backlog priority
- "Too expensive" mixed with "not using it" = activation problem masquerading as pricing
- "Moved to {competitor}" = competitive gap — feeds `competitive-po` context

---

## 3. Competitor Review Sites

**Enabled flag:** `voc_sources.competitor_reviews.enabled`
**Sites:** `voc_sources.competitor_reviews.sites` — array of `{competitor, site, url}`

### What to pull
- 1-3 star reviews (complaints)
- 4-5 star "but I wish…" reviews (enhancement ideas)
- Review date range (recency matters)
- Verified user, business size, use case (all three help segment)

### How to pull
- **G2** — public review pages (`/products/{slug}/reviews`). Use `WebFetch` or MCP scraper. Respect robots.
- **Capterra** — same pattern
- **Trustpilot** — same pattern, but note Trustpilot reviewer mix is more B2C-ish
- **TrustRadius** — similar
- **Gartner Peer Insights** — login-walled for deep reviews; only fetch public summaries

### How to interpret
- **Recurring complaint themes** = opportunity zones (especially if our product already solves them)
- **"Feature X is missing in {competitor}"** = direct gap — verify we have it, then lean into marketing or build if we don't
- **Pricing complaints** = calibration signal for our pricing
- **Onboarding / UX complaints** = universal theme; matters most if we share the pattern
- **Support complaints** = ops differentiator opportunity

### Biases
- Self-selection (happy silent majority, loud complainers)
- Incentivized reviews (many platforms give perks — flag verified reviews preferentially)
- Vertical skew (G2 = software buyers; Capterra = SMB; Trustpilot = consumer-heavy)

### Privacy / Ethics
- Paraphrase in memory; only quote verbatim in outputs when essential
- Never include reviewer names or company names without clear value
- Check platform ToS before programmatic scraping

---

## 4. Forums / Reddit

**Enabled flag:** `voc_sources.forums.enabled`
**Subs:** `voc_sources.forums.subs` — array of URLs or sub names

### What to pull
- Recent posts (last 30-90 days)
- Posts matching our topic keywords (GDPR, cookie compliance, CSRD, AI Act, privacy automation)
- Comment threads where users describe their process / stack
- Frustration patterns ("I'm tired of X", "no one has solved Y")

### How to pull
- **Reddit** — `www.reddit.com/r/{sub}/new.json` or `WebFetch` + search
- **Forums (non-Reddit)** — depends on platform; often `WebFetch` + `WebSearch` combo
- **Stack Overflow / developer forums** — for technical PO items

### How to interpret
- **"How do I do X?"** posts where X is our value prop = prospect awareness of problem without awareness of our solution (marketing gap)
- **"Is there a tool for X?"** = direct demand signal
- **"{competitor} doesn't do Y"** = validated gap worth pursuing
- **Step-by-step workarounds** = process pain we could automate

### Biases
- Reddit skews tech / B2B / English-speaking; localized forums needed for other markets
- Self-selection: people who post are a sliver of users
- Karma incentives drive certain kinds of posts

### Practical tips
- Target **3-5 subs** max; more becomes noise
- Italian/German/French forums exist (e.g., IT Telegram groups for privacy, DE forums like heise.de/security, FR forums like CNIL user groups) — relevant if markets include those countries

---

## 5. Onboarding Analytics

**Enabled flag:** `voc_sources.analytics.enabled`
**Platform:** `voc_sources.analytics.platform` — GA4, Mixpanel, PostHog, Amplitude, etc.
**Dashboard URL:** `voc_sources.analytics.dashboard_url`

### What to pull
- Onboarding funnel steps + drop-off %
- Time-to-first-value metric (time from signup to first "aha" event)
- Feature-adoption heatmap (which features get used, which don't)
- Session replays of stuck users (if available via Hotjar / FullStory)

### How to pull
- Delegate to `marketing-os:analytics-reporter` if marketing-os is installed (check `cross_os.marketing_os`)
- Otherwise, read a user-provided export

### How to interpret
- **High drop-off on step N** = backlog item: simplify/rewrite step N
- **Low feature-adoption on feature F** = either F is not needed, or F is hidden/broken (UX)
- **Time-to-value > 30 min** = onboarding overhaul candidate
- **Session replays** = gold standard; look for repeated click patterns on non-interactive elements (signals of confusion)

### Signal > Survey
Analytics show what people **do**, which is more truthful than what they say. When behavior and survey feedback diverge, trust behavior.

---

## 6. Prospect Interviews

**Enabled flag:** `voc_sources.prospect_interviews.enabled`
**Cadence:** `voc_sources.prospect_interviews.cadence` — "weekly", "monthly", "ad-hoc"
**Log path:** `voc_sources.prospect_interviews.log_path`

### What to pull
- Interview notes (structured or free-form)
- Quotes (verbatim is powerful)
- Willingness-to-pay signals
- Dealbreakers

### How to pull
- Read log file via `Read`
- If user hasn't run interviews, suggest a starter script (see "Hypothesis-mode" in SKILL.md)

### How to interpret
- **3-5 interviews** with the same pain = pattern forming
- **6+ interviews** with the same pain = pattern confirmed (but guard against selection bias — who did you interview?)
- **Verbatim phrases that recur** = language for marketing and product copy
- **Current workaround stack** = competitive landscape from the buyer's POV (often different from what we think of as competitors)

### Sample prospect interview script (Mom Test style)

1. "Walk me through the last time {problem our product solves} came up."
2. "What did you do?"
3. "What would have made that easier?"
4. "Have you ever tried a tool for that? Why didn't it work?"
5. "If a magic wand existed, what would it do?"
6. "What would it be worth to you?" (avoid asking directly if they'd buy — ask about value in their terms)

Avoid leading questions ("Would you use X if it did Y?" biases toward "yes").

---

## 7. NPS / CSAT

**Enabled flag:** `voc_sources.nps_csat.enabled`
**Tool:** `voc_sources.nps_csat.tool`
**Dashboard URL:** `voc_sources.nps_csat.dashboard_url`

### What to pull
- NPS score + distribution (promoters / passives / detractors)
- Free-text reasons
- CSAT per interaction / feature
- Trend over time

### How to pull
- Tool-specific API or manual export
- Delegate to `marketing-os:analytics-reporter` if marketing-os is installed

### How to interpret
- **Detractor free-text** is the highest-value slice — these are people at risk of churning or bad-mouthing
- **Promoter free-text** shows your moat — the things that made them love you (lean in)
- **Passive free-text** often has the best "would be great if" suggestions — passives become promoters with the right nudge

---

## Cross-Source Triangulation

The strongest signal is a pattern that appears in **multiple source types**. E.g.:
- Complaint in support email +
- Same theme in Stripe cancellation reason +
- Same theme in competitor G2 reviews

That's a P0 signal. Single-source patterns are hypotheses until triangulated.

Encourage active collection of sources currently at `enabled: false`. A VoC agent with 3 live sources is worth 10x one with 1 source.
