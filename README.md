# PinnFeed MCP server — live Pinnacle odds for AI assistants

[PinnFeed](https://pinnfeed.io/?utm_source=github.com&utm_medium=referral&utm_campaign=mcp-repo) hosts a remote [Model Context Protocol](https://modelcontextprotocol.io) server so AI assistants (Claude, Claude Code, Cursor, VS Code Copilot, Codex CLI, ChatGPT) can read live and prematch Pinnacle odds, no-vig fair prices, price drops and closing lines.

PinnFeed is an independent service, not affiliated with Pinnacle.

- **URL:** `https://pinnfeed.io/mcp`
- **Transport:** Streamable HTTP (spec 2025-06-18), stateless JSON responses
- **Auth:** your PinnFeed API key as `Authorization: Bearer <key>` (or `x-api-key`, or `?key=<key>` for clients that cannot send headers)
- **Limits:** your plan's sports, data delay and rate limit; every tool call counts as one REST request. A free plan is available (REST, 30 s delay) — see [pricing](https://pinnfeed.io/pricing).
- **Full docs:** https://pinnfeed.io/docs/mcp

## Tools

| Tool | What it returns |
| --- | --- |
| `get_account` | Plan, sports, rate limit, daily quota, data delay, requests used today |
| `list_sports` | Sports with live / prematch event counts and whether each is in your plan |
| `search_events` | Events by sport, team or league text, live or prematch |
| `get_live_odds` | In-play events with prices (decimal, American, no-vig fair, margin, max stake) |
| `get_prematch_odds` | Upcoming events with prices, soonest first |
| `get_event` | One event with every market |
| `get_drops` | Biggest price drops in the last 1–60 minutes |
| `get_closing_odds` | Closing odds for an event (Builder and above) |

## Install

Get an API key at https://pinnfeed.io, then:

### Claude Code
```bash
claude mcp add --transport http pinnfeed https://pinnfeed.io/mcp --header "Authorization: Bearer YOUR_KEY"
```

### Claude (web and desktop)
Settings → Connectors → Add custom connector, URL: `https://pinnfeed.io/mcp?key=YOUR_KEY`

### Cursor (`~/.cursor/mcp.json`)
```json
{ "mcpServers": { "pinnfeed": { "url": "https://pinnfeed.io/mcp", "headers": { "Authorization": "Bearer YOUR_KEY" } } } }
```

### VS Code Copilot (`.vscode/mcp.json`)
```json
{ "servers": { "pinnfeed": { "type": "http", "url": "https://pinnfeed.io/mcp", "headers": { "Authorization": "Bearer YOUR_KEY" } } } }
```

### Codex CLI
```bash
export PINNFEED_KEY="YOUR_KEY"
codex mcp add pinnfeed --url https://pinnfeed.io/mcp --bearer-token-env-var PINNFEED_KEY
```

### ChatGPT
Settings → Apps & Connectors → Advanced → Developer mode, then create a connector with URL `https://pinnfeed.io/mcp?key=YOUR_KEY` and authentication "No authentication".

### Raw protocol
```bash
curl -s https://pinnfeed.io/mcp -H "Authorization: Bearer YOUR_KEY" -H "content-type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"get_drops","arguments":{"sport":"soccer","minutes":15}}}'
```

## Security
A key in a URL or chat is a secret: use a dedicated key per assistant and revoke it from the PinnFeed dashboard if it leaks.

## Related
- [Python SDK](https://github.com/pinnfeed/pinnfeed-python) · [Node.js / TypeScript SDK](https://github.com/pinnfeed/pinnfeed-node)
- [WebSocket protocol](https://pinnfeed.io/docs/websocket) · [REST API](https://pinnfeed.io/docs/rest)

## License
This repository (documentation and config examples) is MIT licensed. The PinnFeed service is subject to its own terms at https://pinnfeed.io.
