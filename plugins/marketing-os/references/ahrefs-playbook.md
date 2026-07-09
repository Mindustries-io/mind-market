# Ahrefs API Playbook

Quick reference for common Ahrefs API patterns used by the SEO Strategist.

## General Rules

- Always `mode=subdomains` for domain-level analysis
- Always check `doc` tool for exact schema: `doc({ tool: "endpoint-name" })`
- Date format: YYYY-MM-DD
- Country codes: 2-letter lowercase (us, gb, de, fr, it, es, etc.)
- `select` is required for most endpoints — only request fields you need
- `where` supports: `=`, `!=`, `>`, `>=`, `<`, `<=`, `contains`, `not_contains`
- `order_by` format: "field:asc" or "field:desc"
- Default `limit`: 100 (max varies by endpoint)

## Common Patterns

### Quick Domain Overview
```
site-explorer-metrics({
  target: "example.com",
  mode: "subdomains",
  date: "2026-04-04",
  country: "us"
})
```

### Find Easy Keyword Wins
```
site-explorer-organic-keywords({
  target: "example.com",
  mode: "subdomains",
  country: "us",
  date: "2026-04-04",
  select: "keyword,position,volume,traffic,url",
  where: "position >= 4 AND position <= 20 AND volume >= 100",
  order_by: "volume:desc",
  limit: 50
})
```
These are keywords you already rank for (positions 4-20) that could move to top 3 with optimization.

### Competitor Discovery
```
site-explorer-organic-competitors({
  target: "example.com",
  mode: "subdomains",
  country: "us",
  date: "2026-04-04",
  select: "domain,common_keywords,organic_keywords,organic_traffic",
  order_by: "common_keywords:desc",
  limit: 10
})
```

### Multi-Domain Comparison
```
batch-analysis({
  targets: [
    { url: "example.com", mode: "subdomains", protocol: "both" },
    { url: "competitor1.com", mode: "subdomains", protocol: "both" },
    { url: "competitor2.com", mode: "subdomains", protocol: "both" }
  ],
  select: ["domain_rating", "organic_traffic", "organic_keywords", "referring_domains"],
  country: "us"
})
```

### Keyword Research Pipeline
```
Step 1: keywords-explorer-overview({
  keywords: "seed keyword 1,seed keyword 2",
  country: "us",
  select: "keyword,volume,keyword_difficulty,cpc,traffic_potential,parent_keyword"
})

Step 2: keywords-explorer-matching-terms({
  keywords: "seed keyword",
  country: "us",
  select: "keyword,volume,keyword_difficulty,cpc,traffic_potential",
  where: "volume >= 100",
  order_by: "volume:desc",
  limit: 50
})

Step 3: keywords-explorer-related-terms({
  keywords: "seed keyword",
  country: "us",
  select: "keyword,volume,keyword_difficulty,cpc",
  order_by: "volume:desc",
  limit: 30
})
```

### Backlink Overview
```
site-explorer-backlinks-stats({
  target: "example.com",
  mode: "subdomains",
  date: "2026-04-04"
})
```

### Top Pages by Traffic
```
site-explorer-top-pages({
  target: "example.com",
  mode: "subdomains",
  country: "us",
  date: "2026-04-04",
  select: "url,organic_traffic,organic_keywords,top_keyword,top_keyword_volume",
  order_by: "organic_traffic:desc",
  limit: 20
})
```

### Site Audit Issues
```
Step 1: site-audit-projects({ project_id: 12345 })
Step 2: site-audit-issues({ project_id: 12345 })
```

### SERP Analysis
```
serp-overview({
  keyword: "target keyword",
  country: "us",
  select: "url,title,position,backlinks,referring_domains,domain_rating,organic_traffic",
  top_positions: 10
})
```

## Date Ranges

For historical data, common ranges:
- Last 30 days: date_from = 30 days ago, date_to = today
- Last 3 months: date_from = 3 months ago
- Last 6 months: date_from = 6 months ago
- Last year: date_from = 1 year ago
- Weekly grouping: `history_grouping: "weekly"`
- Monthly grouping: `history_grouping: "monthly"`

## Quota Management

Check usage before large batch operations:
```
subscription-info-limits-and-usage()
```
