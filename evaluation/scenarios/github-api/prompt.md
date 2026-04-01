# Scenario: GitHub REST API

Apply the Adaptive Tool Discovery (ATD) skill to map a bounded
subset of the GitHub REST API capabilities.

The OpenAPI spec is available at:
<https://github.com/github/rest-api-description>

Map only the following endpoint groups:

- repositories (create, get, list)
- issues (create, get, list, update)
- pull requests (get, list)

Authentication requires a GitHub personal access token
passed as a Bearer token in the Authorization header.

Produce a tool registry at `tools/github-api.toml`.
Mark authentication requirements explicitly.
Do not attempt live calls that would create or modify data.
Read-only verification calls are permitted.

After completing the registry:

1. Create or update `tools/manifest.toml` to include `tools/github-api.toml`
2. Copy `tools/github-api.toml` to `evaluation/scenarios/github-api/tools/`
3. Write a summary of the ATD run to `evaluation/scenarios/github-api/response.txt`
4. Score the output against `evaluation/rubric.md` and include the score table
   in `response.txt`
