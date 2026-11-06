# Example — Test plan for mobile in-app checkout

## Input

> Test plan for the mobile app's new in-app checkout feature. Releasing in 3 weeks. Team:
> 2 QA + 1 automation engineer. Platforms: iOS + Android.

## Output

```
# Test Plan — Mobile In-App Checkout

1. Scope
   In:  Cart review, address entry, payment (card + saved card), order confirmation,
        iOS + Android, English locale.
   Out: Apple Pay / Google Pay (planned for a follow-up release), tablet layouts.
2. Approach & Levels: Unit (payment calculation logic) + system (full checkout flow) +
   UAT (2 internal stakeholders) on real devices; functional + security (payment data
   handling) + accessibility (screen reader checkout flow).
3. Environments & Data: Staging API + sandboxed payment provider; seeded test accounts
   with saved cards for both platforms.
4. Tools: Appium (cross-platform automation), BrowserStack device cloud, Postman (API checks).
5. Entry Criteria: Feature-complete build on staging; payment sandbox available; test
   accounts seeded.
6. Exit Criteria: 100% P1 cases pass on both platforms; 0 open P1/P2 defects; ≥ 80%
   of planned cases automated.
7. Risks & Mitigations: Payment provider sandbox instability (mitigation: mock fallback
   for CI, real sandbox for manual/nightly runs); App Store review timeline risk
   (mitigation: submit a build for review 1 week before the target date).
8. Schedule & Owners: Week 1 — case design + automation scaffolding (QA lead). Week 2 —
   execution + bug-fix cycle (full team). Week 3 — regression + sign-off (QA lead).
9. Deliverables: test case suite, automated Appium suite, test summary report, release sign-off.
```
