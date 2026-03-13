---
name: lusha-contacts
description: >
  Discover and enrich contacts using Lusha's 100M+ contact database. Trigger
  when the user asks to: find contacts with Lusha, prospect for leads, look up
  phone numbers, find direct dials, bulk contact lookup, discover contacts by
  seniority or title, or enrich contacts with phone and email. Also trigger on
  phrases like "use Lusha to find", "Lusha lookup", "get phone numbers for",
  "prospect in [industry/location]", or "bulk enrich these contacts".
version: 1.0.0
---

# Lusha Contact Discovery & Enrichment

Discover contacts and enrich them with verified emails and direct phone numbers using Lusha's 100M+ contact / 60M+ company database.

## Prerequisites

The Lusha MCP must be connected. If Lusha tools are not available, instruct the user:

> **Connect Lusha:**
>
> ```bash
> claude mcp add lusha --transport stdio --command "npx @lusha-org/mcp@latest" --env LUSHA_API_KEY=YOUR_LUSHA_API_KEY
> ```
>
> Get your API key from Lusha → Settings → API.

Before making any Lusha MCP calls, use `tool_search` to load the correct tool definitions and parameter schemas.

## Available Tools

| Tool | Description | Limits |
|------|-------------|--------|
| `prospecting` | Search for contacts with filters (title, seniority, location, industry) | Flexible filters |
| `personBulkLookup` | Enrich up to 100 contacts at once with emails and phone numbers | Max 100 per call |
| `companyBulkLookup` | Enrich companies with firmographic data | Bulk lookup |

## Workflow

### Prospecting — Discover New Contacts

When the user wants to find contacts by criteria:

1. Use `prospecting` with filters:
   - **Title/Role**: "VP of Sales", "Head of Engineering"
   - **Seniority**: C-level, VP, Director, Manager
   - **Location**: Country, state, city
   - **Industry**: SaaS, Fintech, Healthcare, etc.
   - **Company size**: Employee count ranges
2. Review results and select targets
3. Enrich selected contacts for full details

### Bulk Enrichment — Enrich Known Contacts

When the user has a list of contacts to enrich:

1. Parse input (CSV, list, or names + companies)
2. Use `personBulkLookup` with up to 100 contacts per call
3. Return enriched data: email, phone, title, company

### Company Lookup

When the user wants company-level data:

1. Use `companyBulkLookup` with company names or domains
2. Return firmographic data: industry, size, revenue, location

## Combining with Signaliz

Lusha provides initial contact discovery → Signaliz verifies email deliverability:

```
Lusha prospecting → Contacts with emails → Signaliz verify_emails → Confirmed deliverable
```

This two-step approach maximizes deliverability:
1. Lusha finds the email addresses
2. Signaliz verifies they won't bounce

## Combining with Other Tools

- **Lusha → Signaliz** — Verify emails found by Lusha
- **Lusha → Instantly** — Push enriched contacts to outbound campaigns
- **Lusha → Affinity** — Add discovered contacts to CRM pipeline
- **Lusha → Apollo** — Cross-reference and fill gaps in contact data
- **Octave → Lusha** — Find companies with Octave, enrich contacts with Lusha

## Credit Usage

Lusha uses API credits from your existing Lusha plan. Prospecting searches and lookups consume credits. Check your plan limits before bulk operations.

## Output

Present results with:
- Full name and current title
- Company name and domain
- Verified email address
- Direct phone number (when available)
- Location
- Offer to verify emails via Signaliz
- Offer to push to Instantly campaign
- Offer to add to Affinity CRM
