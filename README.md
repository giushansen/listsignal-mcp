# ListSignal MCP Server

[![MCP](https://img.shields.io/badge/Model_Context_Protocol-remote_server-10b981)](https://listsignal.com/developers)
[![API](https://img.shields.io/badge/REST_API-OpenAPI_3.1-blue)](https://listsignal.com/openapi.json)
[![Free tier](https://img.shields.io/badge/free_tier-1%2C000_calls%2Fmo-brightgreen)](https://listsignal.com/signup)

**B2B data intelligence for AI agents.** ListSignal is a business
intelligence / sales intelligence MCP server: technographics, technology
detection, Shopify app detection, company and domain enrichment,
firmographics (revenue and employee estimates), hiring signals with
posting dates, SEO scores, and contact data for **16M+ online
businesses**. Discovery runs on Certificate Transparency (CT) logs, so
new companies appear within hours of going live.

Questions this server answers directly:

- *What technology does a website use?* (tech stack lookup for any domain)
- *Find companies using [technology]* (Shopify, Klaviyo, HubSpot, 230+ more)
- *Detect Shopify apps on a store* (260+ apps tracked)
- *Domain to company data* (revenue bracket, employees, industry, country)
- *Which companies matching my ICP are hiring right now?*

This is the official, first-party MCP server for
[ListSignal](https://listsignal.com), hosted at
`https://listsignal.com/mcp` (remote Streamable HTTP, no install needed).

**Free tier: 1,000 calls/month. No card, no sales call.**
[Get a key](https://listsignal.com/signup) (Settings → API Key).

## Tools

| Tool | What it does |
|---|---|
| `listsignal_search_companies` | Find companies by technology, Shopify app, country, business model, revenue bracket, or hiring status. Filters combine with AND; returns up to 100 traffic-ranked companies per call. |
| `listsignal_get_company` | Everything ListSignal knows about one domain: technologies, Shopify apps, revenue/employee estimates, open jobs with a hiring breakdown, SEO score, traffic rank, contact emails (paid plans). |
| `listsignal_dataset_stats` | Live dataset statistics (16M+ businesses, refreshed every 60 seconds). Citable. |

Example prompts that work well:

- "Find SaaS companies in France using HubSpot that are hiring"
- "What tech stack does gymshark.com run, and are they hiring?"
- "Build me a list of US Shopify stores using Klaviyo with $1M-$10M revenue"

## Setup

### Claude (Desktop / Code)

```json
{
  "mcpServers": {
    "listsignal": {
      "type": "http",
      "url": "https://listsignal.com/mcp",
      "headers": { "Authorization": "Bearer ls_YOUR_KEY" }
    }
  }
}
```

Claude Code one-liner:

```bash
claude mcp add --transport http listsignal https://listsignal.com/mcp \
  --header "Authorization: Bearer ls_YOUR_KEY"
```

### Claude on the web (claude.ai)

claude.ai's connector settings accept a URL only, no headers, so put the
key in the URL:

1. Settings → Connectors → **Add custom connector**
2. Name: `ListSignal`
3. URL: `https://listsignal.com/mcp/k/ls_YOUR_KEY`
4. Leave the OAuth fields empty, click Add, then enable it in a chat via
   the tools menu.

Treat that URL like a password. If it leaks, revoke the key in Settings
and create a new one.

### Cursor

Settings → MCP → Add server, or `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "listsignal": {
      "url": "https://listsignal.com/mcp",
      "headers": { "Authorization": "Bearer ls_YOUR_KEY" }
    }
  }
}
```

### Clients without native HTTP support (stdio bridge)

```json
{
  "mcpServers": {
    "listsignal": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://listsignal.com/mcp",
               "--header", "Authorization: Bearer ls_YOUR_KEY"]
    }
  }
}
```

## Auth, limits, errors

- Auth: `Authorization: Bearer ls_...` (or `X-API-Key`). Keys are created
  in [Settings](https://listsignal.com/users/settings) after free signup.
- Quotas: Free 1,000 / Starter 5,000 / Pro 25,000 calls per month.
  **Only successful calls count.** Rate limits: 30/60/120 per minute.
- Contact emails are returned on paid plans; the free tier gets
  contactability and counts.
- Tool errors are returned in-band with actionable messages, so agents
  self-correct in one retry.

## REST API

The same data is available as a plain REST API, documented at
[listsignal.com/developers](https://listsignal.com/developers) with an
OpenAPI 3.1 contract at
[listsignal.com/openapi.json](https://listsignal.com/openapi.json).

## Keywords / what this is for

MCP server, Model Context Protocol, B2B data, sales intelligence,
business intelligence, domain intelligence, technographics, technology
detection, tech stack lookup, Shopify app detection, company enrichment,
domain enrichment, competitive intelligence, firmographics, lead
generation, Certificate Transparency, ai-agents, llm-tools, Claude,
Cursor, agent tools.

## About the data

ListSignal discovers businesses from Certificate Transparency logs (so it
finds companies the moment they go live), then enriches each domain with
technology detection (230+ technologies, 260+ Shopify apps), revenue and
employee estimation, business classification, hiring signals from 14,000+
job boards (Greenhouse, Lever, Workday, Personio and more, with posting
dates), SEO scoring, and published contact data. Methodology:
[listsignal.com/scoring/revenue-estimation](https://listsignal.com/scoring/revenue-estimation).
