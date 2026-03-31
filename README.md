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

## License

MIT © 2026 [Adaptive Interfaces](https://github.com/adaptive-interfaces)
