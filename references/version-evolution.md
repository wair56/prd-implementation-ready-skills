# Version Evolution

Use this when the user is changing an existing PRD, product, implementation, or v1 behavior. This is not a fresh-from-zero PRD path.

## Version Evolution Gate

Before writing v2 wording, capture the change as a product migration:

| Item | Must Define |
|---|---|
| Old rule | current v1 behavior, existing PRD wording, implementation behavior, or user habit |
| Change intent | what the user wants to change and why now |
| Business driver | user value, operational pain, compliance, revenue, cost, risk, or simplification |
| Impact range | affected objects, pages, fields, status axes, permissions, APIs, reports, notifications, background jobs, exports, and acceptance tests |
| Old vs new | side-by-side diff of behavior, data, UI, state, and exception handling |
| Migration | whether historical records are converted, versioned, left as-is, rebuilt, or shown with old labels |
| Backward compatibility | how old clients, old links, old data, pending workflows, and integrations behave during transition |
| Release plan | feature flag, tenant rollout, data backfill, training, notice, monitoring, and cutoff |
| Rollback | what can be safely rolled back, what requires compensation, and what is irreversible |
| Decision record | who decided, when, why, sources used, and which modules/doc files were updated |

## Diff Table

| Area | Old Rule | New Rule | Reason | Data Migration | UI / Copy Change | Risk | Confirm |
|---|---|---|---|---|---|---|---|

## Change Propagation Checklist

After a change is accepted, update together:

- Main 00/global doc and module docs.
- Object lifecycle and state diagrams.
- Page fields, filters, operation states, disabled reasons, and empty states.
- Data source, writeback, idempotency, reports, notifications, and exports.
- Existing tests/acceptance criteria and release notes.

Red flags:

- Writing v2 as if v1 never existed.
- Keeping old and new rules side by side without saying which wins.
- Changing a formula/state/page without migration, backward compatibility, rollback, or decision record.

