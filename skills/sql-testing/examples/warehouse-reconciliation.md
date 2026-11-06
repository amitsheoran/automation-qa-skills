# Example — Nightly ETL reconciliation

## Input

> After the nightly ETL job, verify the `orders` table synced correctly from `staging`
> to `warehouse`, and that there are no orphaned `order_items`.

## Output

```sql
-- Row count reconciliation: source vs target (should be 0)
SELECT (SELECT COUNT(*) FROM staging.orders)
     - (SELECT COUNT(*) FROM warehouse.orders) AS row_diff;

-- Orphaned order_items after sync (should return 0 rows)
SELECT oi.id
FROM warehouse.order_items oi
LEFT JOIN warehouse.orders o ON o.id = oi.order_id
WHERE o.id IS NULL;

-- Spot-check transformed values: totals must match after currency normalization
SELECT s.id, s.total AS source_total, w.total AS warehouse_total
FROM staging.orders s
JOIN warehouse.orders w ON w.source_id = s.id
WHERE ROUND(s.total * s.fx_rate, 2) <> w.total;

-- Duplicate order ids introduced by the sync (should return 0 rows)
SELECT source_id, COUNT(*) AS n
FROM warehouse.orders
GROUP BY source_id
HAVING COUNT(*) > 1;
```

All four checks returned 0 rows on the last ETL run — reconciliation passed, no data
loss or duplication detected.
