# Architecture — SCRUM-6: Display Total Number of Filtered Products

## Status and source of truth

- **Requirements:** [`requirements.md`](requirements.md), status `READY_FOR_ARCHITECTURE`.
- **Jira:** [SCRUM-6](https://epam-team-d41wx5ya.atlassian.net/browse/SCRUM-6).
- **Scope:** Display the length of the existing filtered result array above the products. No new filtering logic, backend, or product data changes.
- **Repository evidence:** `product-search/src/ProductSearchApp.jsx` derives `visibleProducts` from the existing filter state using `filterProducts`, then renders `visibleProducts.length` in the results heading above the product grid.

## 1. Architecture overview

The existing React page already has the source of truth needed for the count. Keep the existing `visibleProducts` calculation and render a user-facing count from its length in the current results heading. Since the count is derived in the same render as the filtered list, it changes with search, category, and price state without a second state variable, effect, or backend call.

## 2. Component and data flow

| Component | Change / responsibility | Traceability |
|---|---|---|
| `ProductSearchApp` in `product-search/src/ProductSearchApp.jsx` | Format and display the count from `visibleProducts.length` above the list; show exact `0 products found` for zero. | FR-001–FR-003; AC-001–AC-003 |
| Existing `filterProducts` and `visibleProducts` | Remain the single filtered-data source; no behavior change. | FR-002 |
| Existing Vitest tests | Add focused coverage for zero, one, multiple results and count changes as filters change. | NFR-003; AC-001–AC-003 |

### Data flow

```text
search/category/price state
          |
          v
existing filterProducts -> visibleProducts
          |                       |
          +--> visibleProducts.length -> count above list
                                  |
                                  +--> existing product list
```

The count should not be stored separately. This avoids stale count state and keeps it consistent with the displayed products.

## 3. Display and interaction behavior

- Place the count in the existing results heading, before the product grid.
- If the count is zero, render exactly `0 products found`.
- For positive values, the explicit implementation assumption in requirements uses singular/plural product wording.
- Keep the existing empty-state content and list behavior unless directly required by implementation. The count is additional summary text, not a replacement for the existing empty-state element.
- Depend on React's normal render after filter state changes; do not introduce backend calls, debouncing, or duplicate filtering.

## 4. Error handling and security

- The count is a numeric length of the existing in-memory result array; no parsing or external input handling is introduced.
- No new network requests, data collection, or secrets are needed.
- If result derivation fails, existing application behavior remains; do not add a silent fallback count.

## 5. Testing strategy

Add focused Vitest coverage using the existing React test setup to verify:

1. The count appears above the list and represents the initial filtered products.
2. Search, category, and price changes update the count with their corresponding results.
3. A zero-result state displays the exact `0 products found`.
4. Singular and plural output follows the documented positive-count assumption.

Run the existing Vitest suite and production build. Exercise the app-level interaction and exact displayed text; code inspection alone is not sufficient verification.

## 6. Traceability

| Requirement | Component/decision | Planned verification |
|---|---|---|
| FR-001 / AC-001 | Display `visibleProducts.length` in existing results heading above the list. | Component test asserts count and placement. |
| FR-002 / AC-002 | Derive count directly from `visibleProducts`, which is recalculated from existing filter state. | Tests update query, category, and price, then assert count. |
| FR-003 / AC-003 | Special-case zero count to the exact required string. | Test asserts exact text `0 products found`. |
| NFR-001 | Reuse existing React component and filtered array. | Diff/code inspection and tests. |
| NFR-002 | No backend or API changes. | Scoped diff inspection. |
| NFR-003 | Add unit tests to existing Vitest suite. | Run Vitest and record evidence. |

## 7. Assumptions and unresolved questions

- Positive-count singular/plural wording follows the explicit assumption recorded in `requirements.md`; it does not change the required exact zero text.
- No unresolved architecture questions block implementation.
