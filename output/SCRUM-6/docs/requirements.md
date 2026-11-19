# Requirements — SCRUM-6: Display Total Number of Filtered Products

## 1. Overview

### Problem and goal

Customers need to know how many products match the current filters. SCRUM-6 requires a total count above the product list, kept in sync with search, category, and price filters, with the exact zero-results count `0 products found`.

### Scope

- Display the number of products in the existing filtered result set above the product list.
- Update that number when search, category, or price filters change.
- Show `0 products found` when there are no matches.
- Reuse the existing React UI and filtered product data.
- Make no backend changes and add focused Vitest unit tests.

### Out of scope

- Changes to product filtering semantics, product data, or backend services.
- Other product-search behavior not required by SCRUM-6.

## 2. Stakeholders & Personas

| Persona / stakeholder | Source evidence | Need |
|---|---|---|
| Customer | Jira user story | Know how many products match the current filters. |
| Delivery / quality team | Jira technical requirements | Implement the count in existing React code and verify it with Vitest. |

## 3. Assumptions & Dependencies

### Source-backed constraints

- Use existing React components and the existing filtered product data.
- No backend changes are required.
- Add basic Vitest unit tests.

### Assumptions

- For positive result counts, display the numeric count with a product label (singular for one, plural otherwise). The source fixes the zero-result text but does not prescribe positive-count wording.
- The displayed count is derived from the same filtered result set that renders the product list.

### Dependencies

- Existing product-search React page, filtered product data, and filter state.
- Existing Vitest setup.

## 4. Functional Requirements

| ID | Priority | Requirement | Source | Notes |
|---|---|---|---|---|
| FR-001 | Must | The product results section must display the total number of products matching the current filters above the product list. | Jira AC1 | Use the existing filtered result set. |
| FR-002 | Must | The displayed total must update when the search, category, or price filter changes. | Jira AC2 | No backend or separate filtering implementation. |
| FR-003 | Must | When no products match, the displayed total must read exactly `0 products found`. | Jira AC3 | Preserve the exact source string. |

## 5. Non-Functional Requirements

| ID | Priority | Requirement / constraint | Source | Verification |
|---|---|---|---|---|
| NFR-001 | Must | Implement the feature using existing React components and filtered product data. | Technical Requirements | Inspect code and test behavior. |
| NFR-002 | Must | Do not make backend changes for this feature. | Technical Requirements | Inspect the scoped diff. |
| NFR-003 | Must | Add basic Vitest unit tests for the count behavior. | Technical Requirements | Run the focused Vitest suite. |

No separate performance, privacy, availability, or accessibility targets are specified by this issue.

## 6. User Flow

1. The customer views the product list and its total count.
2. The customer changes search, category, or price filters.
3. The displayed count changes to match the filtered products.
4. If there are no matching products, the displayed count is exactly `0 products found`.

## 7. Acceptance Criteria

| ID | Given | When | Then | Source |
|---|---|---|---|---|
| AC-001 | The product list is displayed | The customer views the results section | The total number of filtered products is displayed above the product list. | Jira AC1 |
| AC-002 | The product list is displayed | The customer changes search, category, or price filters | The count updates to reflect the filtered product data. | Jira AC2 |
| AC-003 | No products match the current filters | The results are displayed | The count reads exactly `0 products found`. | Jira AC3 |

## 8. Source & Traceability

- **Issue:** SCRUM-6, “Display Total Number of Filtered Products” (Task, Medium, To Do).
- **Canonical URL:** https://epam-team-d41wx5ya.atlassian.net/browse/SCRUM-6
- **Source statement (verbatim):** “As a customer, I want to see the number of products matching my filters so that I know how many results are available.”
- **Acceptance criteria (verbatim):**
  1. “Display the total number of products above the product list.”
  2. “Update the count automatically when search, category, or price filters change.”
  3. “Display `0 products found` when no products match.”
- **Technical requirements (verbatim):**
  - “Use the existing React components and filtered product data.”
  - “No backend changes required.”
  - “Add basic Vitest unit tests.”
- **Retrieval:** Jira issue and metadata retrieved read-only. No linked issues, comments, or remote links were returned; no unrelated issues were searched.

| Source reference | Requirement | Acceptance criterion |
|---|---|---|
| User story; AC1 | FR-001 | AC-001 |
| AC2 | FR-002 | AC-002 |
| AC3 | FR-003 | AC-003 |
| Technical Requirements | NFR-001 | AC-001–AC-002 |
| Technical Requirements | NFR-002 | AC-001–AC-003 |
| Technical Requirements | NFR-003 | AC-001–AC-003 |

The implementation and verification links will be populated by later phases using the same stable identifiers.

## 9. Open Questions

No implementation-blocking ambiguity was identified. The Jira source specifies the exact zero-count string; positive-count phrasing is an explicitly recorded implementation assumption.

## 10. Status

`READY_FOR_ARCHITECTURE`
