---
name: sourcescrub-signals
description: >
  Research companies and contacts with SourceScrub market intelligence. Trigger
  when the user asks to: search SourceScrub, find companies on SourceScrub,
  get SourceScrub signals, look up conferences, or research private companies.
  Also trigger on phrases like "SourceScrub lookup", "private company data",
  "conference attendees", or "SourceScrub company search".
version: 1.0.0
---

# SourceScrub Market Intelligence

Search for companies, contacts, and conference data using SourceScrub's private company intelligence platform.

## Prerequisites

**SourceScrub requires a custom MCP server** — no official or community MCP exists yet. Until the custom server is built, SourceScrub data must be accessed manually or via its native integrations with Affinity, Salesforce, DealCloud, and SugarCRM.

### When Custom MCP Is Available

> **Connect SourceScrub:**
>
> ```bash
> claude mcp add sourcescrub --transport http --url "https://YOUR_SOURCESCRUB_MCP_URL" --env SOURCESCRUB_USERNAME=YOUR_USERNAME --env SOURCESCRUB_PASSWORD=YOUR_PASSWORD
> ```

### API Access

SourceScrub API access requires contacting their team:
- API endpoint: `api.sourcescrub.com`
- Auth: Username/password → bearer token (with TTL caching)
- Contact SourceScrub for credentials

## Planned Capabilities (Custom Build)

| Tool | Description |
|------|-------------|
| `search_companies` | Search companies by criteria (industry, size, location, signals) |
| `get_company` | Get detailed company profile |
| `search_contacts` | Find contacts at SourceScrub-tracked companies |
| `get_contact` | Get contact details |
| `get_company_signals` | Retrieve growth signals, hiring, tech stack changes |
| `search_conferences` | Search conference and event data |

## Interim Workflow

Until the custom MCP is built, use SourceScrub data via:

1. **Affinity integration** — If connected, SourceScrub data flows into Affinity automatically
2. **Manual export** — Export from SourceScrub UI, import as CSV, process with other tools
3. **Signaliz signals** — Use Signaliz company signals as a partial alternative for company intelligence

## Combining with Other Tools

- **SourceScrub → Octave** — Identify companies on SourceScrub, find contacts via Octave
- **SourceScrub → Signaliz** — Get SourceScrub signals + Signaliz signals for comprehensive view
- **SourceScrub → Affinity** — Push SourceScrub discoveries to CRM (native integration)
- **SourceScrub → Lusha/Apollo** — Find contacts at SourceScrub-identified companies

## Build Status

| Item | Status |
|------|--------|
| API access | Pending — contact SourceScrub team |
| Custom MCP server | Not started — blocked on API access |
| Stack | TypeScript + FastMCP (planned) |
| Estimated effort | 2–3 days after API access |

## Output

When the custom MCP is available, present results with:
- Company name, domain, and description
- Growth signals and market activity
- Contact information
- Conference/event participation
- Offer to cross-reference with Signaliz, Octave, or Crunchbase
