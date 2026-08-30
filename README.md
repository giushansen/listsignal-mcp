# ListSignal MCP Server

Live business intelligence for AI agents: 16M+ online businesses with tech
stacks, Shopify apps, revenue and employee estimates, hiring signals with
posting dates, SEO scores, and contact data. New businesses are discovered
within hours of getting a TLS certificate.

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

## About the data

ListSignal discovers businesses from Certificate Transparency logs (so it
finds companies the moment they go live), then enriches each domain with
technology detection (230+ technologies, 260+ Shopify apps), revenue and
employee estimation, business classification, hiring signals from 14,000+
job boards (Greenhouse, Lever, Workday, Personio and more, with posting
dates), SEO scoring, and published contact data. Methodology:
[listsignal.com/scoring/revenue-estimation](https://listsignal.com/scoring/revenue-estimation).
