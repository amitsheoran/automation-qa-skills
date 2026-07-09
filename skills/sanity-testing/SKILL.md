---
name: sanity-testing
description: Design a narrow, focused sanity check to verify a specific fix or change works and hasn't obviously broken closely related functionality — without full regression.
---

# Purpose

After a targeted change or bug fix, quickly confirm the fix works and nearby functionality
still behaves — a focused, unscripted-but-deliberate check.

## When to use

- A specific bug was fixed or a small change shipped.
- You need confidence fast, before deciding on full regression.

## When NOT to use

- Verifying overall build stability → `smoke-testing`.
- Broad coverage before release → `regression-testing`.

## Inputs

- What changed (the fix/ticket) and its blast radius (related features/data).

## Workflow

1. Confirm the **fix itself** works (the original defect no longer reproduces).
2. Identify **adjacent functionality** that shares code/data with the change.
3. Run a **few targeted checks** on those adjacent areas.
4. Verify no **obvious regressions** in the immediate workflow.
5. Recommend: **Proceed** to deeper testing / release, or **escalate** to full regression.

## Output format

```
Change under test: <ticket / fix>
Fix verification:
  - [ ] Original defect no longer reproduces
Adjacent checks:
  - [ ] <related feature 1> still works
  - [ ] <related feature 2> still works
Verdict: Sane — proceed  |  Not sane — reopen / full regression
```

## Quality checklist

- [ ] The original defect is confirmed fixed.
- [ ] Checks are focused on the change's blast radius (not everything).
- [ ] A clear proceed / escalate verdict is given.
