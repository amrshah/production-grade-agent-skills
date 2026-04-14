---
version: 1.0.0
last_updated: 2026-04-15
schema_version: 1.0.0
---

# Root Cause Analysis Skill

**Description:** Performs structured analysis of stack traces, error reports, and problematic code paths to determine root causes.

## Execution Contract
- **Guarantees:** Identifies formal competing hypotheses, maps out specific validation steps, and calculates a structured confidence score.
- **Does NOT Guarantee:** Generating a functioning fix (handled by other skills) or successfully reproducing the bug environment.

## I/O Paths (Standardized)
- **Input Artifact:** `artifacts/input/root-cause-analysis/error_trace.txt`
- **Output Artifact:** `artifacts/output/root-cause-analysis/report.json`

## Instructions

1. Review the input error logs, traces, or behavioral anomaly reports.
2. Build minimum two distinct hypotheses for why the bug occurs.
3. Determine concrete validation steps required to prove/disprove each hypothesis.
4. Select the most likely cause based on the evidence.
5. Provide a strict confidence score between 0.0 and 1.0.
6. Output MUST strictly match the schema detailed in `schema.json`.

## Hard Rejection Criteria (Validation Triggers)
Your output will be REJECTED via the deterministic validator if:
- You propose no competing hypotheses.
- You provide no actionable validation steps.
- You provide a confidence score without substantial deductive evidence matching the error trace.

If validation limits trigger, output the failure schema:
```json
{
  "status": "failed",
  "reason": "<reason>",
  "retryable": true
}
```
