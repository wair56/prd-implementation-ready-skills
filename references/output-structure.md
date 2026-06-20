# Output Structure

Use this when writing the actual PRD or splitting multiple documents.

Apply `necessity-gate.md`: choose the smallest document structure that preserves business clarity, implementation decisions, testing criteria, UX constraints, and risk controls.

## Global vs Module Detail

For complex or multi-module PRDs, create one entrance document / front section:

`00_PRD总览_全局规则_主业务流程`

It contains:

- Plain-language business background, role glossary, and core concept dictionary.
- Business anchors / non-negotiable premises and change-control note.
- One-sentence business goal.
- Document/module index.
- Main business flow.
- Core object relationship.
- Data flow and linkage map.
- Global rule index.
- Cross-module dependencies and writebacks.
- In-scope / out-of-scope index.
- High-risk must-read items.

It does not contain page-level detailed tables.

## Global Rule Boundary

Global overview can say:

> All list Tabs must define business wording, data source, include/exclude conditions, mutual exclusivity, default sort, and must avoid technical conditions.

Global overview should not expand:

- 来账待认领 / 来账已认领 Tab details.
- Payment page Tab filters.
- Field lists.
- Button rules.
- Modal text.
- Interface fields.

Those belong in the specific module/page PRD.

## Full PRD Skeleton

Use this as a menu, not a quota. Omit or merge sections that add no decision, rule, acceptance criterion, or reader clarity.

1. Document info and confirmation record.
2. Business background, role glossary, business anchors, core concept dictionary, and plain-language flow.
3. Overview, global rule index, and main flow.
4. Background and goals.
5. Scope.
6. Users, roles, permissions, visibility.
7. User stories and scenarios.
8. Business flow.
9. Core objects, object flows, and state machines.
10. Data flow, source documents, writeback, linkage, and idempotency.
11. Feature details.
12. Exceptions and reverse flows.
13. Boundaries.
14. Interaction and UI states.
15. Data requirements.
16. Analytics.
17. Non-functional requirements.
18. Release strategy.
19. Dependencies and risks.
20. Appendix.

## Feature Detail Page Section

Each concrete page/module should include:

- Page purpose.
- Target端 / touchpoint and target user.
- Entry and permissions.
- Data source and authority.
- Data flow, linked objects, writeback, and downstream consumers.
- List fields.
- Search/filter/sort/page rules.
- Tab filter rules in business language.
- UX carrier and key components.
- Operations and state restrictions.
- Detail/drawer/modal behavior.
- Empty/loading/error/disabled states.
- Validation, disabled reasons, confirmation, recovery, audit trace.
- Exceptions and acceptance criteria.

Only expand a subsection when it changes behavior, implementation, testing, UX, risk, or operation.

## List Tab Filter Table

Keep this inside the relevant page/module:

| Page / Tab | Business Include Condition | Business Exclude Condition | Data Source | Default Sort | Mutually Exclusive? | Empty State |
|---|---|---|---|---|---|---|

No SQL, code enum, or database field conditions in PRD正文.
