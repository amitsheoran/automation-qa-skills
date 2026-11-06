---
name: root-cause-analysis
description: Systematically diagnose the true root cause of a defect or incident using 5-Whys, fault isolation, and evidence — not just the surface symptom.
---

# Purpose

Move from "the test failed" to "here is the exact cause, the evidence, the fix, and how we
prevent recurrence." Produces an RCA suitable for a bug comment or incident review.

## When to use

- A bug's cause is unclear, or a fix keeps not working.
- Post-incident review; recurring or flaky failures.

## When NOT to use

- Just logging the defect → use `jira-bug-writer`.
- A flaky test where the cause is test-side timing → note it, but confirm with evidence first.

## Inputs

- The failure symptom, logs/stack traces, recent changes (diffs, deploys, config, data).
- The environment and whether it reproduces.

## Workflow

1. **Reproduce & isolate.** Confirm the failure and narrow to the smallest trigger.
2. **Collect evidence.** Logs, stack traces, timestamps, diffs, DB state, network calls.
3. **Build a timeline.** What changed just before it started? (deploy, config, data, dependency)
4. **Ask 5 Whys** — each answer grounded in evidence, not guesses.
5. **Distinguish** trigger vs. root cause vs. contributing factors.
6. **Propose the fix** (and why it addresses the root, not the symptom).
7. **Prevention.** Test to add, alert, guardrail, or process change.

## Output format

```
Symptom:        <observed failure>
Root Cause:     <the actual defect, one sentence>
Evidence:       <logs/diff/data proving it>
5 Whys:
  1. Why did X happen? → ...
  2. Why? → ...
  ...
Trigger:        <what set it off now>
Contributing:   <factors that made it possible>
Fix:            <change that removes the root cause>
Prevention:     <regression test / alert / guardrail>
```

## Quality checklist

- [ ] Root cause is proven by evidence, not assumed.
- [ ] Symptom, trigger, and root cause are clearly separated.
- [ ] Fix addresses the root cause, not the symptom.
- [ ] A concrete regression test / guardrail is proposed.

## Example

See `examples/checkout-500.md` for a full worked example.
