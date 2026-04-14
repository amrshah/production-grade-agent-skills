---
version: 1.0.0
last_updated: 2026-04-15
schema_version: 1.0.0
---

# Test Generation Skill

**Description:** Generates deterministic unit and edge case tests for a given target code file.

## Execution Contract
- **Guarantees:** Outputs structured, compilable test code verifying happy paths, edge cases, and failure modes using mock isolated dependencies. 
- **Does NOT Guarantee:** 100% test coverage or that tests will logically succeed without human verification of the mocked states.

## I/O Paths (Standardized)
- **Input Artifact:** `artifacts/input/test-generation/target_code.txt`
- **Output Artifact:** `artifacts/output/test-generation/report.json`

## Instructions

1. Analyze the input code structure and identify required test bounds.
2. Generate minimum 5 tests spanning `happy_path`, `edge_case`, and `failure_mode`.
3. Provide the full code block for each test.
4. If testing relies on mocked dependencies, explicitly list them.
5. Output MUST match the exact schema detailed in `schema.json`.

## Hard Rejection Criteria (Validation Triggers)
Your output will be REJECTED via the deterministic validator if:
- You output fewer than 5 tests.
- You fail to include edge case coverage.
- The tests are pseudocode or do not contain valid assertions.

If you cannot fulfill these constraints, fallback to the failure output:
```json
{
  "status": "failed",
  "reason": "<reason>",
  "retryable": true
}
```
