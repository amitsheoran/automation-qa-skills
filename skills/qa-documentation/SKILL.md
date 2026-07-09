---
name: qa-documentation
description: Produce clear QA documentation — test plans, test summary reports, release sign-off notes, runbooks, and knowledge-base articles — in a consistent, review-ready format.
---

# Purpose

Generate professional QA documentation artifacts that stakeholders can read and act on:
test summary reports, release sign-offs, runbooks, and process docs.

## When to use

- End of a test cycle (summary report), a release gate (sign-off), or documenting a process.

## When NOT to use

- Planning coverage before testing → `test-plan` / `test-strategy`.

## Inputs

- Doc type (summary report, sign-off, runbook, KB article).
- Test results / metrics, scope, environment, and audience.

## Workflow

1. Confirm the **document type** and **audience** (dev, PM, leadership).
2. Pull the relevant facts: scope, executed/passed/failed/blocked counts, open defects by severity.
3. Fill the matching template below.
4. Lead with a **verdict/summary**; keep detail in sections.
5. State risks and a clear recommendation (Go / No-Go / Go-with-caveats).

## Output format — Test Summary Report

```
# Test Summary Report — <feature/release> (<date>)

Verdict: PASS with 2 known low-severity issues

Scope:        <what was / wasn't tested>
Environment:  <build, env, data set>
Execution:    Total 120 · Passed 112 · Failed 3 · Blocked 5
Defects:      P1: 0 · P2: 1 · P3: 4  (see linked tickets)
Coverage:     Requirements covered 18/18 · Automation 74%
Risks:        <residual risk + mitigation>
Recommendation: Go / No-Go / Go with caveats — <one line why>
```

## Quality checklist

- [ ] Verdict/recommendation is stated up front.
- [ ] Numbers reconcile (total = passed + failed + blocked).
- [ ] Open defects linked with severities.
- [ ] Scope explicitly lists what was NOT tested.
- [ ] Written for the stated audience (no unexplained jargon for non-QA readers).
