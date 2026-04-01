---
name: adaptive-tool-discovery
description: >
  Generates a persistent capability registry for external tool surfaces -
  MCP servers, APIs, CLIs, and SDKs - so agents can invoke tools correctly.
  Activate when an agent encounters an unfamiliar tool
  or an existing registry needs refresh.
---

# Adaptive Tool Discovery (ATD)

This skill operates under the Adaptive Conformance Specification (ACS).
Apply ACS discovery and conformance steps before executing any
domain-specific actions below.

ATD maps the capability surface of external tools -
what exists, what each tool does, how to invoke it correctly,
and what its failure modes are.
The output is a persistent tool capability registry
that any agent or subagent can load without re-running discovery.

## 1. Scope

### 1.1. What ATD covers

- Discovery of available tools on a given surface
- Capability mapping: purpose, invocation, inputs, outputs, failures, constraints
- Production of a structured, persistent capability registry
- Verification of tool behavior through execution
- Refresh protocol for keeping the registry current

### 1.2. What ATD does not cover

- Codebase or team onboarding context - see Adaptive Onboarding (AO)
- Observation and conformance discipline - governed by ACS
- Post-discovery verification of agent understanding
- Normative judgments about tool design or quality

### 1.3. What counts as a tool

A tool is any external capability surface an agent can invoke:

- MCP servers and their exposed functions
- REST or GraphQL APIs
- CLIs and their subcommands
- SDKs and their callable interfaces
- Any function registered in an agent's tool-use framework

ATD is not specific to any framework or protocol.
The spec applies wherever an agent must invoke an external capability
it has not previously mapped.

## 2. Workflow

Follows ACS workflow steps (Discover → Interpret → Select → Execute →
Validate → Conclude) with the following ATD-specific guidance at each step.

### 2.1. Discover

Enumerate the available tool surface:

- List all tools, endpoints, commands, or functions available
- For each tool, identify:
  - name and invocation path
  - declared input schema (required and optional parameters)
  - declared output schema
  - any documented constraints (auth, rate limits, scope)
- Sources to check:
  - MCP server manifest or schema endpoint
  - API documentation or OpenAPI spec (URL or file)
  - CLI `--help` output and man pages
  - SDK reference documentation

Record what you find.
Do not proceed until the surface is sufficiently enumerated
for the current task.

### 2.2. Interpret

For each tool, determine:

- What does this tool do? (one sentence)
- What inputs are required vs. optional?
- What does a successful response look like?
- What are the known failure modes and their meanings?
- Are there rate limits, authentication requirements, or scope restrictions?

Identify any tools that appear to duplicate functionality.
Note ambiguity explicitly - do not resolve it by assumption.

### 2.3. Select

Identify which tools are relevant to the current task or registry scope.
State the selection and the evidence that justifies it.
If tool purpose is ambiguous, say so.

### 2.4. Execute

Verify tool behavior through minimal, safe invocations:

- Use the smallest valid input that exercises the tool
- Do not use production data for verification calls
- Record actual inputs and outputs, not just declared schemas
- If a tool cannot be safely invoked, record that as a constraint

### 2.5. Validate

Check the registry entry before committing it:

- Declared schema matches observed behavior
- Failure modes are accurately described
- No unverified claims are present
- All fields defined in the registry schema are populated

If validation fails, record the failure.
Do not silently omit fields or substitute assumptions.

### 2.6. Conclude

Produce the capability registry.
State any tools that could not be fully mapped and why.
Copy the registry file to the scenario folder used for this run,
if one exists, so the output is preserved alongside the response.
Score the registry against the rubric defined in `evaluation/rubric.md`
and include the score inline in the response.
State what additional access or documentation would be needed
to complete those entries.

## 3. Capability Registry

### 3.0. Registry Requirements

- The registry is a persistent artifact, not a session-local record
- It must be stored at a stable, accessible location (file path or URL)
- Every agent and subagent that uses these tools loads the registry
  rather than re-running discovery
- The registry is versioned alongside the codebase or infrastructure it serves
- Each entry must be grounded in observed, verified behavior
- Unverified fields must be marked explicitly

### 3.1. Registry Location

The registry is owned by the team whose tools it describes.
It does not live in the ATD repo.

Use one file per tool surface, not one monolithic registry file.
A single file across many tool surfaces becomes slow and fragile to update (re-verification runs must search hundreds of lines to find and update
the correct entries, increasing the risk of missed or duplicate updates).

Recommended structure:

```text
tools/
  manifest.toml          # lists all registry files
  github-api.toml        # one file per tool surface
  mcp-filesystem-server.toml
  open-meteo.toml
  uv-cli.toml
```

The manifest declares all registries:

```text
# tools/manifest.toml
registries = [
  "tools/open-meteo.toml",
  "tools/github-api.toml",
  "tools/uv-cli.toml",
]
```

Agents load the manifest first, then load only
the registries relevant to their current task.
Re-verification targets a single surface file,
not the entire registry.
For tool surfaces shared across multiple codebases,
a dedicated registry repo with a stable URL is appropriate.
The location must be stable and accessible to all agents
that need to invoke the tools.

### 3.2. Registry Entry Schema

Each tool entry must include:

```toml
[[tools]]
name = ""              # stable identifier used to invoke the tool
purpose = ""           # what it does, one sentence
invoke = ""            # how to call it: endpoint, command, or function path
auth = ""              # authentication method; "none" if not required

[tools.inputs]
required = []          # required parameter names
optional = []          # optional parameter names
schema_url = ""        # URL or path to full input schema; omit if none

[tools.outputs]
success = ""           # what a successful response looks like
schema_url = ""        # URL or path to full output schema; omit if none

[tools.failures]
# known error modes and their meanings
# example: NOT_FOUND = "resource does not exist"

[tools.constraints]
rate_limit = ""        # requests per unit time; "unknown" if not verified
scope = ""             # access scope required; "none" if not restricted

[tools.meta]
verified = ""          # ISO date of last live verification (YYYY-MM-DD)
verified_by = ""       # agent or person who verified
notes = ""             # anything that does not fit above; omit if none
```

Unverified fields must be set to `"unverified"`, not omitted or guessed.

### 3.3. Registry Refresh

The registry must be refreshed when:

- a tool's schema or behavior changes
- a new tool is added to the surface
- a tool is removed or deprecated
- verification date exceeds the team's staleness threshold

**Automated refresh** is recommended via a scheduled CI workflow
with manual trigger support for unexpected tool changes.
Implementation is team-specific.

Example refresh behavior:

- Small changes (new optional parameter, description update):
  auto-commit with changelog entry
- Large changes (schema change, tool added or removed):
  open a PR for human review before merging

## 4. Error Handling

Follows ACS section 5 (Error Handling).

ATD-specific additions:

- If a tool cannot be invoked safely, record it as `status = "unverifiable"`
  with a reason; do not omit the entry
- If a tool's declared schema contradicts observed behavior,
  record both and flag the discrepancy explicitly
- If a tool is undocumented, record what was observed through execution;
  mark all inferred fields as `"inferred"`

## 5. Stopping Conditions

Follows ACS section 6 (Stopping Conditions).

ATD-specific additions:

- Stop if authentication or access cannot be obtained
- Stop if the tool surface cannot be enumerated with available credentials
- Report what tools were mapped, what were not, and what access is needed

## 6. Invocation by Other Skills

Skills that invoke ATD should state this in their preamble:

> This skill operates under Adaptive Tool Discovery (ATD).
> Apply ATD discovery and registry steps before executing any
> domain-specific actions below.

ATD governs tool capability mapping.
ACS governs observation and conformance discipline.
Both apply when a skill requires agents to use external tools
within an unfamiliar system.
