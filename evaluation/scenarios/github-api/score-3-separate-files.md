# Score 3: github-api (separate files, with token)

- Date: 2026-03-31
- Response: response-2-with-token.md
- Registry: tools/registry.toml (copied to evaluation/scenarios/github-api/tools/)
- Evaluator: Denise M. Case

## Results

| Dimension                        | Score | Notes                                                                                               |
| -------------------------------- | ----- | --------------------------------------------------------------------------------------------------- |
| 1. Surface Enumeration           | 2     | 9 endpoints mapped across 3 groups; sources explicit (docs.github.com, github/rest-api-description) |
| 2. Capability Accuracy           | 2     | Auth, required path params, pagination, optional params, and failure modes correctly identified     |
| 3. Verification                  | 2     | 6 read-only GET endpoints verified live; actual HTTP status and error body shapes recorded          |
| 4. Registry Completeness         | 2     | All required fields present; mutation endpoints marked "unverified"; no silent omissions            |
| 5. Minimality and Discipline     | 2     | Scoped to 9 endpoints per prompt; did not map entire GitHub API                                     |
| 6. Validation Integrity          | 2     | Declared schema matches observed behavior for verified endpoints; unverified state explicit         |
| 7. Transparency and Traceability | 2     | All read-only entries traced to specific live calls; unverified entries cite schema source          |

Total: **14 / 14 Conformant**

## Critical Violations

None.

## Notes

Third run of the github-api scenario — first with token and per-surface file structure.
Six read-only GET endpoints verified live against octocat/Hello-World (HTTP 200 each).
Error shapes verified: 404 and 401 body structures confirmed from live calls.
Rate limit confirmed from X-RateLimit-Limit response header: 5000 req/hour.
Auth scope confirmed from X-OAuth-Scopes / X-Accepted-OAuth-Scopes headers.

Mutation endpoints (github-repos-create, github-issues-create, github-issues-update)
remain verified = "unverified" per scenario constraint — write calls not permitted
without a dedicated test repo.

Notable improvement over runs 1 and 2:
the agent correctly rejected the first write attempt to
tools/registry.toml (user rejected), then re-read the scenario and produced
a correctly named per-surface file.
This demonstrates the scenario prompt update was effective.

The agent also self-scored against the rubric inline and copied the registry
to the scenario folder unprompted, both good behaviors worth preserving
in the prompt.md as explicit instructions for future runs.
