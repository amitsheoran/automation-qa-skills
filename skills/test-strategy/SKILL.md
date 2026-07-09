---
name: test-strategy
description: Define a high-level, long-lived testing strategy for a product or team — test pyramid, automation approach, environments, quality gates, and metrics.
---

# Purpose

Set the *organizational* approach to quality: how the team tests across releases, where
automation lives, what the quality gates are, and how quality is measured.

## When to use

- Standing up QA for a product/team; defining the automation approach and quality gates.

## When NOT to use

- A single feature/release plan → `test-plan`.

## Inputs

- Product architecture (web/API/mobile/data), team size/skills, release cadence, risk tolerance.

## Workflow

1. Define the **test pyramid** balance (unit/integration/E2E) suited to the architecture.
2. Decide the **automation strategy**: frameworks, what to automate, CI triggers, flake policy.
3. Define **environments & data strategy** (ephemeral, seeded, masked prod).
4. Establish **quality gates**: PR checks, coverage thresholds, release gates.
5. Choose **metrics**: escaped defects, automation %, flake rate, MTTR, coverage.
6. Cover **non-functional** ownership: performance, security, accessibility.
7. Define **roles & responsibilities** (who owns what testing).

## Output format

```
# Test Strategy — <product/team>

Test Pyramid:     unit __% · integration __% · E2E __%
Automation:       frameworks, scope, CI triggers, flake policy
Environments:     dev/stage/prod approach + test data strategy
Quality Gates:    PR checks · coverage ≥ X% · release gate criteria
Non-Functional:   perf / security / a11y ownership & cadence
Metrics:          escaped defects, automation %, flake rate, MTTR
Roles:            who owns unit / API / E2E / NFR / release sign-off
```

## Quality checklist

- [ ] Pyramid balance justified by the architecture.
- [ ] Automation scope and flake policy defined.
- [ ] Quality gates are enforceable in CI.
- [ ] Metrics are measurable and owned.
- [ ] Non-functional testing has an owner and cadence.
