---
name: deployment-guide
description: Produce a safe release/deployment runbook with pre-deploy checks, deploy steps, smoke verification, monitoring, and a tested rollback plan.
---

# Purpose

Generate a repeatable deployment runbook so releases are predictable: what to check before,
how to deploy, how to verify, what to watch, and how to roll back.

## When to use

- Preparing to ship a release; standardizing deploys; on-call handover.

## When NOT to use

- Designing CI test coverage → `test-strategy`.

## Inputs

- Environment, deploy mechanism (CI/CD, blue-green, canary), and the change set.

## Workflow

1. **Pre-deploy gate:** all quality gates green (smoke/regression), migrations reviewed, feature flags set, changelog ready, stakeholders notified.
2. **Backup/rollback prep:** DB backup/snapshot, note current version, confirm rollback is tested.
3. **Deploy:** steps in order (migrate → deploy → warm caches), prefer canary/blue-green.
4. **Verify:** run `smoke-testing` on the target env; check key metrics and error rates.
5. **Monitor:** watch dashboards/alerts for the defined soak window.
6. **Rollback plan:** exact trigger conditions and steps to revert (and how to verify the revert).

## Output format

```
# Release Runbook — v<X.Y.Z> → <env>

Pre-deploy:
  [ ] Smoke + regression green   [ ] Migrations reviewed   [ ] Flags set
  [ ] DB backup taken            [ ] Current version noted  [ ] Stakeholders notified
Deploy:
  1. Run migrations
  2. Deploy app (canary 10% → 100%)
  3. Warm caches
Verify:
  [ ] Smoke suite passes on <env>   [ ] Error rate < baseline   [ ] p95 latency normal
Monitor (30 min): dashboards, alerts, logs
Rollback (if <trigger>):
  1. Re-deploy previous build v<X.Y.Z-1>
  2. Roll back migration <name> (if needed)
  3. Re-run smoke to confirm recovery
```

## Quality checklist

- [ ] Pre-deploy gate lists concrete, checkable items.
- [ ] Backup taken and current version recorded before deploy.
- [ ] Post-deploy smoke + metric verification included.
- [ ] Rollback has explicit trigger conditions and is verified after.
