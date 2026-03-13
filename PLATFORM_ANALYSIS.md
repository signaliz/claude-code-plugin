# Detailed Platform Analysis — MCP Integration Roadmap

**Date:** 2026-03-13
**Context:** Signaliz Claude Code Plugin ecosystem expansion

This document analyzes 9 platforms for MCP (Model Context Protocol) integration with the Signaliz plugin ecosystem. Each platform is assessed for current MCP availability, recommended integration path, and effort required.

---

## Table of Contents

1. [Affinity CRM](#1-affinity-crm)
2. [Instantly](#2-instantly)
3. [Manus AI](#3-manus-ai)
4. [SourceScrub](#4-sourcescrub)
5. [Outreach.io](#5-outreachio)
6. [Lusha](#6-lusha)
7. [Crunchbase Pro](#7-crunchbase-pro)
8. [Apollo.io](#8-apolloio)
9. [AlphaSense](#9-alphasense)
10. [Summary Matrix](#summary-matrix)
11. [Implementation Roadmap](#implementation-roadmap)

---

## 1. Affinity CRM

### Current State

- **Official MCP:** "Coming Soon" on Affinity's product roadmap. Will allow connecting Affinity MCP directly into an LLM to pull company records, contacts, and notes.
- **Community MCP:** [yaniv-golan/affinity-sdk](https://github.com/yaniv-golan/affinity-sdk) — High-quality Python MCP server (MIT license). Supports lists, entries, field values, persons, organizations, and opportunities. Available via `uvx` with no install needed. Includes Claude Code plugin support.
- **Third-party wrappers:** Composio, Pipedream, Zapier, n8n, and Gumloop all offer Affinity MCP wrappers.

### Recommendation

**Use `yaniv-golan/affinity-sdk` MCP immediately.**

It supports read operations comprehensively and includes write capabilities (add/remove list entries, update field values). For use cases requiring limited write capabilities, this is ideal. Switch to Affinity's official MCP when it launches for tighter support.

### Write Capabilities Available

| Operation | Supported |
|-----------|-----------|
| Add entities to lists | Yes |
| Remove list entries | Yes |
| Update field values | Yes |
| Full CRUD on persons | Yes (via SDK layer) |
| Full CRUD on organizations | Yes (via SDK layer) |

### Integration Path

```bash
# Install via uvx (no local install needed)
uvx affinity-mcp --api-key YOUR_AFFINITY_KEY

# Or add as Claude Code MCP
claude mcp add affinity --transport stdio --command "uvx affinity-mcp --api-key YOUR_AFFINITY_KEY"
```

### Status: Ready to use — Community MCP

---

## 2. Instantly

### Current State

- **Official Instantly MCP:** Native MCP at `mcp.instantly.ai/mcp` with 31 tools across 5 categories (campaigns, leads, accounts, emails, analytics). Free with any Instantly subscription. **Already connected** — 3 Instantly MCPs in Claude (Instantly, Airbyte MCP, Velox Instantly) with separate API keys.
- **Community MCP:** [bcharleson/instantly-mcp](https://github.com/bcharleson/instantly-mcp) — Python/FastMCP server with 38 tools across 6 categories. Key differentiator: multi-tenant support via per-request API keys in HTTP mode (`x-instantly-api-key` header). Supports lazy loading of tool categories to reduce context window.

### Multi-Workspace Challenge

The official Instantly MCP binds one API key per connection. For multi-workspace, three separate Instantly MCPs are currently running in Claude with separate API keys. This works but clutters the tool list.

### Recommendation

**Fork `bcharleson/instantly-mcp` and add a workspace registry tool.**

1. Add a `list_workspaces` tool that returns configured workspaces
2. Add a `switch_workspace` tool that dynamically swaps the API key at runtime
3. Deploy as a single HTTP MCP server on your infrastructure

This collapses 3 connections into 1 with dynamic workspace switching. The `bcharleson` server already supports per-request API keys via headers, so the plumbing is there.

### Proposed Architecture

```
┌──────────────────────────────┐
│  Unified Instantly MCP       │
│  (forked bcharleson server)  │
├──────────────────────────────┤
│  list_workspaces()           │
│  switch_workspace(name)      │
│  ─── all 38 existing tools ──│
│  (use active workspace key)  │
├──────────────────────────────┤
│  Workspace Registry:         │
│    Instantly   → key_1       │
│    Airbyte     → key_2       │
│    Velox       → key_3       │
└──────────────────────────────┘
```

### Status: Operational (3 MCPs) — Consolidation recommended

---

## 3. Manus AI

### Current State

- **Official Manus MCP:** Available on npm as `manus-mcp` (by nanameru). Provides tools for creating AI tasks, managing webhooks, and integrating Manus workflows. Uses `MANUS_MCP_API_KEY` env variable. Open API docs at [open.manus.ai/docs](https://open.manus.ai/docs).
- **Manus as MCP client:** Manus also supports inbound MCP connections, allowing it to read from connected tools like Notion, Atlassian, Linear, Gmail, Slack, etc.

### Recommendation

**Ready to use. No custom build needed.**

Install via `npx manus-mcp` with your API key. Use it to delegate complex multi-step research tasks from Claude to Manus agents.

### Integration Path

```bash
# Install and connect
npx manus-mcp

# Or add as Claude Code MCP
claude mcp add manus --transport stdio --command "npx manus-mcp" --env MANUS_MCP_API_KEY=YOUR_KEY
```

### Status: Ready to use — Official MCP on npm

---

## 4. SourceScrub

### Current State

- **No MCP server exists anywhere** — not native, not community, not on any registry.
- **REST API available** at `api.sourcescrub.com` — requires contacting SourceScrub for access credentials.
- **API auth model:** Username/password-based token request, then bearer token for subsequent calls. Endpoints cover companies, searches, and contacts.
- **Native integrations:** SourceScrub integrates with Affinity, Salesforce, DealCloud, and SugarCRM but has no AI/MCP layer.

### Recommendation

**Custom MCP build required.**

### Build Plan

| Aspect | Detail |
|--------|--------|
| Stack | TypeScript + FastMCP (consistent with Signaliz architecture) |
| Auth | Token refresh flow (username/password → bearer token with TTL caching) |
| Transport | Remote HTTP MCP for Claude.ai compatibility |
| Est. effort | 2–3 days (pending API access confirmation) |

### Core Tools to Implement

```typescript
// Tool definitions for SourceScrub MCP
const tools = [
  'search_companies',      // Search companies by criteria
  'get_company',           // Get company details by ID
  'search_contacts',       // Search contacts at companies
  'get_contact',           // Get contact details by ID
  'get_company_signals',   // Retrieve company signal data
  'search_conferences',    // Search conference/event data
];
```

### Auth Flow

```
┌─────────┐    POST /token     ┌──────────────┐
│  Client  │ ───────────────► │  SourceScrub  │
│          │  user/pass        │     API       │
│          │ ◄─────────────── │              │
│          │  bearer_token     │              │
│          │                   │              │
│          │  GET /companies   │              │
│          │  Auth: Bearer xxx │              │
│          │ ───────────────► │              │
└─────────┘                   └──────────────┘
```

### Blockers

- [ ] API access credentials — must contact SourceScrub team
- [ ] API documentation review — confirm endpoint coverage

### Status: Custom build required — Blocked on API access

---

## 5. Outreach.io

### Current State

- **Official Outreach MCP Server:** Native MCP at `api.outreach.io/mcp/` following MCP Authorization standards (Nov 2025). Connects via `npx mcp-remote`.
- **Requirements:** Organization must have **Amplify** enabled with active credits. User must be active and licensed on at least one Outreach instance.
- **Alternatives:** CData MCP Server for Outreach (read-only via JDBC), Zapier MCP wrapper.

### Recommendation

**Use the official Outreach MCP.**

It provides deal insights, sequence data, and recommended next actions. Full read/write on prospects, sequences, and accounts. The only gate is the Amplify license requirement.

### Integration Path

```bash
# Connect via mcp-remote
claude mcp add outreach --transport stdio --command "npx mcp-remote https://api.outreach.io/mcp/"
```

### Capabilities

| Category | Operations |
|----------|-----------|
| Prospects | Read/Write — create, update, search |
| Sequences | Read/Write — manage sequence steps and enrollment |
| Accounts | Read/Write — account management |
| Deal Insights | Read — pipeline analytics and recommendations |
| Next Actions | Read — AI-recommended engagement actions |

### Prerequisites

- [ ] Amplify license enabled on Outreach org
- [ ] Active credits for Amplify features
- [ ] User licensed on Outreach instance

### Status: Ready to use — Requires Amplify license

---

## 6. Lusha

### Current State

- **Official Lusha MCP:** Open-source at [github.com/lusha-oss/lusha-public-api-mcp](https://github.com/lusha-oss/lusha-public-api-mcp). Install via `npx @lusha-org/mcp@latest`. Built on 100M+ contacts and 60M+ companies database.
- **Remote MCP** (fully managed, no local hosting) also available. Listed on Pipedream and Zapier.

### Tools Available

| Tool | Description |
|------|-------------|
| `personBulkLookup` | Look up to 100 contacts at once |
| `companyBulkLookup` | Bulk company enrichment |
| `prospecting search` | Search with filters (title, seniority, location) |

### Recommendation

**Ready to use.**

Best workflow: Use prospecting search to discover contacts → enrich selected results for emails/phones. Pairs well with Signaliz for post-Lusha email verification.

### Integration Path

```bash
# Install official MCP
npx @lusha-org/mcp@latest

# Or add as Claude Code MCP
claude mcp add lusha --transport stdio --command "npx @lusha-org/mcp@latest" --env LUSHA_API_KEY=YOUR_KEY
```

### Synergy with Signaliz

```
┌────────┐  discover   ┌──────────┐  verify   ┌──────────┐  campaign  ┌──────────┐
│  Lusha  │ ─────────► │ Contacts  │ ────────► │ Signaliz │ ────────► │ Instantly │
│ search  │            │ w/ emails │           │ verify   │           │ outbound  │
└────────┘            └──────────┘           └──────────┘           └──────────┘
```

### Status: Ready to use — Official open-source MCP

---

## 7. Crunchbase Pro

### Current State

- **No official Crunchbase MCP.**
- **Community options:**

| Server | Tools | Stack | License | Notes |
|--------|-------|-------|---------|-------|
| Cyreslab-AI/crunchbase-mcp-server | 5 tools | Node.js | MIT | Basic — search companies, details, funding, acquisitions, search people |
| MCPBundles Crunchbase | 20+ tools | — | — | Comprehensive — orgs, people, funding, acquisitions, IPOs, events, jobs, degrees, categories, locations, diversity, principals |

- **Also available:** Bright Data scraper-based MCP (public data only, no API key needed), Zapier wrapper.

### Recommendation

**Use MCPBundles' comprehensive server if you have Enterprise API access.** Otherwise start with Cyreslab-AI for basic lookups.

If you need full Pro search/filter capabilities not covered by either, a custom enhancement layer on top of Cyreslab-AI is ~1 day of work.

### Decision Tree

```
Do you have Crunchbase Enterprise/Applications API access?
├── YES → Use MCPBundles Crunchbase (20+ tools, full coverage)
├── NO, but have Basic API key → Use Cyreslab-AI (5 tools, covers core lookups)
└── NO API key at all → Use Bright Data scraper MCP (public data only)
```

### Status: Ready to use — Community MCP (choose by API tier)

---

## 8. Apollo.io

### Current State

- **Official Apollo MCP in Claude:** Launched late Feb 2026 as a native connector inside Claude, powered by Apollo's own MCP server hosted on Apollo infrastructure. Beta/early access, available on all paid Apollo plans at no additional cost. Supports search, enrich, create/update contacts, add to sequences.
- **Community options:**

| Server | Stack | Notes |
|--------|-------|-------|
| @thevgergroup/apollo-io-mcp | npm/Node.js | 9 tools, 94% smaller payloads, optimized for Claude Desktop |
| lkm1developer | TypeScript | Community server |
| edwardchoh | Python | Community server |

### Recommendation

**Enable the native Apollo connector in Claude.ai.** It mirrors your Apollo account permissions exactly.

For Claude Code or non-Claude clients, use `@thevgergroup/apollo-io-mcp` which has the most polished implementation. Apollo also offers a dedicated Claude Code/Cowork plugin with namespaced slash commands for power users.

### Integration Path

```bash
# For Claude.ai — enable native connector in Settings → Connected Apps → Apollo

# For Claude Code — use community MCP
claude mcp add apollo --transport stdio --command "npx @thevgergroup/apollo-io-mcp" --env APOLLO_API_KEY=YOUR_KEY
```

### Capabilities

| Operation | Native Connector | Community MCP |
|-----------|:----------------:|:-------------:|
| Search people | Yes | Yes |
| Search companies | Yes | Yes |
| Enrich contacts | Yes | Yes |
| Create contacts | Yes | Yes |
| Update contacts | Yes | Yes |
| Add to sequences | Yes | Limited |
| Optimized payloads | — | Yes (94% smaller) |

### Status: Ready to use — Native connector + community MCP

---

## 9. AlphaSense

### Current State

- **No MCP server exists anywhere.**
- **GraphQL API** available at `api.alpha-sense.com/gql` with search, document retrieval, and content analysis capabilities.
- **Auth model:** API key + client_id/client_secret + OAuth bearer token (password grant type).
- **Important note:** Alpha Vantage (financial market data) has MCP servers, but this is a completely different product from AlphaSense (enterprise research intelligence).
- **API access** requires Enterprise license and explicit API credentials from their team (apisupport@alpha-sense.com).

### Recommendation

**Custom MCP build required.**

### Build Plan

| Aspect | Detail |
|--------|--------|
| Stack | TypeScript + FastMCP with GraphQL client (`graphql-request`) |
| Auth | OAuth token flow with auto-refresh (password grant type) |
| Transport | Remote HTTP MCP for Claude.ai compatibility |
| Est. effort | 3–4 days (GraphQL schema discovery + auth complexity + API access procurement) |

### Core Tools to Implement

```typescript
// Tool definitions for AlphaSense MCP
const tools = [
  'search_documents',         // Keyword/date/type filters across research library
  'get_document',             // Retrieve specific document content
  'search_companies',         // Company lookup and filtering
  'get_company_financials',   // Financial data for a company
  'get_earnings_transcripts', // Earnings call transcripts
  'search_expert_calls',      // Expert call/interview search
];
```

### Auth Flow

```
┌─────────┐  POST /oauth/token        ┌─────────────┐
│  Client  │  grant_type=password      │  AlphaSense  │
│          │  client_id + secret       │     API      │
│          │  api_key + user/pass      │              │
│          │ ──────────────────────►  │              │
│          │                           │              │
│          │  ◄── access_token ──────  │              │
│          │                           │              │
│          │  POST /gql                │              │
│          │  Auth: Bearer xxx         │              │
│          │ ──────────────────────►  │              │
└─────────┘                           └─────────────┘
```

### Blockers

- [ ] Enterprise license — required for API access
- [ ] API credentials — must contact apisupport@alpha-sense.com
- [ ] GraphQL schema — needs discovery after access is granted

### Status: Custom build required — Blocked on Enterprise license + API access

---

## Summary Matrix

| Platform | MCP Exists | Type | Effort | Status | Priority |
|----------|:----------:|------|:------:|--------|:--------:|
| **Affinity CRM** | Yes | Community (yaniv-golan) | None | Ready to use | High |
| **Instantly** | Yes | Official + Community | ~2 days (consolidation) | Operational, optimize later | Medium |
| **Manus AI** | Yes | Official (npm) | None | Ready to use | Medium |
| **SourceScrub** | No | Custom build needed | 2–3 days | Blocked on API access | High |
| **Outreach.io** | Yes | Official | None | Requires Amplify license | Medium |
| **Lusha** | Yes | Official (open-source) | None | Ready to use | High |
| **Crunchbase Pro** | Partial | Community options | 0–1 days | Ready (choose by API tier) | Medium |
| **Apollo.io** | Yes | Official (native) | None | Ready to use | High |
| **AlphaSense** | No | Custom build needed | 3–4 days | Blocked on API access | Low |

---

## Implementation Roadmap

### Phase 1 — Immediate (No Build Required)

Connect platforms with existing MCP servers:

1. **Affinity CRM** — Install `yaniv-golan/affinity-sdk` via `uvx`
2. **Lusha** — Install `@lusha-org/mcp@latest` via `npx`
3. **Apollo.io** — Enable native connector in Claude.ai; add community MCP for Claude Code
4. **Manus AI** — Install `manus-mcp` via `npx`
5. **Outreach.io** — Connect official MCP (pending Amplify license)
6. **Crunchbase Pro** — Install Cyreslab-AI or MCPBundles MCP (by API tier)

### Phase 2 — Short-Term Build (1–2 Weeks)

Custom work and consolidation:

1. **Instantly consolidation** — Fork `bcharleson/instantly-mcp`, add workspace registry, deploy unified server (~2 days)
2. **SourceScrub MCP** — Build custom TypeScript MCP after securing API access (~2–3 days)

### Phase 3 — Medium-Term Build (2–4 Weeks)

Complex builds requiring external dependencies:

1. **AlphaSense MCP** — Build custom TypeScript + GraphQL MCP after securing Enterprise license + API access (~3–4 days)

### Phase 4 — Migration (When Available)

Swap community MCPs for official ones:

1. **Affinity CRM** — Migrate from `yaniv-golan/affinity-sdk` to official Affinity MCP when launched

---

## Integration Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    Claude Code + Signaliz Plugin                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────┐  ┌──────────┐  ┌──────────┐  ┌────────────┐ │
│  │   Signaliz    │  │  Octave   │  │ Instantly │  │  Affinity  │ │
│  │  email find/  │  │ company & │  │ campaign  │  │  CRM data  │ │
│  │   verify      │  │  people   │  │ mgmt      │  │  & notes   │ │
│  └──────────────┘  └──────────┘  └──────────┘  └────────────┘ │
│                                                                  │
│  ┌──────────────┐  ┌──────────┐  ┌──────────┐  ┌────────────┐ │
│  │    Lusha      │  │  Apollo   │  │ Outreach │  │   Manus    │ │
│  │  contact      │  │ search &  │  │ sequences│  │  AI agent  │ │
│  │  discovery    │  │  enrich   │  │ & deals  │  │  delegate  │ │
│  └──────────────┘  └──────────┘  └──────────┘  └────────────┘ │
│                                                                  │
│  ┌──────────────┐  ┌──────────┐  ┌──────────────────────────┐  │
│  │  Crunchbase   │  │AlphaSense│  │      SourceScrub         │  │
│  │  funding &    │  │ research │  │    company signals       │  │
│  │  market data  │  │ intel    │  │    (custom build)        │  │
│  └──────────────┘  └──────────┘  └──────────────────────────┘  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

*This analysis is based on MCP ecosystem state as of March 2026. Platform availability and capabilities may change. Check individual platform documentation for the latest information.*
