# Brand Radar API Guide

Reference for using the Ahrefs Brand Radar tools to track AI visibility.

## Prerequisites

Brand Radar requires configured reports in Ahrefs. Check existing reports:
```
management-brand-radar-reports()
```

If the brand has a report, use its `report_id` for all queries. If not, you can still query by `brand` name directly in some endpoints.

## Available Data Sources

The `data_source` parameter accepts AI platform identifiers. Always check the `doc` tool for the current list:
```
doc({ tool: "brand-radar-sov-overview" })
```

Common values: `"chatgpt"`, `"perplexity"`, `"gemini"`, `"claude"`, `"copilot"`

You can query multiple sources simultaneously by passing an array.

## Common Patterns

### Share of Voice (SOV)

SOV measures what percentage of AI responses about your industry mention your brand vs competitors.

```
brand-radar-sov-overview({
  select: "brand,sov,impressions,mentions",
  data_source: ["chatgpt"],
  brand: "YourBrand",
  competitors: "Competitor1,Competitor2,Competitor3"
})
```

For trend data:
```
brand-radar-sov-history({
  date_from: "2026-01-01",
  data_source: ["chatgpt"],
  brand: "YourBrand",
  competitors: "Competitor1,Competitor2"
})
```

### Brand Mentions

Count how often the brand appears in AI responses:

```
brand-radar-mentions-overview({
  select: "brand,mentions,data_source",
  data_source: ["chatgpt", "perplexity"],
  brand: "YourBrand"
})
```

### Actual AI Responses

See the real AI-generated text that mentions the brand:

```
brand-radar-ai-responses({
  select: "prompt,response,data_source,cited_urls,search_volume",
  data_source: ["chatgpt"],
  brand: "YourBrand",
  limit: 20,
  order_by: "search_volume:desc"
})
```

This is valuable for understanding:
- How AI describes your brand
- What prompts trigger mentions
- Which sources AI cites alongside your brand

### Cited Pages

See which of your pages AI platforms reference:

```
brand-radar-cited-pages({
  select: "url,mentions_count,impressions",
  data_source: ["chatgpt"],
  brand: "YourBrand",
  order_by: "mentions_count:desc",
  limit: 20
})
```

### Cited Domains

See which domains appear alongside your brand in AI responses:

```
brand-radar-cited-domains({
  select: "domain,mentions_count,impressions",
  data_source: ["chatgpt"],
  brand: "YourBrand",
  order_by: "mentions_count:desc",
  limit: 20
})
```

## Interpretation Guidelines

- **SOV > 30%:** Strong AI visibility in your category
- **SOV 10-30%:** Moderate presence, room for improvement
- **SOV < 10%:** Low visibility, significant opportunity
- **Rising mentions + stable SOV:** Market is growing, you're keeping pace
- **Rising mentions + declining SOV:** Competitors are growing faster
- **Declining mentions:** AI platforms may be citing different sources
- **High impressions, low mentions:** Brand appears in high-traffic queries but not frequently

## Optimization Strategies

Based on data patterns:

1. **Low SOV, competitors cited more:** Create authoritative content on topics competitors dominate
2. **Pages not cited:** Improve E-E-A-T signals (expertise, experience, authoritativeness, trustworthiness)
3. **Wrong pages cited:** Ensure canonical pages have comprehensive, structured content
4. **Negative sentiment in AI responses:** Address the issues at the source (product, service, reviews)
5. **Low impressions:** Target higher-volume prompts/queries in your content strategy
