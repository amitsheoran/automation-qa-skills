---
name: requirement-analysis
description: Analyze a requirement or user story for testability, ambiguity, missing acceptance criteria, and edge cases before test design begins.
---

# Purpose

Review requirements *before* writing test cases to catch ambiguity, gaps, and untestable
statements early — the cheapest place to find defects.

## When to use

- A new story/requirement arrives; grooming/refinement sessions.
- Before `test-case-generator` — to sharpen the input.

## When NOT to use

- The requirement is already crisp and you're ready to write cases → go straight to `test-case-generator`.

## Inputs

- The requirement, user story, spec, or acceptance criteria.
- (Optional) mockups, data model, related requirements.

## Workflow

1. Restate each requirement in one testable sentence.
2. Check each against **INVEST** / testability: is it specific, measurable, unambiguous?
3. List **ambiguities** and **open questions** (who, when, limits, error handling).
4. Derive **missing acceptance criteria** and **edge cases** (empty, max, concurrency, permissions, i18n).
5. Note **dependencies, assumptions, and NFRs** (performance, security, accessibility).
6. Produce a **testability verdict** per requirement: Ready / Needs clarification.

## Output format

```
Requirement: <id / summary>
Testable?    Yes / No
Ambiguities:
  - <question needing an answer before testing>
Missing acceptance criteria:
  - <criterion to add>
Edge cases to cover:
  - <case>
NFRs / dependencies:
  - <perf / security / a11y / dependency>
Verdict: Ready to test  |  Needs clarification (blocking: <items>)
```

## Quality checklist

- [ ] Each requirement restated as a testable statement.
- [ ] Ambiguities phrased as answerable questions.
- [ ] Missing criteria and edge cases listed.
- [ ] NFRs and dependencies considered.
- [ ] Clear Ready / Needs-clarification verdict.
