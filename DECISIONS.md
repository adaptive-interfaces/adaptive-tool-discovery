# DECISIONS.md

Design history and rationale.
Records why the skill is the way it is for
contributors, future maintainers, and dependent skills.

## Decision 1. Scope: tool capability mapping only

ATD covers discovery, mapping, and registry maintenance of external tools.
It does not cover codebase or team onboarding context,
observation and conformance discipline, or post-discovery verification
of agent understanding.

### Rationale 1

These are distinct concerns with distinct workflows.
Observation and conformance discipline is the responsibility of ACS,
which ATD invokes as its foundational layer.
Codebase and team onboarding context is the responsibility of
Adaptive Onboarding (AO).
ATD's scope must stay narrow enough to be reusable across any team
or tool surface without requiring codebase knowledge.

## Decision 2. Tool definition: framework-agnostic

A tool is defined as any external capability surface an agent can invoke:
MCP servers, REST APIs, CLIs, SDKs, or any registered agent function.
ATD is not specific to MCP or any other protocol.

### Rationale 2

Specificity to a single protocol would make ATD dated quickly
and exclude legitimate use cases.
The capability mapping problem is the same regardless of invocation mechanism.
Teams using MCP servers today may use different protocols tomorrow.
The spec should remain valid across that transition.

## Decision 3. Persistent registry as primary output

ATD produces a persistent capability registry stored at a stable location
(file path or URL), not a session-local record.
The registry is owned by the team whose tools it describes,
not by the ATD repo.

### Rationale 3

For teams of significant size (25+ people and agents),
having every agent re-run discovery independently is wasteful
and produces inconsistent results.
A shared persistent registry means discovery runs once,
is verified once, and is loaded by all agents and subagents.
Storing the registry in the team's own codebase or infrastructure
keeps it close to the tools it describes and under the team's version control.

## Decision 4. Refresh via scheduled GitHub Actions

The registry is refreshed on a schedule (e.g., weekly)
via a GitHub Actions workflow.
Teams might use auto-commit for small changes and a PR for large changes.

### Rationale 4

Tools change infrequently but do change.
A scheduled action catches drift without requiring manual intervention.
The small/large change branching ensures humans review significant
schema or behavioral changes before they propagate to all agents,
while keeping minor updates from creating unnecessary review burden.

## Decision 5. Unverified fields use explicit marker

Registry fields that cannot be verified must be set to `"unverified"`,
not omitted or guessed.

### Rationale 5

An agent reading a registry entry must be able to distinguish
between a field that was verified and found to be empty
versus a field that was never checked.
Omission is ambiguous.
Guessing violates the ACS evidence-first principle.
An explicit `"unverified"` marker preserves the integrity of the registry
and signals to the agent that additional discovery may be needed.

## Decision 6. License: MIT

MIT across all `adaptive-interfaces` repos.

### Rationale 6

Organizations need to be able to use and adapt skills
internally without legal friction.
MIT is OSI-approved, universally recognized by legal teams, and
imposes no obligations on derivative works.

## Decision 7. MANIFEST.toml: adaptive-interfaces schema

Uses `schema = "adaptive-interfaces-manifest-1"`, defined in
[adaptive-interfaces-manifest-1.md](https://github.com/adaptive-interfaces/.github/blob/main/schemas/adaptive-interfaces-manifest-1.md),
an adaptation of
[SE_MANIFEST.toml](https://github.com/structural-explainability/spec-se/blob/main/manifests/se-manifest-1.md).

### Rationale 7

The adaptive-interfaces manifest borrows the pattern
(explicit includes/excludes, declarative intent) without the schema binding.

## See Also

- [Markdown Architectural Decision Records](https://adr.github.io/madr/)
- [Adaptive Conformance Specification](https://github.com/adaptive-interfaces/adaptive-conformance-specification)
