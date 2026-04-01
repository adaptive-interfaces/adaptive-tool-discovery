# Evaluation Notes: GitHub REST API

## What to look for

### Discovery

- Agent fetches the OpenAPI spec rather than relying on training knowledge
- Agent correctly scopes discovery to the three endpoint groups
- Does not attempt to map the entire API

### Capability accuracy

- Base URL correctly recorded as `https://api.github.com`
- Authentication correctly identified as Bearer token
- Required path parameters (owner, repo, issue_number) identified
- Pagination noted as a constraint

### Verification

- Agent makes at least one read-only call (e.g., get a public repo)
- Records actual response structure
- Rate limit and auth scope confirmed from response headers
  (X-RateLimit-Limit, X-OAuth-Scopes, X-Accepted-OAuth-Scopes)
  without requiring a separate docs lookup

### Registry

- auth field populated correctly
- rate_limit noted (5000 requests/hour for authenticated)
- Unverified fields marked

## Common failure modes

- Agent maps endpoints from training knowledge rather than the spec
- Agent attempts write operations during verification
- Agent maps all GitHub API endpoints rather than the scoped subset
- rate_limit left as "unverified" when it is documented
- Agent writes to `tools/registry.toml` instead of `tools/github-api.toml`
  (prompt not read carefully enough)
- Agent marks rate_limit as "unverified" when it can be confirmed
  from X-RateLimit-Limit response headers on any authenticated call
