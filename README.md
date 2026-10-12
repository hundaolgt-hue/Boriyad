# Boriyad

Architectural design project driven from Claude Code through Rhino 8 via
[RhinoMCP](https://github.com/jingcheng-chen/rhinomcp).

## How the connection works

```
Claude Code ──stdio──▶ rhinomcp (uvx, Python) ──TCP 127.0.0.1:1999──▶ RhinoMCP plugin inside Rhino 8
```

The MCP server talks to Rhino over a local socket, so Claude Code must run on
the **same machine as Rhino** (Windows or macOS). A cloud session cannot reach
your local Rhino.

## Setup (one time)

1. **Rhino plugin** — In Rhino 8: `Tools → Package Manager`, search `rhinomcp`,
   install, restart Rhino.
2. **uv** (provides `uvx`, needs Python 3.10+):
   - macOS: `brew install uv`
   - Windows: `powershell -c "irm https://astral.sh/uv/install.ps1 | iex"`
3. **Claude Code** — clone this repo and run `claude` from its root. The
   project-scoped `.mcp.json` registers the `rhino` server; approve it when
   prompted. (Alternative: `claude mcp add rhino -- uvx rhinomcp@latest`.)

## Every Rhino session

1. Open Rhino 8 (and your `.3dm` file).
2. Type `mcpstart` in the Rhino command line (`mcpstop` to stop).
3. In Claude Code, run `/mcp` and confirm `rhino` is connected.
4. Smoke test: ask Claude to "summarize the current Rhino document".

Only run one RhinoMCP server at a time.

## Troubleshooting

| Symptom | Fix |
| --- | --- |
| `rhino` shows failed in `/mcp` | Check `uvx --version` works in the same shell you launch `claude` from |
| Connected but tool calls time out | Run `mcpstart` in Rhino; check nothing else is using port 1999 |
| Port conflict | Change `RHINO_MCP_PORT` in `.mcp.json` and the plugin setting to match |

## Layout

- `models/` — Rhino `.3dm` files
- `grasshopper/` — `.gh` definitions
- `scripts/` — reusable RhinoPython / C# scripts
- `exports/` — viewport captures, drawings, renders
