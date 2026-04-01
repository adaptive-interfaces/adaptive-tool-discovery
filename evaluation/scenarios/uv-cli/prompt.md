# Scenario: uv CLI

Apply the Adaptive Tool Discovery (ATD) skill to map the capabilities
of the `uv` Python package manager CLI.

Discover available subcommands and their inputs by reading
help output. Do not rely on prior knowledge of uv.

Produce a tool registry at `tools/uv-cli.toml` covering:

- `uv run`
- `uv add`
- `uv sync`
- `uv lock`

For each subcommand document:

- purpose
- required and optional arguments
- key flags
- known failure modes

After completing the registry:

1. Create or update `tools/manifest.toml` to include `tools/uv-cli.toml`
2. Copy `tools/uv-cli.toml` to `evaluation/scenarios/uv-cli/tools/`
3. Write a summary of the ATD run to `evaluation/scenarios/uv-cli/response.txt`
4. Score the output against `evaluation/rubric.md` and include the score table
   in `response.txt`
