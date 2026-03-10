---
name: find-companies-octave
description: >
  Find and research companies using Octave. Trigger when the user asks to:
  find companies, search for companies, discover target accounts, build an
  account list, find companies like [company], find similar companies, or
  research companies by criteria. Also trigger on phrases like "find me
  companies in [industry]", "who are the competitors of", "companies like
  [company]", "build a target account list", or "ICP matching".
version: 1.0.0
---

# Find Companies with Octave

Search for companies by name, domain, industry, or similarity to existing accounts using Octave's company intelligence.

## Prerequisites

The Octave MCP must be connected. If tools like `find_company` or `find_similar_companies` are not available, instruct the user:

> **Connect Octave:** Add the Octave MCP to Claude Code with your API key. See MCP setup instructions below.

Before making any Octave MCP calls, use `tool_search` to load the correct tool definitions and parameter schemas. Search for tools prefixed with `mcp__Octave__` or `mcp__Airbyte_Octave__` or `mcp__Velox_Octave__`.

## MCP Setup

```bash
claude mcp add octave --transport http --url "https://mcp.octave.run/v1?api_key=YOUR_OCTAVE_API_KEY"
```

Get your API key at [octave.run](https://octave.run).

## Workflow

### Find a specific company

Use `find_company` when the user provides a company name or domain:

```
Find information about Stripe
```

This returns company details including industry, size, location, tech stack, and more.

### Find similar companies

Use `find_similar_companies` when the user wants to find companies like an existing one:

```
Find companies similar to Stripe
```

Provide a company domain or name as the seed. Octave returns companies with similar characteristics.

### Enrich a company

Use `enrich_company` for deep enrichment on a known company — firmographics, technographics, funding history, and more.

### Qualify a company

Use `qualify_company` to score a company against qualification criteria or an ICP definition.

## Input Options

### For `find_company`:
- `company_name` — e.g., "Stripe"
- `domain` — e.g., "stripe.com" (preferred for accuracy)

### For `find_similar_companies`:
- `domain` — seed company domain
- `company_name` — seed company name
- Filtering criteria (industry, size, location) as supported

### For `enrich_company`:
- `domain` — company domain (required)

## If the User Provides a CSV

Parse it, extract `company_name` or `domain` column, and process each company. For large lists, batch and present results progressively.

## Output

Present results with:
- Company name, domain, and description
- Industry and sub-industry
- Employee count and revenue range
- Location (HQ)
- Key technologies in use
- Funding information if available
- Offer to find similar companies for any result
- Offer to proceed with email finding (Signaliz) or campaign creation (Instantly)
