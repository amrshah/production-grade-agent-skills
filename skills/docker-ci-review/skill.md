---
version: 1.0.0
last_updated: 2026-04-15
schema_version: 1.0.0
---

# Docker & CI Review Skill

**Description:** Performs best-practice evaluations and structural checks of Dockerfiles and CI pipelies (e.g. GitHub Actions).

## Execution Contract
- **Guarantees:** Identifies common misconfigurations, caching inefficiencies, and container privileges logic errors mapped to explicit line fixes.
- **Does NOT Guarantee:** Environment compatibility or zero-day container escapes.

## I/O Paths (Standardized)
- **Input Artifact:** `artifacts/input/docker-ci-review/ci_files.tar`
- **Output Artifact:** `artifacts/output/docker-ci-review/report.json`

## Instructions

1. Analyze the provided Dockerfiles or CI/CD pipelines.
2. Check for root privileges, bad layer ordering, bloat, or caching missed opportunities.
3. Suggest explicit, actionable syntax fixes. Avoid vague performance advice.
4. Output MUST adhere strictly to the format defined in `schema.json`.

## Hard Rejection Criteria (Validation Triggers)
Your output will be REJECTED via the deterministic validator if:
- You give generic suggestions without concrete diff replacements.

If validation is triggered or inputs are invalid, use the failure output:
```json
{
  "status": "failed",
  "reason": "<reason>",
  "retryable": true
}
```
