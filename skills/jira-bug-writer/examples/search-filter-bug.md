# Example — Writing up a search filter defect

## Input

> While exploring search filters I found: selecting "In Stock" and "On Sale" together on
> the product listing page returns 0 results, even though products matching both exist.
> Found on staging, Chrome. Logged in as a standard user.

## Output

```
Title: [Search] "In Stock" + "On Sale" filters combined return 0 results (should AND correctly)

Environment: build 5.2.1 · staging · Chrome 126 / macOS · role: standard user
Reproducibility: Always (5/5)
Severity: High   Priority: P2

Preconditions:
- Catalog contains at least one product that is both in stock and on sale (e.g. SKU "WM-200").

Steps to Reproduce:
1. Open /products.
2. Apply the "In Stock" filter — confirm SKU WM-200 appears.
3. Additionally apply the "On Sale" filter (both now selected).
4. Observe the result count.

Expected: Results show products matching BOTH filters (e.g. WM-200 still shown).
Actual:   Result list shows "0 results found", even though WM-200 satisfies both filters
          individually.

Impact: Customers combining these two common filters see an empty catalog and may
        abandon the search — direct conversion risk during a sale period.
Workaround: Apply only one filter at a time.
Evidence: screenshot-0-results.png, network.har (shows query params `inStock=true&onSale=true`
          and a response with `"total": 0`).
```
