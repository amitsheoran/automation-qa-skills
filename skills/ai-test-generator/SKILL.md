---
name: ai-test-generator
description: Use an AI coding agent to generate automated tests directly from code, requirements, or an OpenAPI spec — then review and harden the output against hallucinated assertions.
---

# Purpose

Drive an AI agent to scaffold real, runnable automated tests from a source of truth (code,
user story, or API spec), with a disciplined review step so the tests assert *correct*
behavior — not whatever the code currently does.

## When to use

- Bootstrapping a test suite fast; filling coverage gaps; converting a spec into tests.

## When NOT to use

- Designing manual test cases for humans → `test-case-generator`.

## Inputs

- The source: code files, a requirement/story, or an OpenAPI/GraphQL schema.
- Target framework (e.g. `playwright`, `api-testing`/REST Assured, JUnit/pytest) and coverage goal.

## Workflow

1. **Pick the source of truth.** Generating *from code alone* risks encoding current bugs as
   "expected" — pair it with the requirement/spec so assertions reflect intended behavior.
2. Ask the agent to produce tests in the chosen framework, covering happy path + negative + boundary.
3. **Review every assertion** against the requirement — reject tautological or hallucinated ones.
4. **Run the tests.** Delete/fix any that fail for the wrong reason; confirm they actually catch a regression (mutate the code and see them fail).
5. Deduplicate against existing tests; align naming/structure with the repo.
6. Add the strongest cases to CI; note gaps the agent missed for manual follow-up.

## Output format

- Runnable test files in the target framework, plus a short **review note**:

```
Source of truth: user story US-123 + /orders OpenAPI
Framework: Playwright (TS)
Generated: 14 tests (8 happy, 4 negative, 2 boundary)
Reviewed: removed 2 tautological asserts; fixed 1 wrong expected status (was 200, spec says 201)
Verified: mutated handler → 11/14 failed as expected (3 gaps noted below)
Gaps for manual follow-up: concurrency, idempotency-key reuse
```

## Quality checklist

- [ ] Assertions checked against the requirement/spec, not just current code behavior.
- [ ] Tests actually run and pass.
- [ ] Verified to fail on a real regression (mutation check).
- [ ] No duplicated/hallucinated tests; naming matches the repo.
- [ ] Coverage gaps the AI missed are noted.
