---
name: crunchbase-research
description: >
  Research companies, funding, and market data with Crunchbase. Trigger when the
  user asks to: look up funding rounds, find investors, check company valuations,
  research acquisitions, find IPO data, look up Crunchbase, or get market
  intelligence. Also trigger on phrases like "who funded [company]",
  "funding history for", "Crunchbase lookup", "latest funding rounds in
  [industry]", "acquisition history", or "company financials on Crunchbase".
version: 1.0.0
---

# Crunchbase Market Intelligence

Research companies, funding rounds, investors, acquisitions, IPOs, and market trends using Crunchbase.

## Prerequisites

The Crunchbase MCP must be connected. If Crunchbase tools are not available, instruct the user:

> **Connect Crunchbase:**
>
> ```bash
> # Basic (5 tools, free API key)
> claude mcp add crunchbase --transport stdio --command "npx @cyreslab/crunchbase-mcp-server" --env CRUNCHBASE_API_KEY=YOUR_KEY
> ```
>
> Get your API key from Crunchbase → Settings → API.

Before making any Crunchbase MCP calls, use `tool_search` to load the correct tool definitions and parameter schemas.

## Available Tools

### Core Tools (Cyreslab-AI)

| Tool | Description |
|------|-------------|
| Search companies | Find companies by name, industry, or criteria |
| Get company details | Full company profile from Crunchbase |
| Get funding rounds | Funding history with amounts, investors, dates |
| Get acquisitions | M&A activity for a company |
| Search people | Find founders, executives, and investors |

## Workflow

### Company Research

When the user asks about a company:

1. Search for the company on Crunchbase
2. Pull full company details (founded, HQ, description, categories)
3. Get funding rounds (series, amounts, investors, dates)
4. Get acquisition history if relevant
5. Present a comprehensive company profile

### Funding Intelligence

When the user asks about funding:

1. Search by company, industry, or funding stage
2. Pull funding round details
3. Identify key investors and co-investors
4. Surface trends (round sizes, timing, sector activity)

### Investor Research

When the user asks about an investor or VC:

1. Search for the person or organization
2. Pull their investment history
3. Show portfolio companies and sectors
4. Highlight recent activity

## Combining with Other Tools

Crunchbase provides market intelligence that enriches the sales workflow:

- **Crunchbase → Octave** — Research funding, then find companies matching criteria
- **Crunchbase → Signaliz** — Get company signals after identifying funded companies
- **Crunchbase → Lusha/Apollo** — Find contacts at recently funded companies
- **Crunchbase → Affinity** — Add funded companies to CRM pipeline
- **Crunchbase → Instantly** — Build campaigns targeting recently funded companies

### Example Pipeline

```
Crunchbase: Find Series B companies in fintech (last 6 months)
    → Octave: Find VP of Sales at each company
    → Signaliz: Find and verify emails
    → Instantly: Launch outbound campaign
```

## Output

Present results with:
- Company name, domain, and description
- Founded date, HQ location, employee count
- Total funding raised and last round details
- Key investors
- Recent acquisitions or exits
- Offer to find contacts (Lusha/Apollo), verify emails (Signaliz), or add to pipeline (Affinity)
