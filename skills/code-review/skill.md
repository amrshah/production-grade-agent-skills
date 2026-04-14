---
version: 1.0.0
last_updated: 2026-04-15
schema_version: 1.0.0
---

# Code Review Skill

**Description:** Performs a comprehensive review of the provided codebase for logic errors, bugs, maintainability issues, and strict standards adherence.

## Execution Contract
- **Guarantees:** Structured issue detection, deterministic severity classification, code exact-location reporting.
- **Does NOT Guarantee:** Full programmatic correctness, absence of false negatives.

## I/O Paths (Standardized)
- **Input Artifact:** `artifacts/input/code-review/codebase.txt` (or `.tar` / diff file provided in context)
- **Output Artifact:** `artifacts/output/code-review/report.json`

## Instructions

1. Analyze the provided codebase thoroughly.
2. Identify any logical bugs, potential exceptions, null reference risks, or maintainability violations.
3. For each issue found, specify the exact file path and line number.
4. Explanations MUST be highly concise (maximum 200 characters).
5. Output MUST be strict JSON matching the schema provided in `schema.json` exactly.

## Hard Rejection Criteria (Validation Triggers)
Your output will be REJECTED via the deterministic validator layer if:
- > 30% of your reported issues contain vague or generalized explanations.
- You fail to provide the exact `location` formatting (`file_path:line_number`).
- Duplicate issues are reported.

If you cannot find any issues or fail validation, you must output a failure block strictly following the fallback failure schema:
```json
{
  "status": "failed",
  "reason": "<reason>",
  "retryable": true
}
```
