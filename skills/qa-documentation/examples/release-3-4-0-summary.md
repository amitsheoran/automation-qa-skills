# Example — Test summary report for a checkout redesign release

## Input

> Write the test summary report for release 3.4.0 (checkout redesign): 86 cases executed,
> 81 passed, 2 failed, 3 blocked, defects P1:0 P2:2 P3:1, 20/20 requirements covered,
> 68% automated.

## Output

```
# Test Summary Report — Checkout Redesign v3.4.0 (2026-11-06)

Verdict: Go with caveats — 2 open P2 defects, both have workarounds

Scope:        New checkout flow (cart, address, payment, confirmation). Excludes
              gift-card redemption (deferred to v3.5.0).
Environment:  build 3.4.0-rc2 · staging · seeded test data set "checkout-qa-v2"
Execution:    Total 86 · Passed 81 · Failed 2 · Blocked 3
Defects:      P1: 0 · P2: 2 (CHK-512, CHK-519) · P3: 1 (CHK-521)
Coverage:     Requirements covered 20/20 · Automation 68%
Risks:        CHK-512 (saved address autofill occasionally picks the wrong default) has
              a manual workaround (select address explicitly); low customer-facing frequency.
Recommendation: Go with caveats — ship with CHK-512/CHK-519 tracked for a 3.4.1 patch;
              the 3 blocked cases depend on a sandbox payment provider outage, re-run
              before full sign-off once the provider is back.
```
