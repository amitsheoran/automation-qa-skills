---
name: bug-triage
description: Prioritize and route a backlog of bugs by assigning severity, priority, owner, and action (fix now / next / backlog / wont-fix / duplicate) with clear rationale.
---

# Purpose

Turn an unsorted list of defects into a triaged, actionable queue so the team knows what to
fix first and why.

## When to use

- A sprint/release triage of multiple open bugs.
- Deciding what blocks a release vs. what can wait.

## When NOT to use

- Writing a single ticket → `jira-bug-writer`.
- Finding a cause → `root-cause-analysis`.

## Inputs

- List of bugs (title, area, symptom, and any repro/impact info).
- Release context: deadline, risk tolerance, affected user segments.

## Workflow

1. For each bug, set **Severity** (user/business impact) and **Priority** (urgency).
2. Detect **duplicates** and link them; merge context.
3. Assess **frequency × impact × workaround availability**.
4. Assign an **action**: `Fix now` · `Next sprint` · `Backlog` · `Won't fix` · `Duplicate` · `Needs info`.
5. Route to an **owner/area** and note the rationale.
6. Flag **release blockers** explicitly.

## Output format

| Bug | Severity | Priority | Frequency | Workaround | Action | Owner | Blocker? | Rationale |
|-----|----------|----------|-----------|------------|--------|-------|----------|-----------|
| Pay button disabled with coupon | High | P1 | Always | None | Fix now | Payments | Yes | Revenue blocker, no workaround |
| Tooltip misaligned on Safari | Low | P4 | Rare | N/A | Backlog | Web | No | Cosmetic, single browser |

## Quality checklist

- [ ] Severity and Priority assigned independently.
- [ ] Duplicates linked, not re-triaged separately.
- [ ] Every bug has an action and (if actionable) an owner.
- [ ] Release blockers are explicitly flagged.
- [ ] Rationale is one clear sentence per bug.
