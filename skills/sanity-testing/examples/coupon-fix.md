# Example — Sanity-checking a coupon fix before merge

## Input

> Ticket PAY-482 fixed: "Pay button disabled after applying a coupon" by correctly
> handling `total` when a discount reduces it to 0. Sanity-check before merging.

## Output

```
Change under test: PAY-482 — Pay button disabled after applying a coupon

Fix verification:
  - [x] Original defect no longer reproduces (applying SAVE10 keeps Pay enabled)

Adjacent checks:
  - [x] A 100%-off coupon (total goes to exactly $0) — Pay button stays enabled, order
        completes as a zero-value order
  - [x] Removing an applied coupon recalculates the total and keeps Pay enabled
  - [x] Applying two stackable coupons in sequence still enables Pay
  - [ ] Coupon on a cart with only digital (no-shipping) items — not yet checked

Verdict: Sane — proceed to merge. Flag the digital-items case for the next full
  regression pass rather than blocking this fix.
```
