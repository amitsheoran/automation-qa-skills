# Example — Nightly staging smoke suite

## Input

> Build a smoke suite for the e-commerce app's nightly staging deploy.

## Output

| ID | Critical Path | Check | Expected | Auto? |
|----|---------------|-------|----------|-------|
| SM-01 | App health | `GET /health` | 200, DB + cache OK | Yes |
| SM-02 | Login | Log in as a standard user | Dashboard loads | Yes |
| SM-03 | Core create | Add one product to the cart | Saved, visible in cart | Yes |
| SM-04 | Checkout/payment | One happy-path purchase (test card) | Order confirmed, confirmation email queued | Yes |
| SM-05 | Search | Search for a known in-stock product | Product appears in results | Yes |

**Gate:** any failure above blocks promotion to the regression run and pages the on-call
engineer — total suite runtime target: under 4 minutes.
