# Example — Regression selection for a payments provider swap

## Input

> The `payments` module was refactored to switch providers. `checkout` and `invoicing`
> depend on it. Release is in 5 days.

## Output

| Area | Risk | Reason | Cases | Run When | Auto? |
|------|------|--------|-------|----------|-------|
| Payments | High | Core logic rewritten, new provider SDK | 28 | Every PR + release | Yes |
| Checkout | High | Directly calls payments for charge/refund | 22 | Every PR + release | Yes |
| Invoicing | Medium | Reads payment status, no logic changes | 10 | Release only | Yes |
| Smoke suite (all areas) | — | Baseline build health | 12 | Every PR | Yes |
| Account settings | Low | No dependency on payments | 0 | Not run this cycle | — |

**Selection rule:** any PR touching `payments/*` → run Payments + Checkout + Smoke.
Full Invoicing suite runs once before the release cut, not per-PR.

**Flaky tests quarantined:** `CHK-089 refund retry timing` (known 5% flake on the old
provider's sandbox) — rewritten against the new provider's sandbox before being
re-added to the required gate.
