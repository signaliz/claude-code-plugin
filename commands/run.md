---
description: Run Signaliz enrichment — find emails, verify emails, or get company signals
argument-hint: <capability> [data...]
allowed-tools: [Signaliz:execute_primitive, Signaliz:find_and_verify_emails, Signaliz:verify_emails, Signaliz:enrich_company_signals, Signaliz:verify_email, Signaliz:check_job_status, Signaliz:company_search]
---

# Signaliz Quick Run

Parse the user's arguments and run the appropriate Signaliz capability.

## Argument Parsing

The first argument determines the capability:

- `find` or `emails` → find-verified-emails skill
- `verify` or `check` → verify-emails skill
- `signals` or `enrich` or `research` → company-signals skill

Remaining arguments are the data (names, emails, or domains).

## Examples

```
/signaliz:run find Jane Smith at acme.com
/signaliz:run verify alice@acme.com bob@bigco.com
/signaliz:run signals stripe.com notion.so
```

## Execution

1. Determine the capability from the first argument
2. Parse the remaining arguments into structured input
3. Call the appropriate Signaliz MCP tool
4. Present results clearly with status indicators

If no arguments are provided, show a brief help message with the examples above.

If the Signaliz MCP is not connected, direct the user to run `/signaliz:setup`.
