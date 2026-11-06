# Example — Exploring new search filters

## Input

> Explore the new multi-select search filters on the product listing page. Release is
> in 3 days. 90-minute session.

## Output

```
Charter: Explore the multi-select category/price/rating filters using varied combinations
         and interruptions to discover defects before release.
Timebox: 90 min

Notes:
  - Selected "In Stock" + "On Sale" together → 0 results, even though matching products
    exist when filters are applied one at a time.
  - Clearing one filter chip removes both from the URL query string, not just the one clicked.
  - Rapidly toggling a filter on/off 5x → result count flickers between stale and correct
    values (race condition on the debounce).
  - Browser back button after applying 3 filters restores only the first filter.

Bugs found:
  - "In Stock" + "On Sale" combined returns 0 results incorrectly (SEARCH-204)
  - Removing one filter chip clears all filters (SEARCH-205)
  - Back button loses filter state (SEARCH-206)

Questions/risks:
  - Is filter combination logic AND or OR across categories? Not specified in the ticket.
  - No automated coverage exists yet for multi-filter combinations.

Coverage & next: Covered desktop Chrome, 2-3 filter combinations, back/forward nav. Still
  need: mobile viewport, screen-reader pass, 4+ filter combinations, slow-network behavior.
```
