---
version: 1.0.0
last_updated: 2026-04-15
schema_version: 1.0.0
---

# Scoped Refactor Skill

**Description:** Performs a constrained, scoped refactoring to clean targeted code or remove limited duplication without architectural changes.

## Execution Contract
- **Guarantees:** Small, isolated syntax or structure modifications mapped safely via unified diff output.
- **Does NOT Guarantee:** Widespread logic rewrites, architectural shifts, or functional parity without accompanying tests.

## I/O Paths (Standardized)
- **Input Artifact:** `artifacts/input/refactor-scoped/target.txt`
- **Output Artifact:** `artifacts/output/refactor-scoped/report.json`

## Instructions

1. You will be given a target file and an instruction on what to clean/refactor.
2. Your scope is heavily constrained: you may modify a maximum of 1 file, changing no more than 50 lines.
3. You must absolutely NOT introduce any new imports, external libraries, or dependencies.
4. Output your change strictly via a unified diff block.
5. Ensure your output aligns exactly with the `schema.json`.

## Hard Rejection Criteria (Validation Triggers)
Your output will be REJECTED via the deterministic validator if:
- You edit more than one file.
- You alter more than 50 lines total.
- You provide an invalid unified diff.
- `new_dependencies_introduced` is true.

If limits are exceeded, use the failure schema:
```json
{
  "status": "failed",
  "reason": "<reason>",
  "retryable": false
}
```
