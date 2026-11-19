# Implementation Plan — SCRUM-6

## Prerequisites

- Requirements are `READY_FOR_ARCHITECTURE`.
- Design review has no unresolved `BLOCKER` or `HIGH` findings.
- Implementation remains within `product-search/`; no backend changes.

## Tasks

| ID | Priority | Description | Requirements / ACs | Dependencies | Files | Tests / checks | Completion criteria | Status |
|---|---|---|---|---|---|---|---|---|
| IMP-001 | Must | Add count formatting sourced from the existing filtered result length and display it above the product list, including exact `0 products found`. | FR-001–FR-003; AC-001–AC-003; NFR-001–NFR-002 | None | `product-search/src/ProductSearchApp.jsx`, `product-search/src/filterProducts.js` | Vitest formatter cases; Playwright assertions for initial, changed-filter, and zero results | Count remains derived from filtered results; exact zero text and documented positive singular/plural wording are displayed; existing list and empty-state behavior remain unchanged. | DONE |
| IMP-002 | Must | Add focused regression tests for count formatting and visible updates for each supported filter. | FR-001–FR-003; AC-001–AC-003; NFR-003 | IMP-001 | `product-search/src/filterProducts.test.js`, `product-search/tests/product-search.spec.js` | `npm run test:unit`; `npm run test:e2e` | Unit and browser tests cover zero, one, multiple results and search/category/price changes; all targeted suites pass. | DONE |

## Dependency order

1. IMP-001 defines and renders the count.
2. IMP-002 verifies its formatting and filter-driven updates.

No dependency cycles or unresolved planning assumptions.
