# PLC Cloud Sim — MCP server for LAD / SCL simulation

**Cloud PLC simulator over MCP** by [汇智网 (hubwiz.com)](https://www.hubwiz.com/).
Validate and step-run Ladder (LAD) and SCL programs, run scenario acceptance, get a PASS/FAIL
report — **without installing TIA Portal**.

> This repository is **documentation and client configuration only**. The simulator runs as a
> hosted service at `tia.hubwiz.com`; its implementation is not open source.

- **Endpoint**: `https://tia.hubwiz.com/sim/mcp`
- **Transport**: Streamable HTTP (stateless, POST JSON-RPC)
- **Auth**: `Authorization: Bearer sim_live_…` (the `X-API-Key` header is also accepted)
- **Dashboard / API keys**: https://tia.hubwiz.com/sim
- **Registry entry**: `io.github.tia-forge/plc-cloud-sim` ([official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=plc-cloud-sim))

## What it does

| Tool | Purpose |
|---|---|
| `lad_validate` | Validate one or more LadDoc ladder documents |
| `lad_simulate` | Run scan cycles with given inputs, read back variables and diagnostics |
| `lad_run_scenario` | Run a declarative scenario (cases / exhaustive matrix / seeded fuzz / `always` invariants) |
| `scl_validate` | Parse and validate SCL source |
| `scl_simulate` | Step-run SCL across scan cycles and read variables back |
| `scl_run_scenario` | Run a declarative SCL scenario |

Semantics match a real PLC scan: contacts, coils, timers/counters, FC/FB calls, jumps, and
`IF`/`CASE`/`FOR`/`WHILE` for SCL. Execution is **deterministic and reproducible** — the same
input yields the same result, so a scenario is usable as a regression test.

## Quick start

### Claude Code

```bash
claude mcp add --transport http sim-cloud https://tia.hubwiz.com/sim/mcp \
  --header "Authorization: Bearer sim_live_YOUR_KEY"
claude mcp list
```

### Cursor / VS Code / any JSON-configured client

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

### Ask the agent for something

```
Validate this SCL block, then run it for two scans with Start=false then Start=true,
and tell me the final value of Motor.
```

## Pricing

- WeChat login on the dashboard issues a key immediately: **100 calls free**, no signup form.
- Top-ups: **1000 calls for ¥9**, one-off — **no subscription, no monthly fee**.
- Handshake requests (`initialize`, `tools/list`) are free; each `tools/call` costs 1 call.

## Notes

- No TIA Portal installation is required — the program text (LadDoc / SCL) is sent to the service.
  If your policy forbids uploading program text, use an on-premises path instead (see
  [TiaLink](https://github.com/tia-forge/tialink) for the on-machine gateway).
- Related services: [LadDoc Cloud](https://github.com/tia-forge/laddoc-cloud) (LadDoc → SimaticML),
  [TIA Portal ladder examples](https://tia.hubwiz.com/learn-lad).
- Documentation license: CC BY 4.0. "TIA Portal", "SIMATIC" and "S7" are trademarks of Siemens AG;
  this service is an independent third-party tool, not affiliated with or endorsed by Siemens.
