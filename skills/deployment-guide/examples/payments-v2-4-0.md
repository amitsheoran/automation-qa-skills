# Example — payments-service v2.4.0 release runbook

## Input

> Draft the release runbook for shipping `payments-service v2.4.0` to production via
> blue-green deploy. Change set includes a new `refunds` table migration.

## Output

```
# Release Runbook — payments-service v2.4.0 → production

Pre-deploy:
  [ ] Smoke + regression green on staging
  [ ] `refunds` table migration reviewed (additive, no backfill required)
  [ ] Feature flag `refunds_v2` set to off (dark launch)
  [ ] DB snapshot taken; current version v2.3.6 noted
  [ ] Stakeholders (payments team, support) notified of deploy window

Deploy:
  1. Run migration: create `refunds` table (additive, zero-downtime)
  2. Deploy v2.4.0 to the green environment
  3. Shift 10% of traffic to green; warm caches
  4. Shift remaining 90% once step 3 is healthy for 10 minutes

Verify:
  [ ] Smoke suite passes against green
  [ ] Error rate < 0.5% (baseline) on green
  [ ] p95 checkout latency unchanged (±5%)

Monitor (30 min): payments dashboard, refund error rate, p95/p99 latency, alert channel.

Rollback (if refund error rate > 2% or smoke fails on green):
  1. Shift 100% of traffic back to blue (v2.3.6)
  2. Leave the `refunds` table in place — it's additive, no rollback migration needed
  3. Re-run smoke against blue to confirm recovery
```
