---
description: Set up Signaliz — get your API key, connect the MCP, and verify the connection
allowed-tools: [Signaliz:company_search, Signaliz:verify_email]
---

# Signaliz Setup

Walk the user through connecting Signaliz to Claude Code.

## Step 1: Check if Already Connected

Try calling `Signaliz:company_search` with query "test". If it succeeds, Signaliz is already connected — tell the user and skip to Step 4.

If it fails, continue to Step 2.

## Step 2: Get an API Key

Tell the user:

> **Get your Signaliz API key:**
>
> 1. Go to [signaliz.com/signup](https://signaliz.com/signup) and create a free account
> 2. Navigate to **Settings → API Keys**
> 3. Click **Create API Key** and copy the key (starts with `sk_`)
> 4. Come back here and paste your API key

Wait for the user to provide their API key.

## Step 3: Configure the MCP Connection

Once the user provides their API key, instruct them:

> **Connect Signaliz to Claude Code:**
>
> Run this command in your terminal:
> ```
> claude mcp add signaliz --transport http --url "https://api.signaliz.com/functions/v1/signaliz-mcp?api_key=YOUR_KEY_HERE"
> ```
>
> Replace `YOUR_KEY_HERE` with your actual API key.
>
> Then restart Claude Code to pick up the new MCP connection.

## Step 4: Verify the Connection

Once connected, run a quick verification:

1. Call `Signaliz:company_search` with query "anthropic"
2. If it returns results, tell the user: "✅ Signaliz is connected and working!"
3. Show them what's available:

> **You're all set! Here's what you can do:**
>
> - **Find emails:** "Find the verified email for Jane Smith at acme.com"
> - **Verify emails:** "Verify these emails: alice@example.com, bob@test.com"
> - **Company signals:** "What signals do you have on stripe.com?"
> - **Batch from CSV:** "Upload a CSV and I'll verify all the emails"
>
> Signaliz gives you 100 free credits to start. Each email find costs 1 credit, verification costs 0.5, and company signals cost 2.

## If Connection Fails

If the MCP connection fails:
- Confirm the API key is correct (starts with `sk_`)
- Try the URL directly in a browser to check for errors
- Supabase cold starts can cause the first request to fail — wait 5 seconds and retry
- If persistent, suggest the user contact support@signaliz.com
