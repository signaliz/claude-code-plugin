---
name: alphasense-research
description: >
  Research companies and markets with AlphaSense enterprise intelligence.
  Trigger when the user asks to: search AlphaSense, find earnings transcripts,
  research analyst reports, look up expert calls, get financial intelligence,
  or enterprise research. Also trigger on phrases like "AlphaSense lookup",
  "earnings transcript for", "analyst reports on", "expert call about",
  or "AlphaSense research".
version: 1.0.0
---

# AlphaSense Enterprise Research Intelligence

Search documents, earnings transcripts, expert calls, and financial data using AlphaSense's enterprise research platform.

## Prerequisites

**AlphaSense requires a custom MCP server** — no official or community MCP exists yet. AlphaSense also requires an Enterprise license and explicit API credentials.

### When Custom MCP Is Available

> **Connect AlphaSense:**
>
> ```bash
> claude mcp add alphasense --transport http --url "https://YOUR_ALPHASENSE_MCP_URL" --env ALPHASENSE_CLIENT_ID=YOUR_CLIENT_ID --env ALPHASENSE_CLIENT_SECRET=YOUR_SECRET --env ALPHASENSE_API_KEY=YOUR_API_KEY
> ```

### API Access

- API endpoint: `api.alpha-sense.com/gql` (GraphQL)
- Auth: API key + client_id/client_secret + OAuth bearer token (password grant)
- Contact: apisupport@alpha-sense.com for Enterprise API access

**Important:** Alpha Vantage (financial market data) is a completely different product from AlphaSense (enterprise research intelligence). Do not confuse the two.

## Planned Capabilities (Custom Build)

| Tool | Description |
|------|-------------|
| `search_documents` | Search across research library with keyword/date/type filters |
| `get_document` | Retrieve specific document content |
| `search_companies` | Company lookup and financial filtering |
| `get_company_financials` | Financial data and metrics for a company |
| `get_earnings_transcripts` | Earnings call transcripts with keyword highlighting |
| `search_expert_calls` | Search expert interview/call library |

## Interim Workflow

Until the custom MCP is built:

1. **Crunchbase** — Use Crunchbase for funding and financial data as a partial alternative
2. **Manus AI** — Delegate deep research tasks to Manus agents
3. **Signaliz signals** — Company signals provide some intelligence overlap
4. **Manual access** — Use AlphaSense's web UI and export findings

## Combining with Other Tools

- **AlphaSense → Octave** — Research insights guide company targeting
- **AlphaSense → Crunchbase** — Cross-reference financial data
- **AlphaSense → Manus** — Combine AlphaSense data with Manus deep research
- **AlphaSense → Affinity** — Push research insights to CRM as notes

## Build Status

| Item | Status |
|------|--------|
| Enterprise license | Pending — required for API access |
| API credentials | Pending — contact apisupport@alpha-sense.com |
| GraphQL schema | Unknown — needs discovery after access |
| Custom MCP server | Not started — blocked on license + API access |
| Stack | TypeScript + FastMCP + graphql-request (planned) |
| Estimated effort | 3–4 days after API access |

## Output

When the custom MCP is available, present results with:
- Document title, type, and date
- Key excerpts and highlights
- Company financial summaries
- Earnings call key takeaways
- Expert call summaries
- Offer to cross-reference with Crunchbase, Signaliz, or Manus
