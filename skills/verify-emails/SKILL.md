---
name: verify-emails
description: >
  Verify deliverability of email addresses using Signaliz. Trigger when the
  user asks to: verify emails, check deliverability, validate emails, check
  if emails are valid, audit email list quality, or test email addresses.
  Also trigger on phrases like "are these emails valid", "check these emails",
  "verify this list", "deliverability check", or "which of these bounce".
version: 1.0.0
---

# Verify Email Deliverability with Signaliz

Check whether email addresses are deliverable, risky, or invalid before sending.

## Prerequisites

The Signaliz MCP must be connected. If tools like `execute_primitive` are not available, instruct the user:

> **Connect Signaliz:** Run `/signaliz:setup` or add your API key to the Signaliz MCP configuration. Get a free API key at [signaliz.com/signup](https://signaliz.com/signup).

Before making any Signaliz MCP calls, use `tool_search` to load the correct tool definitions and parameter schemas.

## Input Requirements

Each record needs:
- `email` — the email address to verify

## Workflow

### For 1–25 emails: Use `execute_primitive`

```json
{
  "capability_id": "verify_emails",
  "input_data": [
    {"email": "jane@acme.com"},
    {"email": "bob@bigco.com"}
  ]
}
```

### For 26–5,000 emails: Use `verify_emails`

```json
{
  "emails": [
    {"email": "jane@acme.com"},
    {"email": "bob@bigco.com"}
  ]
}
```

This returns a `job_id`. Poll with `check_job_status`:
- Use `page_size: 500` for completed jobs
- Respect `next_poll_after_seconds`
- Increment `_poll_count` on each poll

### For a single email: Use `verify_email`

Only use this for exactly one email. Never loop it — use the batch tools above.

## Understanding Results

| Status | Meaning | Action |
|--------|---------|--------|
| `email_verified: true`, `email_is_catch_all: false` | **Deliverable** | Safe for outbound |
| `email_verified: true`, `email_is_catch_all: true` | **Catch-all** | Deliverable but risky at volume |
| `email_verified: false` | **Undeliverable** | Do not send — will bounce |
| Catch-all + `is_deliverable: false` | **Undeliverable catch-all** | Do not send |

Key fields in results:
- `email_verified` — boolean deliverability
- `email_is_catch_all` — boolean, domain accepts all addresses
- `email_esp` — email service provider
- `verified_for_sending` — final safe-to-send determination
- `verification_details` — detailed verification steps

## If the User Provides a CSV

Parse it, extract the `email` column, deduplicate, and batch in groups of 25.

## Error Handling

| Problem | Fix |
|---------|-----|
| All tools fail simultaneously | Reconnect the Signaliz MCP |
| VERIFICATION_TIMEOUT | Wait `retry_after` seconds and retry |
| Timeout on large batch | Reduce batch size |

## Output

Present results with:
- ✅ Count of deliverable
- ⚠️ Count of catch-all / risky
- ❌ Count of undeliverable
- Summary: "X of Y emails are safe for outbound"
- Offer to export as CSV
- Recommend: "Pull catch-all emails if bounce rate exceeds 3%"
