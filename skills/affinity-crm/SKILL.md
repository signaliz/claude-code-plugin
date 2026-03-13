---
name: affinity-crm
description: >
  Manage CRM data with Affinity — pull company records, contacts, lists, notes,
  and opportunities. Trigger when the user asks to: check Affinity, pull CRM
  data, get company records, find contacts in CRM, look up a deal, check
  pipeline, update Affinity, add to a list, or manage CRM entries. Also trigger
  on phrases like "what's in Affinity for [company]", "CRM lookup", "add them
  to the [list] in Affinity", "update the deal stage", or "pull notes from
  Affinity".
version: 1.0.0
---

# Affinity CRM Integration

Read and write CRM data — companies, contacts, lists, notes, and opportunities — using the Affinity MCP.

## Prerequisites

The Affinity MCP must be connected. If Affinity tools are not available, instruct the user:

> **Connect Affinity CRM:**
>
> ```bash
> claude mcp add affinity --transport stdio --command "uvx affinity-mcp --api-key YOUR_AFFINITY_API_KEY"
> ```
>
> Get your API key from Affinity → Settings → API Keys.

Before making any Affinity MCP calls, use `tool_search` to load the correct tool definitions and parameter schemas.

## Capabilities

### Read Operations

| Operation | Description |
|-----------|-------------|
| Get lists | Retrieve all CRM lists (deal pipeline, accounts, contacts) |
| Get list entries | Pull entries from a specific list with field values |
| Get persons | Look up contact records |
| Get organizations | Look up company records |
| Get opportunities | Retrieve deal/opportunity data |
| Get notes | Pull interaction notes for a company or contact |
| Get field values | Read custom field values on any entity |

### Write Operations

| Operation | Description |
|-----------|-------------|
| Add entity to list | Add a person or organization to a CRM list |
| Remove list entry | Remove an entry from a list |
| Update field values | Modify custom fields on list entries |
| Create person | Add a new contact to Affinity |
| Create organization | Add a new company to Affinity |

## Workflow

### Look up CRM data

When the user asks about a company or contact in Affinity:

1. Search for the entity by name or domain
2. Pull associated list entries and field values
3. Retrieve any notes or interaction history
4. Present a CRM summary with deal stage, last touch, and key fields

### Add to pipeline

When the user wants to add a company or contact to a CRM list:

1. Find or create the person/organization record
2. Identify the target list (ask user if ambiguous)
3. Add the entity to the list
4. Set initial field values if provided

### Update records

When the user wants to update CRM data:

1. Find the existing list entry
2. Update the specified field values
3. Confirm the changes

## Combining with Other Tools

Affinity pairs naturally with the rest of the GTM stack:

- **After Octave company search** → Add qualified companies to Affinity pipeline
- **After Signaliz email verification** → Update contact records with verified emails
- **After Instantly campaign launch** → Track campaign contacts in Affinity lists
- **After Apollo enrichment** → Push enriched data to Affinity fields

## Output

Present CRM data clearly with:
- Entity name, type, and key identifiers
- List membership and deal stage
- Custom field values in a table
- Recent notes or interactions
- Offer next actions: update fields, add to list, find emails, enrich
