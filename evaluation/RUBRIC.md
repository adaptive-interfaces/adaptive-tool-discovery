# ATD Conformance Rubric

This rubric evaluates whether an agent execution conforms to the
Adaptive Tool Discovery (ATD) skill specification.

Evaluation is based on the registry entries produced
and the behavioral constraints defined in `SKILL.md`.

All judgments must be grounded in the submitted registry and execution record.
No credit is given for implied or assumed behavior.

## Evaluation Dimensions

Each dimension is scored independently.

### 1. Surface Enumeration

Question: Were available tools sufficiently discovered and listed?

Full (2)

- All available tools on the surface are identified
- Names, invocation paths, and declared schemas are recorded
- Sources consulted are explicit (manifest, docs URL, help output)

Partial (1)

- Some tools identified but enumeration is incomplete
- Sources partially documented

None (0)

- No meaningful enumeration
- Tools assumed without inspection

### 2. Capability Accuracy

Question: Was each tool correctly understood?

Full (2)

- Purpose, inputs, outputs, and constraints accurately described
- Required vs optional parameters correctly identified
- Failure modes recorded

Partial (1)

- Minor inaccuracies or omissions
- Some fields missing or weakly justified

None (0)

- Misinterpretation of tool behavior
- Incorrect assumptions about inputs or outputs

### 3. Verification

Question: Was tool behavior confirmed through execution?

Full (2)

- Minimal safe invocations performed for each tool
- Actual inputs and outputs recorded
- Discrepancies between declared and observed behavior noted

Partial (1)

- Some tools verified, others not
- Verification described but incompletely

None (0)

- No verification performed
- Registry entries based on documentation alone without execution

### 4. Registry Completeness

Question: Does each registry entry satisfy the required schema?

Full (2)

- All required fields present and populated
- Unverified fields explicitly marked as `"unverified"`
- No fields omitted or guessed

Partial (1)

- Most fields present; minor omissions
- Some unverified fields not marked

None (0)

- Required fields missing
- Fields guessed or fabricated

### 5. Minimality and Discipline

Question: Is each entry limited to what is supported by evidence?

Full (2)

- No speculative or unobserved content
- Entries are minimal and valid

Partial (1)

- Minor overgeneration or redundancy

None (0)

- Includes unsupported or fabricated content
- Fields added without observed basis

### 6. Validation Integrity

Question: Was each registry entry checked before committing?

Full (2)

- Validation explicitly performed and described
- Declared schema matches observed behavior
- No unverified claims present

Partial (1)

- Validation attempted but incomplete

None (0)

- No validation step
- Errors or discrepancies ignored

### 7. Transparency and Traceability

This dimension is cross-cutting.
It evaluates the traceable relationship between evidence and registry entries
across all tools discovered, not a single entry in isolation.

Question: Can each registry entry be traced back to observed evidence?

Full (2)

- All entries traceable to observed or executed evidence
- Uncertainty and limits explicitly stated

Partial (1)

- Partial traceability
- Some entries not clearly justified

None (0)

- Entries cannot be traced to evidence
- Hidden assumptions present

## Scoring

Each dimension is scored:

- 2 = Full
- 1 = Partial
- 0 = None

Maximum score: 14

## Evaluation Outcomes

- Conformant (12–14)
  - Strong enumeration, accurate capability mapping, full registry
  - No structural violations

- Partially Conformant (7–11)
  - Some gaps or minor inconsistencies
  - No critical violations

- Non-Conformant (0–6)
  - One or more critical failures:
    - tools assumed without inspection
    - fabricated capability descriptions
    - registry entries without verification
    - silent omission of required fields

## Critical Violations (Automatic Non-Conformance)

Any of the following results in a Non-Conformant rating regardless of score:

- No evidence of tool surface enumeration
- Fabricated or hallucinated tool names, schemas, or behavior
- Registry entries not grounded in observed or executed evidence
- Required fields omitted without explanation
- Unverified fields not marked as `"unverified"`

## Notes

- Evaluation is based only on what is explicitly shown.
- Missing registry fields are treated as missing behavior.
- Partial correctness does not compensate for structural violations.
