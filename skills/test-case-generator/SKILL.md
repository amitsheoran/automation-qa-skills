---
name: test-case-generator
description: Generate enterprise-grade, automation-ready manual test cases (positive, negative, boundary, and edge) from a requirement, user story, or feature description.
---

# Purpose

Turn a requirement or user story into a complete, review-ready set of test cases covering
happy path, negative, boundary, and edge scenarios — structured so they can be executed
manually or handed to an automation skill.

## When to use

- A new feature, user story, or acceptance criteria needs test coverage.
- You need a structured test suite before automation.

## When NOT to use

- For a single API contract → use `api-testing`.
- For test *strategy* across a release → use `test-strategy` / `test-plan`.

## Inputs

- Requirement / user story / acceptance criteria.
- (Optional) UI details, roles/permissions, data constraints, environments.

## Workflow

1. Restate the requirement in one line and list the acceptance criteria.
2. Identify equivalence classes and boundaries for every input.
3. Derive scenarios: **positive**, **negative**, **boundary**, **edge/error**, **security/permission**.
4. For each scenario write a test case with the fields below.
5. Assign **Priority** (P1–P4) and **Severity**, and tag **Automation Scope** (Yes/No/Candidate).
6. Flag gaps/questions where the requirement is ambiguous.

## Output format

A Markdown table, one row per test case:

| ID | Title | Preconditions | Test Data | Steps | Expected Result | Priority | Severity | Type | Automation Scope |
|----|-------|---------------|-----------|-------|-----------------|----------|----------|------|------------------|
| TC-001 | ... | ... | ... | 1. ... 2. ... | ... | P1 | Critical | Positive | Yes |

Always include: **Preconditions, Test Data, Steps, Expected Result, Priority, Severity,
Automation Scope, and Boundary / Negative / Edge cases.**

## Quality checklist

- [ ] Every acceptance criterion maps to ≥1 test case.
- [ ] At least one negative and one boundary case per input field.
- [ ] Steps are atomic and reproducible; expected results are unambiguous.
- [ ] IDs are unique and stable; priorities/severities assigned.
- [ ] Ambiguities surfaced as open questions.

## Example

See `examples/login.md` for a full worked example.
