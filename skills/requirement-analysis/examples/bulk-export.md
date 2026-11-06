# Example — Analyzing a thin requirement

## Input

> Requirement: "Users should be able to export their data in bulk." (as given, no further detail)

## Output

```
Requirement: Bulk data export
Testable?    No — too vague to derive pass/fail cases as written

Ambiguities:
  - Which data? All account data, or a specific entity (orders, invoices, activity log)?
  - What format(s) — CSV, JSON, both? Is there a size limit per export?
  - Synchronous download or async (email link / notification when ready)?
  - Who can trigger it — any user, or only account admins?

Missing acceptance criteria:
  - Expected format and maximum rows/size per export.
  - Behavior when the data set is empty.
  - Behavior when the export is very large (async job, progress indicator, timeout).

Edge cases to cover:
  - Zero records to export.
  - Export spanning millions of rows (performance, memory, timeout).
  - Concurrent export requests from the same user.
  - User's data includes special characters / unicode (CSV escaping).

NFRs / dependencies:
  - Performance: max acceptable export time for the largest expected data set.
  - Security: export must only include data the requesting user is authorized to see.

Verdict: Needs clarification (blocking: data scope, format, size limits, sync vs async).
```
