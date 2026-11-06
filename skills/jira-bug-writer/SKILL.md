---
name: jira-bug-writer
description: Write clear, reproducible, triage-ready Jira bug reports with title, environment, steps, expected vs actual, severity/priority, and evidence.
---

# Purpose

Convert a raw defect observation into a well-structured Jira bug that a developer can
reproduce and fix without a follow-up conversation.

## When to use

- A test failed or unexpected behavior was observed and needs logging.
- Converting a Slack/verbal report into a proper ticket.

## When NOT to use

- Deciding *which* bugs to fix first → use `bug-triage`.
- Diagnosing the underlying cause → use `root-cause-analysis`.

## Inputs

- What happened, where (environment/build), and how to reproduce.
- (Optional) logs, screenshots, network traces, affected user/account.

## Workflow

1. Write a **specific** title: `[Area] Short symptom (condition)`.
2. Capture environment: build/version, OS, browser/device, environment (dev/stage/prod), account/role.
3. Write **numbered reproduction steps** starting from a known state.
4. State **Expected** vs **Actual** results separately.
5. Set **Severity** (impact) and **Priority** (urgency) independently.
6. Attach evidence and note reproducibility (always / intermittent / once).
7. Add impact ("blocks checkout for all users") and any known workaround.

## Output format

```
Title: [Checkout] Payment button disabled after applying valid coupon (Chrome)

Environment: build 4.12.0 · staging · Chrome 126 / Win11 · role: standard user
Reproducibility: Always (5/5)
Severity: High   Priority: P1

Preconditions:
- Cart with 1+ items; a valid active coupon exists.

Steps to Reproduce:
1. Add item to cart and open /checkout.
2. Enter coupon SAVE10 and click Apply.
3. Observe the Pay button.

Expected: Pay button stays enabled; total reflects discount.
Actual:   Pay button becomes permanently disabled; console error `TypeError: total is undefined`.

Impact: All users using coupons cannot pay → revenue blocker.
Workaround: Remove coupon to re-enable Pay.
Evidence: screenshot.png, console.log, HAR attached.
```

## Quality checklist

- [ ] Title names the area, symptom, and condition.
- [ ] Steps reproduce from a clean state; a stranger could follow them.
- [ ] Expected and Actual are separate and concrete.
- [ ] Severity and Priority set independently, with impact stated.
- [ ] Evidence attached; reproducibility noted.

## Example

See `examples/search-filter-bug.md` for a full worked example.
