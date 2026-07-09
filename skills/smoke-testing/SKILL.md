---
name: smoke-testing
description: Design a fast, shallow smoke-test suite that verifies a build is stable enough for deeper testing — critical paths only, minutes to run.
---

# Purpose

Produce a small "build verification" suite covering only the most critical, must-work paths,
so a broken build is rejected within minutes.

## When to use

- Right after a deploy/build, before running full regression.
- As a CI gate on every deploy to an environment.

## When NOT to use

- Verifying a specific bug fix didn't break related areas → `sanity-testing`.
- Full coverage before release → `regression-testing`.

## Inputs

- The application's critical user journeys and health endpoints.

## Workflow

1. List the **critical paths** (login, core create/read, checkout, key integrations).
2. For each, define the shallowest check that proves it works end-to-end.
3. Add **health checks**: app up, DB reachable, key third-party integrations responding.
4. Keep the suite **fast** (target < 5 min) and **stable** (no flaky steps).
5. Define the **gate**: any smoke failure → build rejected, deeper testing blocked.

## Output format

| ID | Critical Path | Check | Expected | Auto? |
|----|---------------|-------|----------|-------|
| SM-01 | App health | GET /health | 200, DB+cache OK | Yes |
| SM-02 | Login | Log in as standard user | Dashboard loads | Yes |
| SM-03 | Core create | Create one primary entity | Saved, visible in list | Yes |
| SM-04 | Checkout/payment | One happy-path purchase | Order confirmed | Yes |

## Quality checklist

- [ ] Only critical, must-work paths included.
- [ ] Whole suite runs in a few minutes.
- [ ] Steps are stable (no known flakiness).
- [ ] Failure clearly blocks the pipeline.
