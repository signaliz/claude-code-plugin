# Signaliz Plugin for Claude Code

Full-stack GTM plugin — lead generation, CRM enrichment, outbound campaigns, and market intelligence — directly from Claude Code.

Powered by 11 integrated platforms:

**Core Pipeline:**
- **[Signaliz](https://signaliz.com)** — B2B email finding, verification, and company signal enrichment
- **[Octave](https://octave.run)** — Company and people intelligence, ICP matching, and lead discovery
- **[Instantly](https://instantly.ai)** — Email campaign management and outbound automation (multi-workspace)

**CRM & Enrichment:**
- **[Affinity](https://affinity.co)** — CRM — pipeline, contacts, lists, notes, and opportunities
- **[Lusha](https://lusha.com)** — Contact discovery — 100M+ contacts with direct phone numbers
- **[Apollo.io](https://apollo.io)** — Search, enrichment, and sequence management
- **[Outreach.io](https://outreach.io)** — Enterprise sales engagement — multi-channel sequences

**Market Intelligence:**
- **[Crunchbase](https://crunchbase.com)** — Funding rounds, investors, acquisitions, and market data
- **[Manus AI](https://manus.ai)** — AI agent delegation for complex research tasks
- **[SourceScrub](https://sourcescrub.com)** — Private company intelligence (custom MCP pending)
- **[AlphaSense](https://alpha-sense.com)** — Enterprise research intelligence (custom MCP pending)

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

| Platform | Sign Up | Key Location |
|----------|---------|------------|
| Signaliz | [signaliz.com/signup](https://signaliz.com/signup) | Settings → API Keys (starts with `sk_`) |
| Octave | [octave.run](https://octave.run) | Settings → API Keys |
| Instantly | [instantly.ai](https://instantly.ai) | Settings → Integrations → API |
| Affinity | [affinity.co](https://affinity.co) | Settings → API Keys |
| Lusha | [lusha.com](https://lusha.com) | Settings → API |
| Apollo | [apollo.io](https://apollo.io) | Settings → Integrations → API |
| Outreach | [outreach.io](https://outreach.io) | Requires Amplify license |
| Crunchbase | [crunchbase.com](https://crunchbase.com) | Settings → API |
| Manus AI | [open.manus.ai](https://open.manus.ai) | API Keys |

### 3. Connect the MCPs

Run these commands in your terminal to connect your platforms:

```bash
# Core — Signaliz (email finding, verification, company signals)
claude mcp add signaliz --transport http --url "https://api.signaliz.com/functions/v1/signaliz-mcp?api_key=YOUR_SIGNALIZ_KEY"

# Core — Octave (company and people intelligence)
claude mcp add octave --transport http --url "https://mcp.octave.run/v1?api_key=YOUR_OCTAVE_KEY"

# Core — Instantly (email campaign management)
claude mcp add instantly --transport http --url "https://mcp.instantly.ai/mcp?api_key=YOUR_INSTANTLY_KEY"

# CRM — Affinity
claude mcp add affinity --transport stdio --command "uvx affinity-mcp --api-key YOUR_AFFINITY_KEY"

# Enrichment — Lusha (contact discovery)
claude mcp add lusha --transport stdio --command "npx @lusha-org/mcp@latest" --env LUSHA_API_KEY=YOUR_LUSHA_KEY

# Enrichment — Apollo.io (search and enrichment)
claude mcp add apollo --transport stdio --command "npx @thevgergroup/apollo-io-mcp" --env APOLLO_API_KEY=YOUR_APOLLO_KEY

# Sequences — Outreach.io (requires Amplify license)
claude mcp add outreach --transport stdio --command "npx mcp-remote https://api.outreach.io/mcp/"

# Intelligence — Crunchbase (funding and market data)
claude mcp add crunchbase --transport stdio --command "npx @cyreslab/crunchbase-mcp-server" --env CRUNCHBASE_API_KEY=YOUR_CRUNCHBASE_KEY

# Intelligence — Manus AI (agent delegation)
claude mcp add manus --transport stdio --command "npx manus-mcp" --env MANUS_MCP_API_KEY=YOUR_MANUS_KEY
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

### Find People (Octave)
Discover contacts and decision makers at target companies.

```
Find the Head of Sales at Notion, Figma, and Linear
```

### Find Verified Emails (Signaliz)
Give a name and company domain, get back a verified professional email.

```
Find emails for Jane Smith at acme.com and Bob Lee at figma.com
```

### Verify Emails (Signaliz)
Check if existing email addresses will bounce before you send.

```
Verify these emails: alice@acme.com, bob@bigco.com, carol@startup.io
```

### Company Signals (Signaliz)
Research companies for sales intelligence — hiring, funding, tech stack, news, and more.

```
What signals do you have on stripe.com?
```

### CRM Management (Affinity)
Pull company records, contacts, notes, and manage your pipeline.

```
What's in Affinity for acme.com? Add Stripe to our Pipeline list.
```

### Contact Discovery (Lusha)
Search Lusha's 100M+ contact database for prospects with phone numbers and emails.

```
Find VP of Engineering contacts at fintech companies in NYC using Lusha
```

### Search & Enrich (Apollo)
Search and enrich contacts and companies using Apollo.io.

```
Search Apollo for the CTO at companies similar to Datadog
```

### Sales Sequences (Outreach)
Manage prospects and sequences in Outreach.io.

```
Add these contacts to the Q1 Outbound sequence in Outreach
```

### Funding Research (Crunchbase)
Research funding rounds, investors, acquisitions, and market trends.

```
Who funded Notion? Show me Series B+ fintech companies from the last 6 months.
```

### AI Research (Manus)
Delegate complex, multi-step research tasks to Manus AI agents.

```
Have Manus research the competitive landscape for AI sales tools
```

### Multi-Workspace Campaigns (Instantly)
Manage campaigns across multiple Instantly workspaces.

```
Show me campaigns across all my Instantly workspaces
```

### Lead Generation Pipeline (Full Flow)
Run the complete pipeline: find leads, verify emails, push to campaign.

```
Build an outbound campaign targeting VP of Engineering at Series B+ fintech companies
```

1. **Find companies** matching ICP criteria (Octave)
2. **Find people** at those companies by role (Octave)
3. **Discover contacts** with phone numbers (Lusha)
4. **Find & verify emails** for each contact (Signaliz)
5. **Enrich** with additional data (Apollo)
6. **Research** funding and market context (Crunchbase)
7. **Push leads** to campaign (Instantly) or sequence (Outreach)
8. **Track** in CRM pipeline (Affinity)

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
- **personBulkLookup** (Lusha): Up to 100 contacts per call

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

### Affinity MCP

Affinity provides CRM data — lists, contacts, companies, notes, and pipeline management.

**Install:**
```bash
claude mcp add affinity --transport stdio --command "uvx affinity-mcp --api-key YOUR_KEY"
```

**Capabilities:** Read/write on lists, entries, field values, persons, organizations, and opportunities.

### Lusha MCP

Lusha provides contact discovery from a 100M+ contact / 60M+ company database.

**Install:**
```bash
claude mcp add lusha --transport stdio --command "npx @lusha-org/mcp@latest" --env LUSHA_API_KEY=YOUR_KEY
```

**Available tools:**
- `personBulkLookup` — look up to 100 contacts at once
- `companyBulkLookup` — bulk company enrichment
- `prospecting` — search with filters (title, seniority, location)

### Apollo MCP

Apollo provides contact and company search, enrichment, and sequence management.

**Install:**
```bash
claude mcp add apollo --transport stdio --command "npx @thevgergroup/apollo-io-mcp" --env APOLLO_API_KEY=YOUR_KEY
```

**Capabilities:** Search people/companies, enrich contacts, create/update contacts, add to sequences. 94% smaller payloads optimized for LLM context.

### Outreach MCP

Outreach provides sales engagement — prospects, sequences, accounts, and deal insights.

**Install:**
```bash
claude mcp add outreach --transport stdio --command "npx mcp-remote https://api.outreach.io/mcp/"
```

**Requirements:** Amplify license with active credits.

### Crunchbase MCP

Crunchbase provides market intelligence — companies, funding, acquisitions, and people.

**Install:**
```bash
claude mcp add crunchbase --transport stdio --command "npx @cyreslab/crunchbase-mcp-server" --env CRUNCHBASE_API_KEY=YOUR_KEY
```

**Available tools:** Search companies, get company details, funding rounds, acquisitions, search people.

### Manus MCP

Manus provides AI agent delegation for complex, multi-step research tasks.

**Install:**
```bash
claude mcp add manus --transport stdio --command "npx manus-mcp" --env MANUS_MCP_API_KEY=YOUR_KEY
```

**Capabilities:** Create AI tasks, manage webhooks, integrate Manus workflows.

---

## Skills

The plugin includes auto-triggered skills that activate when Claude detects relevant requests:

| Skill | Triggers On | Platform |
|-------|-------------|----------|
| **find-companies-octave** | "find companies", "target accounts", "companies like [X]" | Octave |
| **find-people-octave** | "find people", "find contacts", "decision makers" | Octave |
| **find-verified-emails** | "find email", "email lookup", "get their email" | Signaliz |
| **verify-emails** | "verify emails", "check deliverability" | Signaliz |
| **company-signals** | "company research", "signals", "hiring", "funding" | Signaliz |
| **affinity-crm** | "check Affinity", "CRM lookup", "add to list" | Affinity |
| **lusha-contacts** | "find contacts with Lusha", "phone numbers", "prospect" | Lusha |
| **apollo-enrich** | "search Apollo", "enrich with Apollo" | Apollo |
| **outreach-sequences** | "Outreach sequence", "add to Outreach" | Outreach |
| **crunchbase-research** | "funding rounds", "investors", "Crunchbase lookup" | Crunchbase |
| **manus-delegate** | "use Manus", "delegate research", "deep research" | Manus |
| **instantly-workspaces** | "switch workspace", "campaigns across workspaces" | Instantly |
| **sourcescrub-signals** | "SourceScrub lookup", "private company data" | SourceScrub |
| **alphasense-research** | "AlphaSense", "earnings transcripts", "analyst reports" | AlphaSense |
| **lead-generation-pipeline** | "lead gen pipeline", "outbound campaign", "full pipeline" | All |

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

### Other Platforms
- **Octave:** See [octave.run](https://octave.run)
- **Instantly:** See [instantly.ai/pricing](https://instantly.ai/pricing)
- **Affinity:** See [affinity.co](https://affinity.co)
- **Lusha:** Uses existing API credits from your Lusha plan
- **Apollo:** Available on all paid Apollo plans
- **Outreach:** Requires Amplify license with active credits
- **Crunchbase:** Free API for basic; Enterprise for full access
- **Manus AI:** See [open.manus.ai](https://open.manus.ai)

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| All Signaliz tools fail at once | Supabase cold start — wait 5 seconds and retry, or reconnect the MCP |
| All Octave tools fail | Check Octave MCP connection and API key |
| All Instantly tools fail | Check Instantly MCP connection and API key |
| Affinity tools fail | Ensure `uvx` is installed; check API key |
| Lusha tools fail | Check Lusha API key and credit balance |
| Apollo tools fail | Check Apollo API key; verify paid plan is active |
| Outreach tools fail | Verify Amplify license is active with credits |
| Crunchbase tools fail | Check API key; Enterprise key needed for full access |
| Manus tools fail | Check API key from open.manus.ai |
| `execute_primitive` times out | Reduce batch size to 10–15 records |
| `check_job_status` returns partial results | Paginate — use `page` parameter with `page_size: 500` |
| "MCP not connected" errors | Run `/signaliz:setup` or reconnect the affected MCP |
| Low email hit rate | Add LinkedIn URLs from Octave/Apollo enrichment for better accuracy |
| Campaign not found in Instantly | Use `list_campaigns` to see available campaigns |

---

## Security & Privacy

- API keys are stored in your local Claude Code MCP configuration — never transmitted to Anthropic
- Signaliz does not store the contacts you look up — queries are processed and discarded
- All API traffic is encrypted over HTTPS
- Workspace-level blocklists and rate limits are enforced server-side
- Each platform's data handling follows their respective privacy policies

---

## Support

- **Signaliz:** [signaliz.com](https://signaliz.com) | support@signaliz.com | [docs.signaliz.com](https://docs.signaliz.com)
- **Octave:** [octave.run](https://octave.run)
- **Instantly:** [instantly.ai](https://instantly.ai)
- **Affinity:** [affinity.co](https://affinity.co)
- **Lusha:** [lusha.com](https://lusha.com)
- **Apollo:** [apollo.io](https://apollo.io)
- **Outreach:** [outreach.io](https://outreach.io)
- **Crunchbase:** [crunchbase.com](https://crunchbase.com)
- **Manus AI:** [manus.ai](https://manus.ai)

---

## License

MIT — see [LICENSE](./LICENSE) for details.
