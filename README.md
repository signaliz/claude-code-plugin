# Signaliz Plugin for Claude Code

GTM data quality and enrichment — find verified emails, check deliverability, and discover company signals — directly from Claude Code.

**[Signaliz](https://signaliz.com)** is a B2B data quality and enrichment platform that verifies emails, finds professional contacts, and enriches companies with business signals like hiring, funding, and tech stack data.

---

## Quick Start

### 1. Install the Plugin

```bash
claude plugin install signaliz@claude-plugin-directory
```

Or test locally during development:
```bash
claude --plugin-dir ./signaliz-plugin
```

### 2. Get Your API Key

1. Go to [signaliz.com/signup](https://signaliz.com/signup) and create a free account
2. Navigate to **Settings → API Keys**
3. Click **Create API Key** — copy the key (starts with `sk_`)

### 3. Connect the MCP

```bash
claude mcp add signaliz --transport http --url "https://api.signaliz.com/functions/v1/signaliz-mcp?api_key=YOUR_KEY_HERE"
```

Or run `/signaliz:setup` inside Claude Code for guided setup.

### 4. Verify It Works

Inside Claude Code, try:
```
Find the verified email for Dario Amodei at anthropic.com
```

---

## What You Can Do

### Find Verified Emails
Give a name and company domain, get back a verified professional email.

```
Find emails for Jane Smith at acme.com and Bob Lee at figma.com
```

- Finds the most likely email pattern
- Verifies deliverability in real-time
- Detects catch-all domains
- Returns email service provider (Google Workspace, Microsoft 365, etc.)

### Verify Emails
Check if existing email addresses will bounce before you send.

```
Verify these emails: alice@acme.com, bob@bigco.com, carol@startup.io
```

- Deliverable / catch-all / undeliverable classification
- Catch-all deep verification
- ESP detection
- Safe-to-send determination

### Company Signals
Research companies for sales intelligence — hiring, funding, tech stack, news, and more.

```
What signals do you have on stripe.com?
```

- Hiring activity and open roles
- Recent funding rounds and investors
- Tech stack and tools in use
- News mentions and product launches
- Leadership changes
- M&A activity

---

## Batch Processing

### From CSV
Upload a CSV file and process it in bulk:

```
Here's a CSV of 200 contacts — find verified emails for all of them
```

### From Natural Language
Describe your targets and let Claude build the list:

```
Find verified emails for the VP of Sales at the top 20 B2B SaaS companies
```

### Batch Limits
- **execute_primitive**: Up to 25 records per call (instant results)
- **find_and_verify_emails / verify_emails / enrich_company_signals**: Up to 5,000 records per job (async with polling)

---

## Commands

| Command | Description |
|---------|-------------|
| `/signaliz:setup` | Guided setup — get API key, connect MCP, verify |
| `/signaliz:run find <name> at <domain>` | Quick email finder |
| `/signaliz:run verify <emails...>` | Quick email verification |
| `/signaliz:run signals <domains...>` | Quick company enrichment |

---

## Skills

The plugin includes three auto-triggered skills that activate when Claude detects relevant requests:

- **find-verified-emails** — triggers on "find email", "email lookup", "get their email"
- **verify-emails** — triggers on "verify emails", "check deliverability", "validate"
- **company-signals** — triggers on "company research", "signals", "hiring", "funding"

---

## Pricing

Signaliz offers a free tier to get started:

| Capability | Credits |
|-----------|---------|
| Find verified email | 1 credit |
| Verify email | 0.5 credits |
| Company signals | 2 credits |

New accounts receive **100 free credits**. See [signaliz.com/pricing](https://signaliz.com/pricing) for paid plans.

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| All Signaliz tools fail at once | Supabase cold start — wait 5 seconds and retry, or reconnect the MCP |
| `execute_primitive` times out | Reduce batch size to 10–15 records |
| `check_job_status` returns partial results | Paginate — use `page` parameter with `page_size: 500` |
| "MCP not connected" errors | Run `/signaliz:setup` or re-add the MCP with your API key |
| API key rejected | Verify key starts with `sk_` and is active at signaliz.com/settings |

---

## Security & Privacy

- API keys are stored in your local Claude Code MCP configuration — never transmitted to Anthropic
- Signaliz does not store the contacts you look up — queries are processed and discarded
- All API traffic is encrypted over HTTPS
- Workspace-level blocklists and rate limits are enforced server-side

---

## Support

- **Website:** [signaliz.com](https://signaliz.com)
- **Email:** support@signaliz.com
- **Documentation:** [docs.signaliz.com](https://docs.signaliz.com)

---

## License

MIT — see [LICENSE](./LICENSE) for details.
