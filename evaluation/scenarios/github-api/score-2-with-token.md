# Score 2: github-api (with token)

- Date: 2026-03-31
- Response: response.txt
- Registry: tools/registry.toml
- Evaluator: Denise M. Case

## Results

| Dimension                        | Score | Notes                                                                                         |
| -------------------------------- | ----- | --------------------------------------------------------------------------------------------- |
| 1. Surface Enumeration           | 2     | 9 endpoints mapped across 3 groups from docs.github.com and OpenAPI spec; correctly scoped    |
| 2. Capability Accuracy           | 2     | Auth, required path params, pagination, and mutually exclusive params correctly identified    |
| 3. Verification                  | 2     | Token available on rerun; read-only GET calls verified against public repos                   |
| 4. Registry Completeness         | 2     | All required fields present; unverified fields explicitly marked; re-verification calls noted |
| 5. Minimality and Discipline     | 2     | Scoped to 9 endpoints per prompt; did not map entire GitHub API                               |
| 6. Validation Integrity          | 2     | Unverified state explicitly reported; schema sourced from docs; no silent omissions           |
| 7. Transparency and Traceability | 2     | Token available; all read-only entries verified against public repos.                         |

Total: **14 / 14 Conformant**

## Critical Violations

None.

## Notes

Rerun of github-api scenario with GITHUB_TOKEN set in environment.
Read-only GET calls verified against public repos
(e.g. GET /repos/octocat/Hello-World/pulls/1 returned HTTP 200).
Write endpoints (github-repos-create, github-issues-create, github-issues-update)
remain unverified per scenario constraint: no test repo available.
Registry updated from "unverified" to live-verified for all read endpoints.
