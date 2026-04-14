# AI Software Factory: Skill Layer (L1)

This repository contains the foundational Skill Layer (L1) components for the AI Software Factory. Unlike traditional LLM prompts, these skills are designed as strict, deterministic-format machine-validated execution units.

These components form the reliable base required before validation and retry (L2), orchestration (L3), and autonomous factory operations (L4) can safely occur.

## Architecture of a Skill

Each skill operates as a controlled function. A skill is composed of two exact files:

1. `skill.md`: Contains the execution instructions, versioning metadata, mapped constraints, and strict rejection rules.
2. `schema.json`: Represents the machine-validated JSON Schema contract governing the exact format the LLM must generate.

## Standardized Artifact I/O

Skills do not assume passing in-memory strings between stages. Every skill reads from and writes to standardized artifact paths. Upstream stages must provide artifacts named specifically to prevent chaotic pipelines.

- **Global Output Point:** `/artifacts/output/<skill_name>/report.json`

### Allowed Input Artifacts
- **code-review:** `/artifacts/input/code-review/codebase.tar` OR `/artifacts/input/code-review/files.json`
- **pr-review:** `/artifacts/input/pr-review/diff.patch`
- **security-audit:** `/artifacts/input/security-audit/codebase.tar`
- **test-generation:** `/artifacts/input/test-generation/target_code.md`
- **root-cause-analysis:** `/artifacts/input/root-cause-analysis/error_trace.log`
- **refactor-scoped:** `/artifacts/input/refactor-scoped/target_file.md`
- **api-schema-generator:** `/artifacts/input/api-schema-generator/routes.json` OR `/artifacts/input/api-schema-generator/source.tar`
- **docker-ci-review:** `/artifacts/input/docker-ci-review/ci_files.tar`

**Strict Input Exclusivity:** Where multiple input formats are allowed (e.g., `.tar` OR `.json`), if upstream passes BOTH formats in the input directory, the skill layer validator will immediately hard-reject the run to prevent ambiguity.

## Execution Boundaries

L1 components must run bounded heavily to isolate drift unpredictability. 
- **Global Retry Limit:** Maximum of `3` attempts (`max_attempts: 3`). If `attempt` hits 3 and fails, the stage is hard-aborted.
- **Context Size Caps:** 
  - Max `5` files per execution run.
  - Max `1000` lines total in the provided scope.
  - Max `50KB` payload size to avoid attention degradation.

Large repositories exceeding these bounds must be dynamically windowed and fed piecemeal by upstream L3 orchestrators.

## Strict Output Contracts & Schema Validation

**Every executed LLM output MUST sequentially pass its matching `schema.json` validator.** Failure to match automatically halts progression and issues a failure artifact instance. This `{ "status": "...", "data": { ... } }` wrapper standardizes outputs across entirely all L1 skills.

### Success Contract
Every valid success must wrap its operational data logically under the `data` key object payload:

```json
{
  "status": "success",
  "data": { 
     "issues": [],
     "summary": {}
  }
}
```

### Failure Contract
Rejection limits and structural failure handlers must track and append context for L2 retries:

```json
{
  "status": "failed",
  "reason": "missing_location",
  "retryable": true,
  "stage": "code-review",
  "attempt": 1
}
```

## Observability Hook

All runs natively route basic execution metrics explicitly to downstream logger sinks for scaling and factory calibration evaluation. (If multiple attempts happen, tracking handles deduplication):

```json
{
  "skill": "code-review",
  "status": "success",
  "duration_ms": 1200,
  "retry_count": 1
}
```

## Non-Guarantees
While L1 components operate predictably via formatting, they are **NOT explicitly proven to guarantee runtime correctness**. Standard bounds expected:
- Cannot guarantee zero false negatives / may miss vulnerabilities entirely.
- May synthesize structurally invalid patches.
- *Strict assumption is required*: Secondary layers (L2) natively validate behavioral parity outside of structure limitations via test execution tools.

## Core L1 Skills

The current authorized L1 skills in this repository are:

- **`code-review`**: Locates logic defects with deterministic severity and exact line mappings.
- **`pr-review`**: Validates pull request patch diffs and identifies isolated structural risks.
- **`security-audit`**: Identifies vulnerabilities enforcing strict, required OWASP 2021 categorizations and actionable patches.
- **`test-generation`**: Generates exhaustive happy path, failure mode, and edge case compilable tests.
- **`root-cause-analysis`**: Builds formal hypotheses, necessary validation steps, and logical confidence intervals for stack traces.
- **`refactor-scoped`**: Outputs strict unified diffs for cleanup functions, strictly capped to <50 line iterations.
- **`api-schema-generator`**: Outputs deterministic OpenAPI schema chunks from implementation code schemas.
- **`docker-ci-review`**: Uncovers pipeline structural flaws and caching inefficiencies with discrete fixes.
