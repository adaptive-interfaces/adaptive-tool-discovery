# Score: mcp-filesystem-server (run 1)

- Date: 2026-03-31
- Response: evaluation/scenarios/mcp-filesystem-server/response.txt
- Registry: evaluation/scenarios/mcp-filesystem-server/tools/mcp-filesystem.toml
- Evaluator: Denise M. Case

## Results

Self-reported score confirmed: **12 / 14 Conformant**

See response.txt for inline self-score table and evidence.

## Notes

MCP server was not connected in the Claude Code session at run time.
Agent correctly detected this, fell back to reading the server source
from GitHub (index.ts), and marked all 14 entries verified = "unverified".

Verification scored 0, no live calls made.
This is the correct score given the constraint, not a failure.
The agent's handling of the gap is itself evidence of correct ATD behavior.

Most thorough discovery of any scenario.
14 tools enumerated from Zod schema declarations in source.
Deprecated alias (read_file) correctly identified and noted.

To complete verification: rerun with MCP server actively connected.
First call: list_allowed_directories (no inputs, no side effects).
