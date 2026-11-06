---
name: test-plan
description: Produce a concrete IEEE-829-style test plan for a feature or release — scope, approach, resources, schedule, environments, entry/exit criteria, and risks.
---

# Purpose

Create a test plan that answers *what* will be tested, *how*, *when*, *by whom*, and *when we
are done* — for a specific feature or release.

## When to use

- Kicking off testing for a feature or release.
- Need agreed entry/exit criteria and scope before execution.

## When NOT to use

- Organization-wide, long-lived testing approach → `test-strategy`.

## Inputs

- Feature/release scope, timeline, team, environments, and risk appetite.

## Workflow

1. Define **scope**: in-scope and explicitly out-of-scope items.
2. Choose the **test approach** and levels (unit/integration/system/UAT) and types (functional, perf, security, a11y).
3. List **environments, data, and tools** needed.
4. Define **entry criteria** (when testing can start) and **exit criteria** (when it's done).
5. Identify **risks** and mitigations; set a **schedule** and **owners**.
6. State **deliverables** (cases, reports, sign-off).

## Output format

```
# Test Plan — <feature/release>

1. Scope
   In:  ...
   Out: ...
2. Approach & Levels: <unit/integration/system/UAT; functional/perf/security/a11y>
3. Environments & Data: ...
4. Tools: ...
5. Entry Criteria: <e.g. build deployed to staging, test data seeded>
6. Exit Criteria:  <e.g. 100% P1 cases pass, 0 open P1/P2, coverage ≥ X%>
7. Risks & Mitigations: ...
8. Schedule & Owners: ...
9. Deliverables: cases, summary report, sign-off
```

## Quality checklist

- [ ] Out-of-scope is stated explicitly.
- [ ] Entry and exit criteria are measurable.
- [ ] Environments, data, and tools identified.
- [ ] Top risks have mitigations and owners.
- [ ] Deliverables and sign-off defined.

## Example

See `examples/mobile-checkout-release.md` for a full worked example.
