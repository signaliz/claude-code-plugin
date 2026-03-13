---
name: apollo-enrich
description: >
  Search and enrich contacts and companies with Apollo.io. Trigger when the user
  asks to: search Apollo, enrich with Apollo, find contacts in Apollo, look up
  a company on Apollo, add contacts to Apollo sequences, or use Apollo for
  prospecting. Also trigger on phrases like "Apollo lookup", "search Apollo for",
  "enrich these contacts with Apollo", "add to Apollo sequence", or "find leads
  on Apollo".
version: 1.0.0
---

# Apollo.io Search & Enrichment

Search, enrich, create, and manage contacts and companies using Apollo.io's database and sequencing tools.

## Prerequisites

The Apollo MCP must be connected. If Apollo tools are not available, instruct the user:

> **Connect Apollo.io:**
>
> ```bash
> claude mcp add apollo --transport stdio --command "npx @thevgergroup/apollo-io-mcp" --env APOLLO_API_KEY=YOUR_APOLLO_API_KEY
> ```
>
> Get your API key from Apollo → Settings → Integrations → API.
>
> **Or** enable the native Apollo connector in Claude.ai → Settings → Connected Apps → Apollo.

Before making any Apollo MCP calls, use `tool_search` to load the correct tool definitions and parameter schemas.

## Capabilities

### Search

| Operation | Description |
|-----------|-------------|
| Search people | Find contacts by name, title, company, industry, location |
| Search companies | Find organizations by name, domain, industry, size |

### Enrich

| Operation | Description |
|-----------|-------------|
| Enrich contact | Get full profile — email, phone, title, employment history |
| Enrich company | Get firmographics — industry, size, funding, tech stack |

### Write

| Operation | Description |
|-----------|-------------|
| Create contact | Add a new person to Apollo |
| Update contact | Modify existing contact fields |
| Add to sequence | Enroll contacts in an Apollo sequence |

## Workflow

### Search and Enrich

When the user wants to find and enrich contacts:

1. Use Apollo's people search with criteria (title, company, industry)
2. Enrich top results for detailed profiles
3. Present results with key fields

### Cross-Platform Enrichment

Apollo works as an enrichment layer alongside other tools:

```
Octave find companies → Apollo enrich contacts → Signaliz verify emails → Instantly campaign
```

### Sequence Management

When the user wants to add contacts to Apollo sequences:

1. Search for the target sequence
2. Add contacts with personalization variables
3. Confirm enrollment

## Combining with Other Tools

- **Apollo → Signaliz** — Verify Apollo emails before sending
- **Apollo → Instantly** — Push enriched contacts to Instantly campaigns
- **Apollo → Affinity** — Sync contact data to Affinity CRM
- **Lusha → Apollo** — Cross-reference contact data for completeness
- **Octave → Apollo** — Find companies with Octave, enrich with Apollo

## Payload Optimization

The `@thevgergroup/apollo-io-mcp` server returns 94% smaller payloads compared to raw Apollo API, optimized for LLM context windows. This is important for large result sets.

## Output

Present results with:
- Full name, title, and seniority
- Company name, domain, and industry
- Email and phone (when available)
- LinkedIn URL
- Employment history highlights
- Offer to verify emails (Signaliz), add to campaign (Instantly), or save to CRM (Affinity)
