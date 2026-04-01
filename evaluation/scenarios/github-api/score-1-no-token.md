# Score 1: github-api (without token)

- Date: 2026-03-31
- Response: response.txt
- Registry: tools/registry.toml
- Evaluator: Denise M. Case

## Results

| Dimension                        | Score | Notes                                                                                          |
| -------------------------------- | ----- | ---------------------------------------------------------------------------------------------- |
| 1. Surface Enumeration           | 2     | 9 endpoints mapped across 3 groups from docs.github.com and OpenAPI spec; correctly scoped     |
| 2. Capability Accuracy           | 2     | Auth, required path params, pagination, and mutually exclusive params correctly identified     |
| 3. Verification                  | 1     | No token available; all entries marked "unverified"; re-verification calls documented in notes |
| 4. Registry Completeness         | 2     | All required fields present; unverified fields explicitly marked; re-verification calls noted  |
| 5. Minimality and Discipline     | 2     | Scoped to 9 endpoints per prompt; did not map entire GitHub API                                |
| 6. Validation Integrity          | 2     | Unverified state explicitly reported; schema sourced from docs; no silent omissions            |
| 7. Transparency and Traceability | 2     | No token clearly stated; all entries traceable to docs.github.com; gaps explicitly declared    |

Total: **13 / 14 Conformant**

## Critical Violations

None.

## Notes

Strong run under constrained conditions: no GitHub token available in the environment.
The agent correctly handled the constraint by marking all entries `verified = "unverified"`
rather than fabricating verification results or omitting the field.
Each entry includes a concrete re-verification call in the notes field
so a future run with a token can complete the registry cleanly.

Verification dimension scored Partial (1) rather than Full (2) because no live calls
were made.
This is an expected and acceptable result given the environment constraint,
not a conformance failure.
The agent's handling of the constraint is itself evidence of correct ATD behavior:
stop rather than guess, record what is known, state what is needed to proceed.

Notable accuracy: labels and assignees on github-issues-update correctly identified
as replacement arrays rather than additive, a non-obvious constraint that would cause
real bugs if missed.

To complete verification: set GITHUB_TOKEN in the environment and rerun the scenario.
Read-only calls are safe against any public repo (e.g. github/docs).
Write endpoints (create, update) should remain unverified unless a dedicated test repo
is available.
