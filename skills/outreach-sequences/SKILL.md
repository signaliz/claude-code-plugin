---
name: outreach-sequences
description: >
  Manage sales sequences and prospects with Outreach.io. Trigger when the user
  asks to: create an Outreach sequence, add prospects to Outreach, manage
  Outreach accounts, check deal insights, get sequence analytics, or use
  Outreach for outbound. Also trigger on phrases like "Outreach sequence",
  "add to Outreach", "Outreach prospects", "deal insights from Outreach",
  "sequence performance", or "Outreach pipeline".
version: 1.0.0
---

# Outreach.io Sequences & Prospect Management

Manage prospects, sequences, accounts, and deal insights using the official Outreach MCP.

## Prerequisites

The Outreach MCP must be connected. **Requires Outreach Amplify license with active credits.**

If Outreach tools are not available, instruct the user:

> **Connect Outreach.io:**
>
> ```bash
> claude mcp add outreach --transport stdio --command "npx mcp-remote https://api.outreach.io/mcp/"
> ```
>
> Requirements:
> - Organization must have **Amplify** enabled with active credits
> - User must be active and licensed on an Outreach instance

Before making any Outreach MCP calls, use `tool_search` to load the correct tool definitions and parameter schemas.

## Capabilities

### Prospects

| Operation | Description |
|-----------|-------------|
| Search prospects | Find prospects by email, name, or account |
| Create prospect | Add a new prospect to Outreach |
| Update prospect | Modify prospect fields |

### Sequences

| Operation | Description |
|-----------|-------------|
| List sequences | View all available sequences |
| Add to sequence | Enroll prospects in a sequence |
| Sequence analytics | View open/reply/click rates |

### Accounts

| Operation | Description |
|-----------|-------------|
| Search accounts | Find accounts by name or domain |
| Create account | Add a new account |
| Update account | Modify account fields |

### Intelligence

| Operation | Description |
|-----------|-------------|
| Deal insights | Pipeline analytics and deal health |
| Next actions | AI-recommended engagement actions |

## Workflow

### Enroll Prospects in a Sequence

1. Find or create the prospect in Outreach
2. Identify the target sequence (list sequences if needed)
3. Enroll the prospect with personalization variables
4. Confirm enrollment

### Pipeline Review

1. Pull deal insights for active opportunities
2. Surface AI-recommended next actions
3. Present deal health and engagement metrics

## Combining with Other Tools

- **Signaliz → Outreach** — Verify emails, then enroll in Outreach sequences
- **Octave → Outreach** — Find companies and people, push to Outreach
- **Lusha → Outreach** — Discover contacts, add as Outreach prospects
- **Apollo → Outreach** — Sync Apollo contacts to Outreach sequences
- **Affinity → Outreach** — Sync CRM pipeline contacts to Outreach sequences

## vs. Instantly

Both Outreach and Instantly handle outbound sequences. Key differences:

| Aspect | Outreach | Instantly |
|--------|----------|-----------|
| Focus | Enterprise sales engagement | Email campaign automation |
| Channels | Email, phone, social, tasks | Email only |
| CRM sync | Deep Salesforce/CRM integration | Lightweight |
| Best for | SDR teams with complex workflows | High-volume cold email |

Use both: Outreach for enterprise multi-touch sequences, Instantly for high-volume cold email campaigns.

## Output

Present results with:
- Prospect name, email, and account
- Sequence enrollment status
- Deal stage and next actions
- Engagement metrics (opens, replies, clicks)
- Offer cross-platform actions: verify email (Signaliz), enrich (Apollo/Lusha)
