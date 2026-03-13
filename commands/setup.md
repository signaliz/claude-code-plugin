---
description: Set up platform connections — configure API keys and connect MCPs for all supported platforms
allowed-tools: [Signaliz:company_search, Signaliz:verify_email]
---

# Full Platform Setup

Walk the user through connecting all available platforms to Claude Code.

## Overview

Present the user with the full list of available platforms and their connection status:

> **Available Platforms:**
>
> | # | Platform | What It Does |
> |---|----------|-------------|
> | 1 | **Signaliz** | Email finding, verification, company signals |
> | 2 | **Octave** | Company & people intelligence |
> | 3 | **Instantly** | Email campaign management (multi-workspace) |
> | 4 | **Affinity** | CRM — pipeline, contacts, notes |
> | 5 | **Lusha** | Contact discovery — 100M+ contacts with phones |
> | 6 | **Apollo.io** | Search & enrichment — contacts, companies, sequences |
> | 7 | **Outreach.io** | Sales engagement — multi-channel sequences |
> | 8 | **Crunchbase** | Market intelligence — funding, investors, M&A |
> | 9 | **Manus AI** | AI agent delegation — complex research tasks |
> | 10 | **SourceScrub** | Private company data (custom MCP pending) |
> | 11 | **AlphaSense** | Enterprise research intelligence (custom MCP pending) |

Ask the user which platforms they want to set up. They can say "all" or list specific ones.

## Platform Setup Instructions

### 1. Signaliz

```bash
claude mcp add signaliz --transport http --url "https://api.signaliz.com/functions/v1/signaliz-mcp?api_key=YOUR_SIGNALIZ_KEY"
```

**Get your key:** [signaliz.com/signup](https://signaliz.com/signup) → Settings → API Keys (starts with `sk_`)

### 2. Octave

```bash
claude mcp add octave --transport http --url "https://mcp.octave.run/v1?api_key=YOUR_OCTAVE_KEY"
```

**Get your key:** [octave.run](https://octave.run) → Settings → API Keys

### 3. Instantly

```bash
# Single workspace
claude mcp add instantly --transport http --url "https://mcp.instantly.ai/mcp?api_key=YOUR_INSTANTLY_KEY"

# Additional workspaces — add each with a unique name
claude mcp add instantly-ws2 --transport http --url "https://mcp.instantly.ai/mcp?api_key=YOUR_SECOND_KEY"
claude mcp add instantly-ws3 --transport http --url "https://mcp.instantly.ai/mcp?api_key=YOUR_THIRD_KEY"
```

**Get your key:** [instantly.ai](https://instantly.ai) → Settings → Integrations → API

### 4. Affinity CRM

```bash
claude mcp add affinity --transport stdio --command "uvx affinity-mcp --api-key YOUR_AFFINITY_KEY"
```

**Get your key:** Affinity → Settings → API Keys

### 5. Lusha

```bash
claude mcp add lusha --transport stdio --command "npx @lusha-org/mcp@latest" --env LUSHA_API_KEY=YOUR_LUSHA_KEY
```

**Get your key:** Lusha → Settings → API

### 6. Apollo.io

```bash
claude mcp add apollo --transport stdio --command "npx @thevgergroup/apollo-io-mcp" --env APOLLO_API_KEY=YOUR_APOLLO_KEY
```

**Get your key:** Apollo → Settings → Integrations → API

### 7. Outreach.io

```bash
claude mcp add outreach --transport stdio --command "npx mcp-remote https://api.outreach.io/mcp/"
```

**Requires:** Outreach Amplify license with active credits.

### 8. Crunchbase

```bash
claude mcp add crunchbase --transport stdio --command "npx @cyreslab/crunchbase-mcp-server" --env CRUNCHBASE_API_KEY=YOUR_CRUNCHBASE_KEY
```

**Get your key:** Crunchbase → Settings → API

### 9. Manus AI

```bash
claude mcp add manus --transport stdio --command "npx manus-mcp" --env MANUS_MCP_API_KEY=YOUR_MANUS_KEY
```

**Get your key:** [open.manus.ai](https://open.manus.ai) → API Keys

### 10. SourceScrub

> Custom MCP build pending. Contact SourceScrub for API access.
> Interim: SourceScrub data flows through Affinity's native integration.

### 11. AlphaSense

> Custom MCP build pending. Requires Enterprise license.
> Contact: apisupport@alpha-sense.com
> Interim: Use Manus AI for deep research, Crunchbase for financial data.

## Verification

After connecting platforms, verify each one by calling a basic tool. Report status:

```
Platform Status:
━━━━━━━━━━━━━━━━━━━━━━━━━━━
Signaliz      ✅ Connected
Octave        ✅ Connected
Instantly     ✅ Connected
Affinity      ✅ Connected
Lusha         ✅ Connected
Apollo        ✅ Connected
Outreach      ⚠️ Needs Amplify license
Crunchbase    ✅ Connected
Manus         ✅ Connected
SourceScrub   ⏳ Custom MCP pending
AlphaSense    ⏳ Custom MCP pending
━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## If Connection Fails

- **Signaliz fails:** Confirm key starts with `sk_`. Cold starts may cause first request to fail — retry after 5s.
- **Any MCP fails:** Check API key, verify the command runs standalone, restart Claude Code.
- **Outreach fails:** Verify Amplify license is active.
- **Affinity fails:** Ensure `uvx` is installed (`pip install uvx` if needed).
