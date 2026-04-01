# Score: github-api (run 4)

- Date: 2026-03-31
- Response: evaluation/scenarios/github-api/response.txt
- Registry: evaluation/scenarios/github-api/tools/github-api.toml
- Evaluator: Denise M. Case

## Results

Total: **14 / 14 Conformant**

See response.txt for inline self-score table and evidence.

## Notes

Fourth run. First clean run with per-surface file and token.
Agent correctly wrote to `tools/github-api.toml` and updated `tools/manifest.toml`.
Manifest now lists all three surfaces: uv-cli, open-meteo, github-api.

Six read-only GET endpoints verified live against octocat/Hello-World.
Rate limit confirmed from X-RateLimit-Limit header: 5000 req/hour.
Auth scope confirmed from X-OAuth-Scopes / X-Accepted-OAuth-Scopes headers.
Error shapes (404, 401) confirmed from live calls.

Mutation endpoints remain verified = "unverified" per scenario constraint.
Safe re-verification requires a dedicated test repo.
