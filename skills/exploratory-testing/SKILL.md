---
name: exploratory-testing
description: Run structured exploratory testing using session-based charters and heuristics (SFDIPOT, CRUD, boundaries) to find defects scripted tests miss.
---

# Purpose

Guide time-boxed, charter-driven exploration to discover defects that scripted cases don't
anticipate — with enough structure to be repeatable and reportable.

## When to use

- New/changed features, risky areas, or when specs are thin.
- Complementing (not replacing) scripted regression.

## When NOT to use

- You need repeatable pass/fail records for every acceptance criterion → `test-case-generator`.

## Inputs

- The feature/area, known risks, and a time box (e.g. 60–90 min).

## Workflow

1. Write a **charter**: "Explore <area> using <resources> to discover <information>."
2. Pick **heuristics** to drive coverage:
   - **SFDIPOT** — Structure, Function, Data, Interfaces, Platform, Operations, Time.
   - **CRUD** — Create, Read, Update, Delete each entity.
   - **Boundaries, zeros, negatives, huge inputs, interruptions, back button, double-submit.**
3. Time-box the **session**; take running notes (actions, observations, questions, bugs).
4. Log defects immediately (→ `jira-bug-writer`).
5. Debrief: coverage achieved, risks found, areas needing more time.

## Output format

```
Charter: Explore <area> using <data/tools> to discover <risk/info>.
Timebox: 90 min
Notes:
  - <action> → <observation>
Bugs found:
  - <symptom> (ticket)
Questions/risks:
  - <open question>
Coverage & next: <what was covered / what still needs a session>
```

## Quality checklist

- [ ] A clear charter frames the session.
- [ ] At least one heuristic set drove coverage.
- [ ] Session is time-boxed with running notes.
- [ ] Findings are logged and debriefed with coverage/risk notes.

## Example

See `examples/search-filters-session.md` for a full worked example.
