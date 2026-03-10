---
name: find-people-octave
description: >
  Find people and contacts at companies using Octave. Trigger when the user
  asks to: find people, search for contacts, find decision makers, look up
  people at a company, find leads, find prospects, build a contact list, or
  find people by title/role. Also trigger on phrases like "who works at
  [company]", "find the VP of Sales at", "find people like", "decision makers
  at [company]", or "build a prospect list".
version: 1.0.0
---

# Find People with Octave

Search for people by name, company, title, role, or similarity to existing contacts using Octave's contact intelligence.

## Prerequisites

The Octave MCP must be connected. If tools like `find_person` or `find_similar_people` are not available, instruct the user:

> **Connect Octave:** Add the Octave MCP to Claude Code with your API key. See MCP setup instructions below.

Before making any Octave MCP calls, use `tool_search` to load the correct tool definitions and parameter schemas. Search for tools prefixed with `mcp__Octave__` or `mcp__Airbyte_Octave__` or `mcp__Velox_Octave__`.

## MCP Setup

```bash
claude mcp add octave --transport http --url "https://mcp.octave.run/v1?api_key=YOUR_OCTAVE_API_KEY"
```

Get your API key at [octave.run](https://octave.run).

## Workflow

### Find a specific person

Use `find_person` when the user provides a person's name and/or company:

```
Find John Smith at Stripe
```

Returns contact details including name, title, company, LinkedIn, and more.

### Find similar people

Use `find_similar_people` when the user wants to find contacts similar to an existing one:

```
Find people similar to our best customer contact
```

### Find people at a company by role

Combine `find_person` with title/role criteria:

```
Find the Head of Engineering at Notion
```

### Enrich a person

Use `enrich_person` for deep enrichment — employment history, social profiles, skills, and more.

### Qualify a person

Use `qualify_person` to score a contact against buyer persona criteria.

## Input Options

### For `find_person`:
- `first_name` + `last_name`
- `company_name` or `domain`
- `title` or `role` (e.g., "VP of Sales")
- `linkedin_url` (most accurate)

### For `find_similar_people`:
- Seed person details (name, title, company)
- Filtering criteria as supported

### For `enrich_person`:
- `linkedin_url` (preferred) or name + company

## If the User Provides a CSV

Parse it, extract name and company columns, normalize data, and process each contact. For large lists, batch and present results progressively.

## Combining with Other Tools

After finding people with Octave, suggest the natural next steps:
- **Find & verify emails** → Use the `find-verified-emails` skill (Signaliz)
- **Push to campaign** → Use Instantly to add leads to a campaign
- **Full pipeline** → Use the `lead-generation-pipeline` skill for the complete flow

## Output

Present results with:
- Full name and current title
- Company name and domain
- Location
- LinkedIn profile URL
- Seniority level and department
- Key skills or expertise areas
- Offer to find verified email (Signaliz) for any contact
- Offer to find similar people for any result
