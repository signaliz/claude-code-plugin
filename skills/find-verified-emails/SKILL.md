---
name: find-verified-emails
description: >
  Find verified professional email addresses for contacts using Signaliz.
  Trigger when the user asks to: find emails, look up email addresses, get
  verified work emails, find contact emails, email lookup, or email discovery.
  Also trigger on phrases like "find their email", "get me emails for",
  "email finder", or "what's their email address". Requires contact name
  and company domain.
version: 1.0.0
---

# Find Verified Emails with Signaliz

Find verified professional email addresses given a person's name and company domain.

## Prerequisites

The Signaliz MCP must be connected. If tools like `execute_primitive` are not available, instruct the user:

> **Connect Signaliz:** Run `/signaliz:setup` or add your API key to the Signaliz MCP configuration. Get a free API key at [signaliz.com/signup](https://signaliz.com/signup).

Before making any Signaliz MCP calls, use `tool_search` to load the correct tool definitions and parameter schemas. Do not guess parameter names.

## Input Requirements

Each record needs:
- `first_name` + `last_name` (or `full_name`)
- `company_domain` (e.g., "stripe.com")

Optional fields that improve accuracy:
- `company_name`
- `linkedin_url`

## Workflow

### For 1–25 contacts: Use `execute_primitive`

```json
{
  "capability_id": "find_verified_emails",
  "input_data": [
    {
      "first_name": "Jane",
      "last_name": "Smith",
      "company_domain": "acme.com"
    }
  ]
}
```

### For 26–5,000 contacts: Use `find_and_verify_emails`

This returns a `job_id`. Poll with `check_job_status`:
- Use `page_size: 500` for completed jobs
- Respect `next_poll_after_seconds`
- Increment `_poll_count` on each poll

## Understanding Results

Each result contains:
- `email` — the found email address
- `email_verified` — boolean, true if verified deliverable
- `email_is_catch_all` — boolean, true if domain accepts all addresses
- `email_esp` — email service provider (e.g., "Google Workspace")
- `verified_for_sending` — boolean, safe to send outbound

### Status Classification

| Result | Meaning | Action |
|--------|---------|--------|
| `email_verified: true`, `email_is_catch_all: false` | **Valid** | Safe for outbound |
| `email_verified: true`, `email_is_catch_all: true` | **Catch-all** | Deliverable but monitor bounces |
| No email found | **Not found** | Try with LinkedIn URL or skip |

## If the User Provides a CSV

Parse it, extract name and domain columns, normalize domains (strip `https://`, `www.`, trailing slashes), and batch in groups of 25.

## Error Handling

| Problem | Fix |
|---------|-----|
| All Signaliz tools fail | Disconnect and reconnect the Signaliz MCP (Supabase cold start) |
| Timeout on execute_primitive | Reduce batch to 10–15 records |
| No email found | Suggest providing LinkedIn URL for better accuracy |

## Output

Present results clearly with:
- ✅ Count of verified emails
- ⚠️ Count of catch-all emails
- ❌ Count of not-found
- Offer to export as CSV if more than 5 results
