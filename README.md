# Signaliz Plugin for Claude Code

Lead generation and GTM data quality — find companies and people, discover verified emails, and push leads to outbound campaigns — directly from Claude Code.

Powered by three integrated platforms:
- **[Signaliz](https://signaliz.com)** — B2B email finding, verification, and company signal enrichment
- **[Octave](https://octave.run)** — Company and people intelligence, ICP matching, and lead discovery
- **[Instantly](https://instantly.ai)** — Email campaign management and outbound automation

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

### 2. Get Your API Keys

| Platform | Sign Up | Key Format |
|----------|---------|------------|
| Signaliz | [signaliz.com/signup](https://signaliz.com/signup) → Settings → API Keys | Starts with `sk_` |
| Octave | [octave.run](https://octave.run) → Settings → API Keys | See Octave dashboard |
| Instantly | [instantly.ai](https://instantly.ai) → Settings → API Keys | See Instantly dashboard |

### 3. Connect the MCPs

Run these commands in your terminal to connect all three services:

```bash
# Signaliz — email finding, verification, and company signals
claude mcp add signaliz --transport http --url "https://api.signaliz.com/functions/v1/signaliz-mcp?api_key=YOUR_SIGNALIZ_KEY"

# Octave — company and people intelligence
claude mcp add octave --transport http --url "https://mcp.octave.run/v1?api_key=YOUR_OCTAVE_KEY"

# Instantly — email campaign management
claude mcp add instantly --transport http --url "https://mcp.instantly.ai/mcp?api_key=YOUR_INSTANTLY_KEY"
```

Or run `/signaliz:setup` inside Claude Code for guided setup.

### 4. Verify It Works

Inside Claude Code, try:
```
Find companies similar to Stripe, then find the VP of Sales at each one, verify their emails, and add them to my outbound campaign
```

---

## What You Can Do

### Find Companies (Octave)
Search for target accounts by name, industry, size, or similarity to existing customers.

```
Find B2B SaaS companies with 50-500 employees that use Salesforce
```

- Company search by name or domain
- Find similar companies from a seed account
- Firmographic enrichment (industry, size, location, funding)
- ICP qualification scoring

### Find People (Octave)
Discover contacts and decision makers at target companies.

```
Find the Head of Sales at Notion, Figma, and Linear
```

- Search by name, title, role, or company
- Find similar people from a seed contact
- Contact enrichment (title, seniority, LinkedIn)
- Buyer persona qualification

### Find Verified Emails (Signaliz)
Give a name and company domain, get back a verified professional email.

```
Find emails for Jane Smith at acme.com and Bob Lee at figma.com
```

- Finds the most likely email pattern
- Verifies deliverability in real-time
- Detects catch-all domains
- Returns email service provider (Google Workspace, Microsoft 365, etc.)

### Verify Emails (Signaliz)
Check if existing email addresses will bounce before you send.

```
Verify these emails: alice@acme.com, bob@bigco.com, carol@startup.io
```

- Deliverable / catch-all / undeliverable classification
- Catch-all deep verification
- ESP detection
- Safe-to-send determination

### Company Signals (Signaliz)
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

### Lead Generation Pipeline (Full Flow)
Run the complete pipeline: find leads → verify emails → push to campaign.

```
Build an outbound campaign targeting VP of Engineering at Series B+ fintech companies
```

1. **Find companies** matching ICP criteria (Octave)
2. **Find people** at those companies by role (Octave)
3. **Find & verify emails** for each contact (Signaliz)
4. **Push leads** to an Instantly campaign with enrichment data for personalization

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
- **execute_primitive** (Signaliz): Up to 25 records per call (instant results)
- **find_and_verify_emails / verify_emails / enrich_company_signals** (Signaliz): Up to 5,000 records per job (async with polling)
- **add_leads_to_campaign_or_list_bulk** (Instantly): Bulk lead upload to campaigns

---

## MCP Installation Details

### Signaliz MCP

Signaliz provides email finding, email verification, and company signal enrichment.

**Install:**
```bash
claude mcp add signaliz --transport http --url "https://api.signaliz.com/functions/v1/signaliz-mcp?api_key=YOUR_KEY"
```

**Get your API key:**
1. Go to [signaliz.com/signup](https://signaliz.com/signup) and create a free account
2. Navigate to **Settings → API Keys**
3. Click **Create API Key** — copy the key (starts with `sk_`)

**Available tools:**
- `execute_primitive` — batch find/verify emails, enrich companies (up to 25 records)
- `find_and_verify_emails` — async bulk email finding (up to 5,000 records)
- `verify_emails` — async bulk email verification
- `verify_email` — single email verification
- `enrich_company_signals` — async bulk company enrichment
- `find_emails_with_verification` — find and verify emails for contacts
- `check_job_status` — poll async job progress and results

### Octave MCP

Octave provides company and people intelligence — search, enrichment, qualification, and similarity matching.

**Install:**
```bash
claude mcp add octave --transport http --url "https://mcp.octave.run/v1?api_key=YOUR_KEY"
```

**Get your API key:**
1. Go to [octave.run](https://octave.run) and create an account
2. Navigate to **Settings → API Keys**
3. Create and copy your API key

**Available tools:**
- `find_company` — search for companies by name or domain
- `find_similar_companies` — find companies similar to a seed
- `enrich_company` — deep company enrichment (firmographics, tech stack, funding)
- `qualify_company` — score company against ICP criteria
- `find_person` — search for people by name, title, or company
- `find_similar_people` — find people similar to a seed contact
- `enrich_person` — deep contact enrichment
- `qualify_person` — score contact against buyer persona
- `generate_email` — generate personalized email copy
- `generate_content` — generate sales content

### Instantly MCP

Instantly provides email campaign management — create campaigns, add leads, and manage outbound.

**Install:**
```bash
claude mcp add instantly --transport http --url "https://mcp.instantly.ai/mcp?api_key=YOUR_KEY"
```

**Get your API key:**
1. Go to [instantly.ai](https://instantly.ai) and create an account
2. Navigate to **Settings → API Keys** (or **Integrations → API**)
3. Create and copy your API key

**Available tools:**
- `list_campaigns` — list all campaigns
- `get_campaign` — get campaign details
- `create_campaign` — create a new campaign
- `activate_campaign` — start a campaign
- `pause_campaign` — pause a campaign
- `add_leads_to_campaign_or_list_bulk` — add leads to a campaign in bulk
- `create_lead` — add a single lead
- `list_leads` — list leads in a campaign
- `list_accounts` — list sending accounts
- `get_campaign_analytics` — campaign performance metrics

---

## Skills

The plugin includes six auto-triggered skills that activate when Claude detects relevant requests:

| Skill | Triggers On | Platform |
|-------|-------------|----------|
| **find-companies-octave** | "find companies", "target accounts", "companies like [X]", "ICP matching" | Octave |
| **find-people-octave** | "find people", "find contacts", "decision makers", "prospect list" | Octave |
| **find-verified-emails** | "find email", "email lookup", "get their email" | Signaliz |
| **verify-emails** | "verify emails", "check deliverability", "validate" | Signaliz |
| **company-signals** | "company research", "signals", "hiring", "funding" | Signaliz |
| **lead-generation-pipeline** | "lead gen pipeline", "outbound campaign", "find and email", "full pipeline" | All three |

---

## Pricing

### Signaliz
New accounts receive **100 free credits**.

| Capability | Credits |
|-----------|---------|
| Find verified email | 1 credit |
| Verify email | 0.5 credits |
| Company signals | 2 credits |

See [signaliz.com/pricing](https://signaliz.com/pricing) for paid plans.

### Octave
See [octave.run](https://octave.run) for pricing details.

### Instantly
See [instantly.ai/pricing](https://instantly.ai/pricing) for pricing details.

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| All Signaliz tools fail at once | Supabase cold start — wait 5 seconds and retry, or reconnect the MCP |
| All Octave tools fail | Check Octave MCP connection and API key |
| All Instantly tools fail | Check Instantly MCP connection and API key |
| `execute_primitive` times out | Reduce batch size to 10–15 records |
| `check_job_status` returns partial results | Paginate — use `page` parameter with `page_size: 500` |
| "MCP not connected" errors | Run setup commands above or reconnect the affected MCP |
| Low email hit rate | Add LinkedIn URLs from Octave enrichment for better accuracy |
| Campaign not found in Instantly | Use `list_campaigns` to see available campaigns |

---

## Security & Privacy

- API keys are stored in your local Claude Code MCP configuration — never transmitted to Anthropic
- Signaliz does not store the contacts you look up — queries are processed and discarded
- All API traffic is encrypted over HTTPS
- Workspace-level blocklists and rate limits are enforced server-side

---

## Support

- **Signaliz:** [signaliz.com](https://signaliz.com) | support@signaliz.com | [docs.signaliz.com](https://docs.signaliz.com)
- **Octave:** [octave.run](https://octave.run)
- **Instantly:** [instantly.ai](https://instantly.ai)

---

## License

MIT — see [LICENSE](./LICENSE) for details.
