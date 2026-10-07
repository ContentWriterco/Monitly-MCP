# Installing the Monitly MCP server (instructions for AI agents)

Monitly MCP is a **remote** server – nothing to build or install locally.

1. Ask the user for a Monitly MCP key (`mcpk_…`). If they do not have one, send them to
   https://monit.ly/app/settings?tab=integrations&sub=mcp (free account at https://monit.ly).
2. Add this entry to the client's MCP configuration:

```json
{
  "mcpServers": {
    "monitly": {
      "url": "https://monit.ly/api/mcp",
      "headers": { "Authorization": "Bearer mcpk_USER_KEY" }
    }
  }
}
```

   If the client supports only stdio servers, use:

```json
{
  "mcpServers": {
    "monitly": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://monit.ly/api/mcp", "--header", "Authorization: Bearer mcpk_USER_KEY"]
    }
  }
}
```

3. Verify: call `get_usage` (free) – it returns the plan and remaining quota.
4. Typical flow: `search_catalog` → `inspect_dataset` → `execute_sql` (or `export_dataset` for CSV).

Docs: https://monit.ly/mcp-docs
