# Example — Checkout 500 errors after a deploy

## Input

> Symptom: checkout returns HTTP 500 for ~8% of orders since yesterday's deploy. Logs
> show a `NullPointerException` in `DiscountCalculator`. The deploy introduced a new
> `loyaltyDiscount` feature flag, default off for most accounts but on for accounts
> created before 2023.

## Output

```
Symptom:        Checkout returns HTTP 500 for ~8% of orders since the v4.8.0 deploy.
Root Cause:     DiscountCalculator.applyLoyaltyDiscount() reads account.enrolledAt,
                which is null for accounts created before the loyalty program existed
                (pre-2023), causing a NullPointerException.
Evidence:       Stack trace points to DiscountCalculator.java:41. Querying affected
                accounts confirms 100% of 500s correlate with enrolledAt IS NULL and
                accountCreatedAt < 2023-01-01 (matches the ~8% of accounts predating
                the loyalty program).
5 Whys:
  1. Why does checkout 500? → NullPointerException in DiscountCalculator.
  2. Why is there a null? → account.enrolledAt is null for pre-2023 accounts.
  3. Why is it read unguarded? → The new loyaltyDiscount flag assumes every account
     has an enrolledAt value once the flag is on for that account.
  4. Why does the flag apply to pre-2023 accounts at all? → The rollout targeted
     "accounts older than 1 year," which unintentionally includes pre-loyalty-program
     accounts that were never backfilled.
  5. Why weren't they backfilled? → The migration plan only covered post-2023 accounts;
     pre-2023 accounts were missed during loyalty program launch.
Trigger:        v4.8.0 deploy enabled the loyaltyDiscount flag for accounts > 1 year old.
Contributing:   Incomplete data backfill when the loyalty program first launched.
Fix:            Null-check enrolledAt in DiscountCalculator and skip the loyalty discount
                (rather than crash) when it's absent; separately backfill enrolledAt for
                pre-2023 accounts.
Prevention:     Add a regression test with a null enrolledAt account; add a data-integrity
                alert if any account has the loyaltyDiscount flag on with enrolledAt null.
```
