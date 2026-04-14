---
version: 1.0.0
last_updated: 2026-04-15
schema_version: 1.0.0
---

# Security Audit Skill

**Description:** Performs a strict security audit of the provided codebase identifying vulnerabilities mapping strictly to OWASP and standard CVE references.

## Execution Contract
- **Guarantees:** Identifies security antipatterns, hardcoded secrets, injection risks, and known dependency vulnerabilities if explicit context is provided. Matches issues to OWASP.
- **Does NOT Guarantee:** A fully penetration-tested environment or 100% absence of zero-days.

## I/O Paths (Standardized)
- **Input Artifact:** `artifacts/input/security-audit/codebase.txt`
- **Output Artifact:** `artifacts/output/security-audit/report.json`

## Instructions

1. Audit the source code specifically looking for vulnerabilities (A01-A10 OWASP Top 10).
2. Look for SQLi, XSS, Broken Auth, IDOR, SSRF, Deserialization, etc.
3. You must provide an EXACT mapping to an OWASP category.
4. Generics such as "sanitize input" are strictly forbidden. You must provide the actionable patch in the `fix` field.
5. Output MUST strictly match the schema provided in `schema.json`.

## Hard Rejection Criteria (Validation Triggers)
Your output will be REJECTED via the deterministic validator if:
- You omit the exact OWASP mapping.
- The `fix` provided is a generic instructional phrase rather than an actionable patch.

If validation limits trigger, you must fall back to the failure schema:
```json
{
  "status": "failed",
  "reason": "<reason>",
  "retryable": true
}
```
