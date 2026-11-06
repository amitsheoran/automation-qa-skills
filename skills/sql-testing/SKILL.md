---
name: sql-testing
description: Validate data at the database layer — integrity, constraints, reconciliation, migrations, and ETL — with safe, read-first SQL queries and assertions.
---

# Purpose

Verify that application behavior is backed by correct data: referential integrity, constraint
enforcement, reconciliation between systems, and correct migrations/ETL.

## When to use

- Backend/data validation, ETL/migration testing, reconciliation checks.

## When NOT to use

- API response validation → `api-testing`.

## Inputs

- Schema, the tables/columns involved, and the business rules to verify.

## Workflow

1. **Read-first, always.** Run `SELECT`s; never mutate production data. Use a transaction + rollback if a write is unavoidable in a test DB.
2. **Integrity checks:** orphan rows, null violations, duplicate keys, broken FKs.
3. **Constraint checks:** uniqueness, enums/domains, ranges, NOT NULL enforced.
4. **Reconciliation:** counts/sums match between source and target (e.g. app vs. warehouse).
5. **Migration/ETL:** row counts before/after, spot-check transformed values, no data loss.
6. Express each check so it returns **0 rows = pass** (offending rows on failure).

## Output format — assertion queries

```sql
-- Orphaned order rows (should return 0)
SELECT o.id
FROM orders o
LEFT JOIN customers c ON c.id = o.customer_id
WHERE c.id IS NULL;

-- Duplicate emails (should return 0)
SELECT email, COUNT(*) AS n
FROM users
GROUP BY email
HAVING COUNT(*) > 1;

-- Reconciliation: source vs target totals (diff should be 0)
SELECT (SELECT COUNT(*) FROM staging.orders)
     - (SELECT COUNT(*) FROM warehouse.orders) AS row_diff;
```

## Quality checklist

- [ ] All queries are read-only (or transaction + rollback in a test DB).
- [ ] Checks return 0 rows on pass; offending rows on fail.
- [ ] Integrity, constraint, and reconciliation checks included.
- [ ] Migration/ETL verified with before/after counts and spot checks.
- [ ] No secrets or real PII pasted into the repo.

## Example

See `examples/warehouse-reconciliation.md` for a full worked example.
