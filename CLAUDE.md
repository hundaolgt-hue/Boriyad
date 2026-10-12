# Boriyad — architectural design in Rhino via MCP

The `rhino` MCP server (see `.mcp.json`, `README.md`) drives a live Rhino 8 /
Grasshopper session on the user's machine.

## Working with Rhino

- Start every task with `get_document_summary` to learn units, layers and
  existing geometry before creating anything. Confirm model units (expect
  millimetres or metres) and never assume.
- Put geometry on named layers by building system, e.g. `A-WALL`, `A-FLOR`,
  `A-ROOF`, `A-GLAZ`, `A-STRS`, `S-COLS`, `SITE`. Create missing layers first.
- Prefer native tools (`create_object`, `extrude_curve`, `boolean_*`, `loft`)
  over `run_command`; use `execute_rhinoscript_python_code` for repetitive or
  parametric operations, and save scripts worth keeping in `scripts/`.
- After a modelling step, use `capture_viewport` to check the result visually.
- Destructive operations (`delete_object`, `delete_layer`, `gh_clear_canvas`,
  booleans on existing objects) — confirm with the user first; `undo` exists
  but is not a substitute.
- Keep Grasshopper definitions parametric; store `.gh` files in `grasshopper/`.
