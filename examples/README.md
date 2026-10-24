# Client setups

## Claude Code

```bash
claude mcp add --transport http sim-cloud https://tia.hubwiz.com/sim/mcp \
  --header "Authorization: Bearer sim_live_YOUR_KEY"
claude mcp list          # expect: sim-cloud … connected
```

## Cursor

`~/.cursor/mcp.json` (or project `.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "sim-cloud": {
      "url": "https://tia.hubwiz.com/sim/mcp",
      "headers": { "Authorization": "Bearer sim_live_YOUR_KEY" }
    }
  }
}
```

## VS Code (Copilot / MCP-aware builds)

`.vscode/mcp.json`:

```json
{
  "servers": {
    "sim-cloud": {
      "type": "http",
      "url": "https://tia.hubwiz.com/sim/mcp",
      "headers": { "Authorization": "Bearer ${input:sim_key}" }
    }
  },
  "inputs": [
    { "id": "sim_key", "type": "promptString", "description": "sim_live_… API key", "password": true }
  ]
}
```

## Troubleshooting

| Symptom | Cause |
|---|---|
| `401` on the first request | Missing/expired key, or the `Bearer ` prefix was dropped |
| `404` | Wrong path — the endpoint is `/sim/mcp`, not `/mcp` |
| `initialize` works, `tools/call` fails on quota | Free 100 calls used up; top up in the dashboard |
