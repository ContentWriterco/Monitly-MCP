# 📊 Monitly MCP Server – Official Statistics for AI Agents

[![MCP](https://img.shields.io/badge/MCP-2024--11--05-blue)](https://modelcontextprotocol.io)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Monitly](https://img.shields.io/badge/Powered_by-Monitly-black)](https://monit.ly)
[![Docs](https://img.shields.io/badge/Docs-MCP-blue)](https://monit.ly/mcp-docs)

**[Monitly](https://monit.ly) MCP** is a remote [Model Context Protocol](https://modelcontextprotocol.io) server that gives Claude, Cursor, Windsurf, VS Code and any MCP-compatible agent direct access to **official statistics and economic indicators** – Eurostat, World Bank, OECD, IMF, WHO and national statistical offices – for **150+ countries**.

Ask in plain language: the agent finds the right dataset, checks the country coverage and pulls the time series.

---

## Quick start

1. Create a free account at [monit.ly](https://monit.ly) and generate an MCP key in **Settings → Integrations → MCP** ([direct link](https://monit.ly/app/settings?tab=integrations&sub=mcp)). MCP keys start with `mcpk_`.
2. Add the server to your client.

**Cursor / Windsurf / VS Code / Claude Desktop** (`mcp.json` / `claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "monitly": {
      "url": "https://monit.ly/api/mcp",
      "headers": {
        "Authorization": "Bearer mcpk_YOUR_KEY_HERE"
      }
    }
  }
}
```

**Claude Code**:

```bash
claude mcp add --transport http monitly https://monit.ly/api/mcp \
  --header "Authorization: Bearer mcpk_YOUR_KEY_HERE"
```

**Clients that only support stdio** (via [`mcp-remote`](https://www.npmjs.com/package/mcp-remote)):

```json
{
  "mcpServers": {
    "monitly": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://monit.ly/api/mcp",
               "--header", "Authorization: Bearer mcpk_YOUR_KEY_HERE"]
    }
  }
}
```

| Field | Value |
|-------|-------|
| Endpoint | `POST https://monit.ly/api/mcp` |
| Transport | Streamable HTTP (JSON-RPC 2.0) |
| Authentication | `Authorization: Bearer mcpk_…` or `X-API-Key: mcpk_…` |
| MCP version | `2024-11-05` |
| Documentation | [monit.ly/mcp-docs](https://monit.ly/mcp-docs) |

---

## Example prompts

- Compare HICP inflation in Poland, Germany and Czechia over the last ten years.
- Rank EU countries by the latest unemployment rate.
- Which EU country had the fastest real GDP growth last year?
- Show government debt as % of GDP for the Baltic states since 2010.
- Give me a macro snapshot of Poland: GDP, inflation, unemployment and interest rates.
- How has the Polish current account balance changed quarter by quarter?

---

## Tools

### Data tools (count toward the monthly `mcp_queries` quota)

| Tool | What it does |
|------|--------------|
| `search_catalog` | Find statistical datasets (Eurostat, World Bank, OECD, IMF…) with hybrid search (name/code + embeddings). Optional `country` and `source_name` filters. Returns compact dataset cards. |
| `search_datasets` | Alias of `search_catalog`. |
| `inspect_dataset` | Peek one dataset: default slice, whether a country is covered, `dataId` and the latest values. |
| `get_dataset` | Full metadata for one dataset: description, source and metadata URLs, dimensions, period range, geographies. |
| `execute_sql` | Read-only `SELECT` against the `main` and `data` tables to pull a full time series. |
| `export_dataset` | CSV download URL for a dataset, optionally filtered by countries and dimension values. |

### Account tools (free)

| Tool | What it does |
|------|--------------|
| `list_plans`, `get_plan`, `get_usage` | Plans, current plan and limits, usage this billing period. |
| `upgrade_plan` | Returns a Stripe Checkout URL for the Pro plan. |
| `list_watchlist`, `add_to_watchlist`, `add_watchlist_batch`, `remove_from_watchlist` | Follow datasets and get alerts when they update. |
| `set_notification_preferences` | E-mail / Slack alert settings. |
| `list_api_keys`, `create_api_key`, `delete_api_key` | REST API keys (`mon_…`). |
| `list_mcp_keys`, `create_mcp_key`, `delete_mcp_key` | MCP keys (`mcpk_…`). |

`initialize`, `tools/list` and `ping` are never metered and work without a key, so clients can list tools before you add one.

---

## Data sources

Eurostat · World Bank · OECD · IMF · WHO · national statistical offices (e.g. Statistics Poland – BDL).
Browse the catalog at [monit.ly](https://monit.ly).

## Related

- [Monitly REST API](https://monit.ly/api-docs)
- [Compabase MCP](https://github.com/ContentWriterco/Compabase-MCP) – 3M+ Polish companies (KRS, CEIDG)

## License

MIT for this repository (documentation and configuration). The Monitly service is subject to the [Monitly terms](https://monit.ly).
