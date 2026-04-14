---
version: 1.0.0
last_updated: 2026-04-15
schema_version: 1.0.0
---

# API Schema Generator Skill

**Description:** Translates backend API route signatures and models into deterministic OpenAPI format.

## Execution Contract
- **Guarantees:** Structured OpenAPI valid YAML/JSON chunks, fully typed models mapped from source.
- **Does NOT Guarantee:** Discovery of undocumented routes or perfect validation matching runtime conditions.

## I/O Paths (Standardized)
- **Input Artifact:** `artifacts/input/api-schema-generator/route_code.txt`
- **Output Artifact:** `artifacts/output/api-schema-generator/report.json`

## Instructions

1. Analyze the incoming backend code/models.
2. Formulate corresponding strict OpenAPI 3.x schema blocks for the paths, responses, and parameters.
3. Your output must strictly match the JSON structure described in `schema.json`.
4. Wrap the generated OpenAPI code accurately in the required fields.

## Hard Rejection Criteria (Validation Triggers)
Your output will be REJECTED via the deterministic validator if:
- The generated OpenAPI block itself fails external schema parsing downstream.

If you cannot extract the schema or validation fails, output the failure format:
```json
{
  "status": "failed",
  "reason": "<reason>",
  "retryable": true
}
```
