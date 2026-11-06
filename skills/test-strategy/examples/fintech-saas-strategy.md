# Example — Test strategy for a fintech SaaS team

## Input

> Define the testing strategy for a 15-engineer SaaS team building a web + REST API
> fintech product. Biweekly release cadence, moderate risk tolerance (regulated but
> not life-critical).

## Output

```
# Test Strategy — Fintech SaaS Platform

Test Pyramid:     unit 60% · integration 30% · E2E 10% (API-heavy product; UI covers
                  only the critical money-movement flows end-to-end)
Automation:       Playwright for E2E, REST Assured for API/contract tests. All new
                  code requires unit tests in CI; E2E runs on every merge to main and
                  nightly against staging. Flaky tests quarantined within 24h or deleted.
Environments:     Ephemeral per-PR environments for integration tests; a stable staging
                  environment with masked/synthetic production-shaped data for E2E;
                  never real customer financial data outside production.
Quality Gates:    PR: unit + lint + 80% diff coverage. Pre-release: full regression +
                  security scan + accessibility check on changed screens.
Non-Functional:   Performance — owned by platform team, load-tested before each
                  release touching the ledger service. Security — OWASP Top 10 review
                  each release (compliance requirement); accessibility — WCAG 2.2 AA,
                  reviewed quarterly.
Metrics:          Escaped defects per release, automation % of regression suite, flake
                  rate (<2% target), MTTR for P1 incidents.
Roles:            Feature teams own unit + integration tests for their service; a
                  central QA function owns E2E, performance, and release sign-off.
```
