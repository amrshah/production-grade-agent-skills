---
version: 1.0.0
last_updated: 2026-04-15
schema_version: 1.0.0
---

# PR Review Skill

**Description:** Performs a strict diff-based code review of pull requests or raw diff files.

## Execution Contract
- **Guarantees:** Identifies exactly what changed, evaluates isolated logic impact of the diff, proposes structured inline fixes.
- **Does NOT Guarantee:** Discovery of regressions outside the scope of the provided diff.

## I/O Paths (Standardized)
- **Input Artifact:** `artifacts/input/pr-review/input_diff.patch`
- **Output Artifact:** `artifacts/output/pr-review/report.json`

## Instructions

1. Analyze the provided `.patch` or diff.
2. Focus strictly on lines added (`+`) or affected by deletions (`-`).
3. For each actionable issue, provide the exact file and the scoped line number.
4. If no severe issues are present, approve the PR diff structurally.
5. Explanations must be concise. Max 200 character limitations apply.
6. Output MUST strictly match the schema provided in `schema.json`.

## Hard Rejection Criteria (Validation Triggers)
Your output will be REJECTED via the deterministic validator if:
- You report issues on lines NOT present in the diff.
- You provide generalized advice ("consider rewriting this block") instead of exact fixes.
- The `location` format `file:line` is broken.

If you fail validation, output the failure fallback schema:
```json
{
  "status": "failed",
  "reason": "<reason>",
  "retryable": true
}
```
