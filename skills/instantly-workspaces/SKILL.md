---
name: instantly-workspaces
description: >
  Manage campaigns across multiple Instantly workspaces. Trigger when the user
  asks to: switch Instantly workspace, list workspaces, manage multiple Instantly
  accounts, check campaigns across workspaces, or use a specific Instantly
  account. Also trigger on phrases like "switch to [workspace] Instantly",
  "which Instantly workspace", "list my Instantly workspaces", or "campaigns
  in [workspace]".
version: 1.0.0
---

# Instantly Multi-Workspace Management

Manage campaigns, leads, and analytics across multiple Instantly workspaces from a single interface.

## Prerequisites

At least one Instantly MCP must be connected. For multi-workspace support, connect each workspace:

> **Connect Instantly workspaces:**
>
> ```bash
> # Primary workspace
> claude mcp add instantly --transport http --url "https://mcp.instantly.ai/mcp?api_key=YOUR_PRIMARY_KEY"
>
> # Additional workspaces (rename as needed)
> claude mcp add instantly-workspace2 --transport http --url "https://mcp.instantly.ai/mcp?api_key=YOUR_SECOND_KEY"
> claude mcp add instantly-workspace3 --transport http --url "https://mcp.instantly.ai/mcp?api_key=YOUR_THIRD_KEY"
> ```
>
> Get API keys from each Instantly workspace → Settings → Integrations → API.

Before making any Instantly MCP calls, use `tool_search` to load the correct tool definitions. Search for tools prefixed with `mcp__instantly__`, `mcp__instantly_workspace2__`, etc.

## Multi-Workspace Awareness

When the user has multiple Instantly MCPs connected:

1. **Identify which workspace** the user wants to operate on
2. If ambiguous, **ask the user** which workspace to use
3. Use the correct MCP prefix for tool calls
4. **Always confirm** the workspace before write operations

## Capabilities per Workspace

Each connected Instantly workspace provides:

| Category | Tools |
|----------|-------|
| Campaigns | List, create, activate, pause campaigns |
| Leads | Add leads (single or bulk), list leads |
| Accounts | List sending accounts |
| Analytics | Campaign performance metrics |
| Emails | Email activity and status |

## Workflow

### Cross-Workspace Campaign Overview

1. List campaigns from all connected workspaces
2. Present a unified view with workspace labels
3. Show key metrics per campaign

### Add Leads to Specific Workspace

1. Confirm which workspace to use
2. Identify or create the target campaign
3. Add leads with enrichment data
4. Confirm the operation and lead count

### Workspace-Specific Analytics

1. Pull analytics from the specified workspace
2. Present campaign performance (sent, opened, replied, bounced)
3. Compare across workspaces if requested

## Future: Unified Workspace Server

For organizations running 3+ Instantly workspaces, a unified MCP server with dynamic workspace switching is recommended. This is based on the `bcharleson/instantly-mcp` architecture which supports per-request API keys via the `x-instantly-api-key` header.

Architecture:
```
Single MCP Server → Workspace Registry → Dynamic API Key Routing
```

Benefits:
- Collapses N workspace connections into 1
- Reduces tool list clutter
- Supports `list_workspaces` and `switch_workspace` commands

## Output

Present results with:
- Workspace name clearly labeled on all results
- Campaign names and statuses
- Lead counts and delivery metrics
- Offer cross-workspace comparisons
