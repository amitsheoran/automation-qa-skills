# Release Sign-off Checklist

Use before approving a production release. Pairs with `deployment-guide` and `qa-documentation`.

## Functional
- [ ] All P1/P2 test cases passed
- [ ] 0 open P1/P2 defects (or explicitly waived with sign-off)
- [ ] Smoke suite green on the target environment
- [ ] Regression suite (full or risk-based selection) green

## Non-functional
- [ ] Performance SLOs met (p95 latency, error rate)
- [ ] Accessibility: no new WCAG 2.2 AA blockers
- [ ] Security: no open High/Critical findings

## Data & migrations
- [ ] Migrations reviewed and reversible
- [ ] DB backup/snapshot taken
- [ ] Data reconciliation checks pass

## Operational
- [ ] Feature flags configured for launch
- [ ] Monitoring/alerts in place for new functionality
- [ ] Rollback plan documented and tested
- [ ] Stakeholders notified; changelog published

## Sign-off
- [ ] QA verdict recorded: **Go / No-Go / Go with caveats**
