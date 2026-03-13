---
name: manus-delegate
description: >
  Delegate complex research tasks to Manus AI agents. Trigger when the user asks
  to: use Manus, delegate a research task, run a Manus agent, do deep research,
  multi-step analysis, or complex investigation. Also trigger on phrases like
  "have Manus research", "delegate this to Manus", "deep research on",
  "Manus task", or "run a Manus agent to".
version: 1.0.0
---

# Manus AI Agent Delegation

Delegate complex, multi-step research tasks to Manus AI agents. Manus agents can browse the web, analyze documents, and perform multi-step reasoning autonomously.

## Prerequisites

The Manus MCP must be connected. If Manus tools are not available, instruct the user:

> **Connect Manus AI:**
>
> ```bash
> claude mcp add manus --transport stdio --command "npx manus-mcp" --env MANUS_MCP_API_KEY=YOUR_MANUS_API_KEY
> ```
>
> Get your API key from [open.manus.ai](https://open.manus.ai) → API Keys.

Before making any Manus MCP calls, use `tool_search` to load the correct tool definitions and parameter schemas.

## When to Use Manus

Delegate to Manus when the task requires:

- **Deep web research** — comprehensive multi-source investigation
- **Multi-step analysis** — tasks that require sequential reasoning across sources
- **Document analysis** — processing and summarizing large documents
- **Complex workflows** — tasks that benefit from autonomous agent execution

## Workflow

### Create a Research Task

1. Formulate a clear, specific task description
2. Use Manus to create the task
3. Monitor progress via webhooks or polling
4. Retrieve and present results

### Example Tasks for Manus

| Task Type | Example |
|-----------|---------|
| Competitive analysis | "Research the top 5 competitors of [company] — pricing, features, market position" |
| Market research | "Analyze the AI sales tools market — size, growth, key players, trends" |
| Company deep-dive | "Research [company] — founders, funding, product, customers, recent news" |
| Industry mapping | "Map the B2B data enrichment landscape — categories, players, differentiators" |

## Combining with Other Tools

Manus provides research depth that complements transactional tools:

- **Manus research → Octave** — Deep research first, then find matching companies
- **Manus research → Crunchbase** — Validate Manus findings with Crunchbase data
- **Manus research → Signaliz** — Research companies, then get sales signals
- **Manus analysis → Affinity** — Research-backed notes pushed to CRM

## Output

Present Manus results with:
- Task summary and completion status
- Key findings organized by category
- Sources and references
- Offer to act on findings: find contacts, verify emails, add to CRM
