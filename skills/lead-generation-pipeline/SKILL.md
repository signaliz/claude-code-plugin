---
name: lead-generation-pipeline
description: >
  End-to-end lead generation pipeline: find leads with Octave, discover contacts
  with Lusha/Apollo, verify emails with Signaliz, research with Crunchbase,
  push to Instantly/Outreach campaigns, and track in Affinity CRM. Trigger when
  the user asks to: run a lead gen pipeline, build and launch an outbound
  campaign, find leads and add them to a campaign, prospect and outreach, full
  pipeline, generate leads for a campaign, or end-to-end outbound. Also trigger
  on phrases like "find leads and email them", "build a campaign for [ICP]",
  "outbound pipeline", "prospect into [company/industry]", or "find, verify,
  and campaign".
version: 2.0.0
---

# Lead Generation Pipeline

End-to-end outbound pipeline leveraging all connected platforms:

**Find companies (Octave)** → **Research funding (Crunchbase)** → **Find contacts (Octave/Lusha/Apollo)** → **Verify emails (Signaliz)** → **Push to campaign (Instantly/Outreach)** → **Track in CRM (Affinity)**

## Prerequisites

Core MCPs must be connected (additional platforms enhance the pipeline):

1. **Octave MCP** — for finding companies and people (core)
2. **Signaliz MCP** — for finding and verifying emails (core)
3. **Instantly MCP** — for campaign management and lead delivery (core)
4. **Lusha MCP** — for contact discovery with phone numbers (optional, enhances contact finding)
5. **Apollo MCP** — for search and enrichment (optional, enhances contact finding)
6. **Crunchbase MCP** — for funding and market intelligence (optional, enhances research)
7. **Outreach MCP** — for multi-channel sequences (optional, alternative to Instantly)
8. **Affinity MCP** — for CRM pipeline tracking (optional, enhances tracking)

If any core tools are missing, guide the user through setup:

> **Quick setup — run these commands:**
>
> ```bash
> # Core — Octave (company & people intelligence)
> claude mcp add octave --transport http --url "https://mcp.octave.run/v1?api_key=YOUR_OCTAVE_KEY"
>
> # Core — Signaliz (email finding & verification)
> claude mcp add signaliz --transport http --url "https://api.signaliz.com/functions/v1/signaliz-mcp?api_key=YOUR_SIGNALIZ_KEY"
>
> # Core — Instantly (email campaigns)
> claude mcp add instantly --transport http --url "https://mcp.instantly.ai/mcp?api_key=YOUR_INSTANTLY_KEY"
> ```
>
> Run `/signaliz:setup` for full platform configuration.

Before making MCP calls, use `tool_search` to load tool definitions for each service.

## Pipeline Steps

### Step 1: Define Target Criteria

Ask the user for their Ideal Customer Profile (ICP):
- **Company criteria**: industry, size, location, tech stack, funding stage
- **Contact criteria**: titles/roles, seniority, department
- **Volume**: how many leads they want

Example prompt:
```
Build an outbound campaign targeting VP of Sales at Series B+ SaaS companies with 50-500 employees
```

### Step 2: Find Companies (Octave)

Use Octave to build the account list:

1. **`find_company`** — search by criteria (industry, size, etc.)
2. **`find_similar_companies`** — expand from seed accounts
3. **`enrich_company`** — get firmographic details
4. **`qualify_company`** — score against ICP

Collect company domains for the next step.

### Step 3: Find People (Octave)

Use Octave to find contacts at the target companies:

1. **`find_person`** — search by title/role at each company
2. **`find_similar_people`** — find similar decision makers
3. **`enrich_person`** — get detailed contact profiles
4. **`qualify_person`** — score against buyer persona

Collect first_name, last_name, and company_domain for each contact.

### Step 4: Find & Verify Emails (Signaliz)

Use Signaliz to find and verify professional emails:

**For 1–25 contacts** — use `execute_primitive`:
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

**For 26–5,000 contacts** — use `find_and_verify_emails`:
- Returns a `job_id`
- Poll with `check_job_status` (respect `next_poll_after_seconds`)
- Use `page_size: 500` for results

**Filter results:**
- Keep contacts with `email_verified: true`
- Flag catch-all emails (`email_is_catch_all: true`) — include but note the risk
- Drop contacts with no email found

### Step 5: Push to Instantly Campaign

#### 5a. Select or Create Campaign

**List existing campaigns:**
Use `list_campaigns` to show available campaigns.

**Or create a new campaign:**
Use `create_campaign` with:
- `name` — campaign name
- Campaign settings as needed

#### 5b. Add Leads to Campaign

Use `add_leads_to_campaign_or_list_bulk` to push verified leads:

```json
{
  "campaign_id": "campaign-uuid",
  "leads": [
    {
      "email": "jane@acme.com",
      "first_name": "Jane",
      "last_name": "Smith",
      "company_name": "Acme Corp",
      "personalization": "Noticed Acme recently raised a Series B..."
    }
  ]
}
```

Include all enrichment data as custom variables for email personalization.

#### 5c. Activate Campaign (with confirmation)

**Always ask the user before activating.** Use `activate_campaign` only after explicit approval.

## Progress Reporting

After each step, report progress:

```
Pipeline Progress:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Step 1: ✅ ICP defined — VP Sales at Series B+ SaaS (50-500 employees)
Step 2: ✅ 47 companies found via Octave
Step 3: ✅ 52 contacts found at target companies
Step 4: ✅ 41 verified emails (5 catch-all, 6 not found)
Step 5: ⏳ Ready to push to Instantly...
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## Error Handling

| Step | Problem | Fix |
|------|---------|-----|
| 2–3 | Octave tools fail | Check Octave MCP connection and API key |
| 4 | Signaliz tools fail | Reconnect Signaliz MCP (cold start — wait 5s) |
| 4 | Low email hit rate | Try adding LinkedIn URLs from Octave enrichment |
| 5 | Instantly tools fail | Check Instantly MCP connection and API key |
| 5 | Campaign not found | List campaigns and let user select |

## Output

Present a final summary:

- **Companies found**: X accounts matching ICP
- **Contacts found**: Y decision makers identified
- **Emails verified**: Z valid emails (A catch-all, B not found)
- **Leads pushed**: Z leads added to campaign "[Campaign Name]"
- **Campaign status**: Draft / Active (pending user activation)

Offer to:
- Export the full lead list as CSV
- Show detailed results for any step
- Activate the campaign (with user confirmation)
- Run the pipeline again with different criteria

## Enhanced Pipeline Steps (When Additional Platforms Connected)

### Step 2b: Research Funding (Crunchbase)

If Crunchbase MCP is connected, enrich the company list with funding data:

1. Pull funding rounds for target companies
2. Filter by funding stage if specified in ICP
3. Add investor and funding context for personalization

### Step 3b: Discover Contacts (Lusha / Apollo)

If Lusha or Apollo MCPs are connected, supplement Octave's people search:

1. **Lusha `prospecting`** — discover additional contacts with phone numbers
2. **Apollo search** — cross-reference and fill gaps
3. Merge results, deduplicate by email/name+company

### Step 5b: Multi-Channel Sequences (Outreach)

If Outreach MCP is connected, offer as an alternative or complement to Instantly:

1. Create or select an Outreach sequence
2. Enroll prospects with personalization
3. Use Outreach for enterprise multi-touch, Instantly for high-volume cold email

### Step 6: Track in CRM (Affinity)

If Affinity MCP is connected, push pipeline data to CRM:

1. Add companies to an Affinity list (or create one)
2. Add contacts as linked entities
3. Set field values (campaign name, source, date, ICP score)
4. Pipeline tracking from prospecting through close
