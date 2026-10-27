# ListSignal MCP Server

[![MCP](https://img.shields.io/badge/Model_Context_Protocol-remote_server-10b981)](https://listsignal.com/developers)
[![API](https://img.shields.io/badge/REST_API-OpenAPI_3.1-blue)](https://listsignal.com/openapi.json)
[![Free tier](https://img.shields.io/badge/free_tier-1%2C000_rows%2Fmo%2C_counts_free-brightgreen)](https://listsignal.com/signup)

**B2B data intelligence for AI agents.** ListSignal is a business
intelligence / sales intelligence MCP server: technographics, technology
detection, Shopify app detection, company and domain enrichment,
firmographics (revenue and employee estimates), hiring signals with
posting dates, SEO scores, and contact data for **10s of millions of online
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

**Free tier: 1,000 rows a month, counts free. No card, no sales call.**
[Get a key](https://listsignal.com/signup) (Settings → API Keys).

## Tools

One filter grammar, shared with the ListSignal dashboard, its CSV export
and the REST API: what a company runs (`tech` all of, `tech_any`,
`tech_not`, `dns_tech`, `email_provider`), who it is (`country`,
`business_model`, `industry`, `revenue`, `employees`, `language`,
`keyword`, `min_confidence`, `min_realness`), stores (`shopify`,
`has_catalog`, `min_products`, `max_products`, `min_price_avg`,
`max_price_avg`, `min_new_products_30d`), SaaS and hiring
(`has_pricing`, `hiring`, `min_job_count`, `ats`), reachability
(`has_email`, `has_named_email`, `has_phone`), quality
(`min_seo_score`, `max_seo_score`), when (`first_seen_after`,
`last_checked_after`, `discovered`, `freshness`) and what changed
(`changed_field`, `changed_kind`, `changed_value`, `changed_within`).
Every filter is documented with an example at
[listsignal.com/data](https://listsignal.com/data#filters).

| Tool | Costs | What it does |
|---|---|---|
| `listsignal_count` | nothing | How many companies match the filters. Call it first: size a segment, tighten it, then pull. |
| `listsignal_search_companies` | rows returned | One page of companies in the target ranking (small real businesses with contact details first), up to the plan's rows a page; `total=true` adds the count. |
| `listsignal_query` | rows returned, `limit=0` free | Totals, `group_by` with counts (business model, country, revenue, employees, industry, mail provider, TLD), chosen `fields`, up to 500 rows on Pro and Scale. |
| `listsignal_get_company` | one row | Everything about one domain: 121 fields, the last 20 changes, each email with its kind and a deliverability verdict, the naming pattern, the people found on the site. |
| `listsignal_changes` | rows returned | The change feed, newest first: a technology added or removed, hiring started, a revenue band moved, a site down. |
| `listsignal_dataset_stats` | nothing | Live dataset statistics, refreshed every 60 seconds. Citable. |

Example prompts that work well:

- "How many Shopify stores in Australia with 300+ products run Klaviyo but not Gorgias, with a person's email on the site?"
- "Find UK SaaS companies that are hiring right now and publish an address"
- "Who went from zero to open roles in the last 30 days in the US, UK, Canada or Australia?"
- "What tech stack does gymshark.com run, and what changed there this month?"

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
  in [Settings](https://listsignal.com/users/settings) after free signup;
  make one per connector so each can be revoked alone.
- Plans meter rows a month on every surface (dashboard CSV, REST, MCP):
  Free 1,000 / Starter 2,500 / Pro 15,000 / Scale 75,000. A call costs
  the rows it returns; counts, group-bys with `limit=0` and dataset stats
  are free and keep working when the month is spent. Rate limits are the
  plan's searches a minute: 10 / 30 / 120 / 300.
- Contact emails and phones are returned on paid plans; the free tier
  gets contactability and counts.
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
