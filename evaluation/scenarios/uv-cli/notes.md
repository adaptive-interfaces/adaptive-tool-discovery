# Evaluation Notes: uv CLI

## What to look for

### Discovery

- Agent runs `uv --help` before anything else
- Agent runs `uv <subcommand> --help` for each subcommand
- Does not rely on prior knowledge of uv's interface

### Capability accuracy

- `uv run` correctly identified as running a script or command
- `uv add` correctly identified as adding a dependency
- Required vs optional arguments correctly distinguished
- `--frozen` and `--no-sync` flags noted for relevant subcommands

### Verification

- Agent executes at least one safe subcommand (e.g., `uv --version`)
- Records actual output, not just help text
- Failure modes verified live: exit 2 with "No pyproject.toml found"
  confirmed for uv add, uv sync, and uv lock outside a project root
- uv run verified as the exception — succeeds without a project
  when --no-project behavior applies

### Registry

- Invocation path correctly recorded (e.g., `uv run`, not just `run`)
- Failure modes grounded in observed behavior or help text
- No hallucinated flags

## Common failure modes

- Agent relies on training knowledge of uv rather than reading help output
- Agent conflates uv subcommands with pip or poetry equivalents
- Agent omits failure modes entirely
- Agent writes to `tools/registry.toml` instead of `tools/uv-cli.toml`
  (prompt not read carefully enough)
- Agent treats uv run as requiring a project root
  (it does not unlike the other three subcommands)
- Agent misses the cross-command --frozen/--locked/--dry-run pattern
  that applies consistently across all four subcommands
