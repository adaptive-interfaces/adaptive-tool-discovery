# adaptive-tool-discovery

[![CI Status](https://github.com/adaptive-interfaces/adaptive-tool-discovery/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/adaptive-interfaces/adaptive-tool-discovery/actions/workflows/ci.yml)
[![Link Check](https://github.com/adaptive-interfaces/adaptive-tool-discovery/actions/workflows/links.yml/badge.svg?branch=main)](https://github.com/adaptive-interfaces/adaptive-tool-discovery/actions/workflows/links.yml)
[![MIT](https://img.shields.io/badge/license-see%20LICENSE-yellow.svg)](./LICENSE)

> Adaptive Tool Discovery (ATD) Skill

Maps the capability surface of external tools, e.g.
MCP servers, APIs, CLIs, and SDKs,
so agents can invoke them correctly without repeated discovery.
ATD produces a persistent tool capability registry
usable by all agents and subagents on a team.

## Motivation

On larger teams, having every agent re-run tool discovery
independently is wasteful and produces inconsistent results.
ATD solves this by running discovery once, verifying tool behavior,
and producing a shared registry that any agent can load.

ATD invokes
[Adaptive Conformance Specification (ACS)](https://github.com/adaptive-interfaces/adaptive-conformance-specification)
as its foundational observation and conformance layer.

## Scope: Included

- Discovery of available tools on a given surface
- Capability mapping: purpose, invocation, inputs, outputs, failures, constraints
- Production of a structured persistent capability registry
- Verification of tool behavior through execution
- Refresh protocol for keeping the registry current

## Scope: Excluded

- Codebase or team onboarding context. See [adaptive-onboarding](https://github.com/adaptive-interfaces/adaptive-onboarding)
- Observation and conformance discipline. Governed by [ACS](https://github.com/adaptive-interfaces/adaptive-conformance-specification)
- Post-discovery verification of agent understanding
- Normative judgments about tool design or quality

## Using ATD

Skills that depend on ATD include this in their preamble:

> This skill operates under Adaptive Tool Discovery (ATD).
> Apply ATD discovery and registry steps before executing any
> domain-specific actions below.

Read [`SKILL.md`](./SKILL.md) for the full specification.

## Testing ATD on your own tool surface

Clone the repo and add your own private scenarios under `evaluation/local/`.
That directory is gitignored and will never be committed or pushed.
Your proprietary test cases stay local while you pull upstream updates freely.

```shell
git clone https://github.com/adaptive-interfaces/adaptive-tool-discovery
cd adaptive-tool-discovery
mkdir -p evaluation/local/my-scenario
```

See [`evaluation/scenarios/`](./evaluation/scenarios/) for examples of how
scenarios are structured.

## Repository contents

```text
adaptive-tool-discovery/
  SKILL.md              the specification itself
  MANIFEST.toml         repository intent, scope, and role
  DECISIONS.md          design history and rationale
  LICENSE               MIT
  evaluation/
    rubric.md           grading criteria for conformance quality
    scenarios/          public test cases
    local/              gitignored, private test cases
```

## Developer

Format Markdown files with Prettier extension.
Then run:

```shell
npx markdownlint-cli2 "**/*.md"
uvx skillcheck SKILL.md --min-desc-score 75
```

## Get and Set GitHub Personal Access Token (Needed for Associated Scenario)

Before running the `github-api` scenario, create a GitHub personal access token
with at least repo read scope for the verification calls.
Hit ESC to escape the current Claude session and create a token.

1. Go to <https://github.com/settings/tokens>
2. Click **Generate new token** / **Generate new token (classic)**
3. Give it a name in the note field, e.g. `atd-scenario-test`
4. Set expiration (e.g. 7-30 days for a test token)
5. Select scopes:
   - `public_repo` - read access to public repositories (sufficient for read-only verification calls)
   - Add `repo` if you need to verify against private repositories
6. Click **Generate token**
7. Copy the token immediately; GitHub will not show it again

Set it in PowerShell before starting your Claude Code session:

```powershell
$env:GITHUB_TOKEN = "ghp_yourtoken_here"
```

Verify it is set:

```powershell
$env:GITHUB_TOKEN
```

## Set Up MCP Filesystem Server (Needed for Associated Scenario)

Before running the `mcp-filesystem-server` scenario,
install and start the MCP reference filesystem server locally.
Hit ESC to escape the current Claude session to complete setup.

### Install

Requires Node.js. Install the MCP filesystem server globally:

```shell
npm install -g @modelcontextprotocol/server-filesystem
```

### Start the server

The filesystem server requires at least one allowed directory.
Pass the path you want it to expose, for example:

```powershell
npx @modelcontextprotocol/server-filesystem C:\Repos
```

The server starts on stdio by default.
Note the server name and connection details for the scenario prompt.

### Verify it is running

In a separate terminal:

```powershell
npx @modelcontextprotocol/server-filesystem C:\Repos
```

### Configure Claude Code to connect

Add the server to your Claude Code MCP config
(typically `.claude/mcp_settings.json` in your home directory):

```pwsh
New-Item -ItemType Directory -Force -Path "$HOME\.claude"

Set-Content -Path "$HOME\.claude\mcp_settings.json" -Value '{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "C:\\Repos"]
    }
  }
}'

Get-Content "$HOME\.claude\mcp_settings.json"
```

The file created looks like:

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "C:\\Repos"]
    }
  }
}
```

Restart Claude Code after editing the config.
Verify the server is available in the session before running the scenario.

## Agent Options for Running Scenarios

**Claude Code** (recommended)
Included with a Claude Pro subscription ($20/month).
Terminal CLI that reads your repo, makes real HTTP calls, writes files,
and executes commands.
This is the right tool for running ATD scenarios end-to-end.

Install:

```shell
npm install -g @anthropic-ai/claude-code
```

**GitHub Copilot**
Good for inline code suggestions while editing.
Not suitable for running scenarios - lacks deep file system access
and long context handling.

**ChatGPT / OpenAI**
Separate product and ecosystem.
Skills in this repo are specified for Claude's format and behavior.
Cross-agent results are not comparable without adaptation.

## Run Scenarios (with Claude Code): General Instructions

Scenarios require an agent with file system access and the ability
to make live HTTP calls.
Claude.ai (chat) is useful for reviewing and refining skills
but cannot execute scenarios - it has no file system access
and cannot make real API calls.

From the repo root, start a Claude Code session:

```shell
claude
```

Select your authorization (e.g. Option 1) and it will open a browser to authenticate against your pro account.
After auth is successful, follow the terminal prompts, e.g. "Login successful. Press Enter to continue…".
Accept the notice (hit Enter).
When asked " Use Claude Code's terminal setup?" chose your option (e.g., 1 or Enter to confirm).
When asked "Is this a project you created or one you trust?", chose your option (e.g., 1 or Enter to confirm).

Then in the session, paste the commands provided below, e.g. something like this:

```shell
─────────────────────────────────────────────────────────────────────────
❯ Read SKILL.md then follow evaluation/scenarios/<scenario-name>/prompt.md
─────────────────────────────────────────────────────────────────────────
```

Read questions and answer or Hit Enter to accept default until done.
After each scenario finishes:

1. Copy the `tools` folder to the scenario folder.
2. Score performance against the rubric in `score.md`.

Note: Scenarios can be run in any order.
The files in `tools/` reflect the order they were generated, e.g.,
github-api, open-meteo-api, uv-cli, mcp-filesystem-server.

### Run Scenario 1: github-api (requires token)

```shell
Run: if ($env:GITHUB_TOKEN) { "token is set" } else { "token is NOT set" }

Read SKILL.md then follow evaluation/scenarios/github-api/prompt.md
```

### Run Scenario 2: mcp-filesystem-server

```shell
Show me your MCP configuration

What MCP servers and tools are available?

Read SKILL.md then follow evaluation/scenarios/mcp-filesystem-server/prompt.md
```

### Run Scenario 3: open-meteo-api (no auth)

```shell
Read SKILL.md then follow evaluation/scenarios/open-meteo-api/prompt.md
```

### Run Scenario 4: uv-cli (no auth)

```shell
Read SKILL.md then follow evaluation/scenarios/uv-cli/prompt.md
```

## Alternative: Explicitly Call ACS (not required)

```shell
Read https://raw.githubusercontent.com/adaptive-interfaces/adaptive-conformance-specification/main/SKILL.md
then read SKILL.md
then follow evaluation/scenarios/<scenario-name>/prompt.md
```

## License

MIT © 2026 [Adaptive Interfaces](https://github.com/adaptive-interfaces)
