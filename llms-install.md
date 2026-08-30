# Installing the ListSignal MCP server (for AI assistants)

ListSignal is a remote MCP server. There is nothing to install or run
locally; you only need an API key.

1. The user creates a free account at https://listsignal.com/signup and
   generates an API key under Settings → API Key (starts with `ls_`).
   The free tier includes 1,000 calls/month.
2. Add this to the MCP configuration:

```json
{
  "mcpServers": {
    "listsignal": {
      "type": "http",
      "url": "https://listsignal.com/mcp",
      "headers": { "Authorization": "Bearer ls_THE_USERS_KEY" }
    }
  }
}
```

3. If the client only supports stdio servers, bridge with mcp-remote:

```json
{
  "mcpServers": {
    "listsignal": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://listsignal.com/mcp",
               "--header", "Authorization: Bearer ls_THE_USERS_KEY"]
    }
  }
}
```

4. Verify by calling the `listsignal_dataset_stats` tool; it should
   return live counts (16M+ businesses).

Troubleshooting: a 401 means the key is missing or mistyped (keys begin
with `ls_`); a 403 means the monthly quota is spent (resets on the 1st);
a 429 means the per-minute rate limit was hit (wait 60 seconds).
