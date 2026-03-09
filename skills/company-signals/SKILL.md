---
name: company-signals
description: >
  Enrich companies with business signals using Signaliz. Trigger when the
  user asks to: research a company, get company signals, find hiring trends,
  check funding status, discover tech stack, get company intelligence, or
  enrich company data. Also trigger on phrases like "what's happening at
  [company]", "company research", "enrichment", "account intel", or
  "what signals do we have on [domain]".
version: 1.0.0
---

# Company Signal Enrichment with Signaliz

Discover hiring trends, funding events, tech stack, news, leadership changes, and more for target companies.

## Prerequisites

The Signaliz MCP must be connected. If tools like `execute_primitive` are not available, instruct the user:

> **Connect Signaliz:** Run `/signaliz:setup` or add your API key to the Signaliz MCP configuration. Get a free API key at [signaliz.com/signup](https://signaliz.com/signup).

Before making any Signaliz MCP calls, use `tool_search` to load the correct tool definitions and parameter schemas.

## Input Requirements

Each record needs:
- `company_domain` (e.g., "stripe.com") — OR —
- `company_name` (e.g., "Stripe")

Domain is strongly preferred for accuracy.

## Workflow

### For 1–25 companies: Use `execute_primitive`

```json
{
  "capability_id": "enrich_company_signals",
  "input_data": [
    {"company_domain": "stripe.com"},
    {"company_domain": "notion.so"}
  ]
}
```

### For 26–5,000 companies: Use `enrich_company_signals`

```json
{
  "companies": [
    {"domain": "stripe.com"},
    {"domain": "notion.so"}
  ]
}
```

This returns a `job_id`. Poll with `check_job_status`.

### Optional Configuration

You can customize enrichment with config options:
- `signal_types` — array: "hiring", "funding", "product_launch", "partnership", "leadership_change", "expansion", "acquisition", "award", "regulatory", "earnings"
- `research_prompt` — custom research question
- `enable_deep_search` — boolean, uses web search for deeper signals (more credits)
- `lookback_days` — integer, how many days back to search (default: 90)

## Signal Types Returned

- **Hiring** — job postings, team growth, new roles
- **Funding** — fundraising rounds, valuations, investors
- **Tech Stack** — technologies and tools in use
- **News** — press mentions, product launches, partnerships
- **Leadership** — executive changes, board appointments
- **Expansion** — new offices, market entry, geographic growth
- **Acquisition** — M&A activity, targets, buyers

## If the User Provides a CSV

Parse it, extract `company_domain` or `domain` column (fall back to `company_name`), normalize domains, and batch in groups of 25.

## Error Handling

| Problem | Fix |
|---------|-----|
| All tools fail | Reconnect Signaliz MCP |
| Timeout | Reduce batch size to 10–15 |
| No signals found | Company may be too small or private — try with `enable_deep_search: true` |

## Output

Present signals organized by company with:
- Company name and domain
- Signal summary grouped by type
- Key highlights (recent funding, active hiring, tech changes)
- Offer to export as CSV for CRM import
