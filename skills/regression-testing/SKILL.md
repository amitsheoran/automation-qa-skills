---
name: regression-testing
description: Build and prioritize a regression suite that catches breakage in existing functionality after changes, using impact analysis and risk-based selection.
---

# Purpose

Ensure new changes don't break existing features. Produces a prioritized regression suite and
a selection strategy so you run the right subset for a given change.

## When to use

- Before a release, or after a significant change/refactor.
- Maintaining a long-lived regression suite.

## When NOT to use

- Quick build stability check → `smoke-testing`.
- Verifying one fix's blast radius → `sanity-testing`.

## Inputs

- The change set (diff/modules touched), existing features, and prior defect hotspots.

## Workflow

1. **Impact analysis:** map the change to affected modules and downstream features.
2. **Risk-rank** areas by change frequency, complexity, and historical defect density.
3. Select regression cases: **always-run core** + **impacted-area** + **recently-buggy**.
4. Tag cases for **automation** vs. manual; keep automated regression in CI.
5. Define a **full vs. selective** policy (full before release, selective per PR).
6. Track **flaky tests** and quarantine them (see `contract-testing`/CI notes).

## Output format

| Area | Risk | Reason | Cases | Run When | Auto? |
|------|------|--------|-------|----------|-------|
| Payments | High | High complexity + recent defects | 24 | Every PR + release | Yes |
| Profile settings | Low | Rarely changes | 6 | Release only | Yes |

Plus a **selection rule**: e.g. "PR touching `payments/*` → run Payments + Checkout + Smoke."

## Quality checklist

- [ ] Selection is driven by impact analysis, not "run everything blindly."
- [ ] High-risk / historically buggy areas are prioritized.
- [ ] Full vs. selective run policy is defined.
- [ ] Flaky tests are identified and quarantined, not ignored.

## Example

See `examples/payments-refactor.md` for a full worked example.
