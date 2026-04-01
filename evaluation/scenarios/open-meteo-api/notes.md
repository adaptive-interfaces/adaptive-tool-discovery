# Evaluation Notes: Open-Meteo API

## What to look for

### Discovery

- Agent fetches the OpenAPI spec or documentation URL
- Does not assume parameter names from general knowledge

### Capability accuracy

- `latitude` and `longitude` correctly identified as required
- At least one `hourly` or `daily` variable documented
- Base URL correctly recorded as `https://api.open-meteo.com/v1/forecast`

### Verification

- Agent makes at least one live call with minimal inputs
- Records actual response structure, not just declared schema
- Failure modes verified live (e.g. HTTP 400 with missing timezone for daily variables)
  not just inferred from documentation

### Registry

- All required schema fields populated
- Unverified fields marked as "unverified"
- No hallucinated parameters (common failure: inventing an API key field)

## Common failure modes

- Agent assumes authentication is required (it is not)
- Agent maps all parameters exhaustively rather than selectively
- Agent records declared schema without verifying via execution
- Agent writes to `tools/registry.toml` instead of `tools/open-meteo.toml`
  (prompt not read carefully enough)
- Agent marks rate_limit as "unverified" when the constraint
  (no documented limit for non-commercial use) is itself the accurate finding
