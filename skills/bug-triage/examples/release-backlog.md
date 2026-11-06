# Example — Pre-release backlog triage

## Input

> Triage this backlog before Friday's release:
> 1. Pay button disabled after applying a coupon — always reproduces, no workaround.
> 2. Tooltip misaligned on Safari only — cosmetic.
> 3. Export-to-CSV times out for reports > 10k rows — affects 3 large enterprise customers,
>    workaround is manual export in batches.
> 4. Duplicate report of #1, filed separately by support.
> 5. Search returns stale results for ~2 seconds after a filter change — low impact, hard to notice.

## Output

| Bug | Severity | Priority | Frequency | Workaround | Action | Owner | Blocker? | Rationale |
|-----|----------|----------|-----------|------------|--------|-------|----------|-----------|
| Pay button disabled after coupon | High | P1 | Always | None | Fix now | Payments | Yes | Revenue blocker, no workaround |
| Tooltip misaligned on Safari | Low | P4 | Rare | N/A | Backlog | Web | No | Cosmetic, single browser |
| CSV export times out > 10k rows | High | P2 | Affects 3 enterprise accounts | Manual batch export | Next sprint | Reporting | No | Real impact but a usable workaround exists |
| Duplicate of "Pay button disabled" | — | — | — | — | Duplicate → merged into bug #1 | Payments | No | Same root cause as #1, closed as dup |
| Stale search results ~2s | Low | P3 | Always, but subtle | N/A | Backlog | Search | No | Low visibility, no reported user complaints |

**Release blockers:** bug #1 (Pay button disabled) — must be fixed before Friday's release.
