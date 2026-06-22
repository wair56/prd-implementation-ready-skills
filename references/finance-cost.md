# Finance Cost

Solves: period attribution, cost inclusion, paid/unpaid cost eligibility, annual/prepaid cost recognition, amortization, accrual, and synced cost data.
Read when: costs enter monthly bills, reports, reconciliation, profit sharing, or any downstream settlement.
Pair with: `data-flow.md` for source/detail eligibility and `business-consistency.md` for cross-document computability.

## Period Attribution

Every cost, income, bill, payment, and reconciliation source must define its attribution period. Do not write "monthly aggregation" without source date and boundary rules.

Core version:

| Item | Must Define |
|---|---|
| Attribution anchor | authoritative field that places the object in a period |
| Period boundary | calendar month, accounting month, timezone, start/end inclusion |
| Non-authoritative dates | display/search/audit-only dates |
| Late data | delayed/backfilled/corrected data after lock |
| Lock rule | whether confirmed/audited bills freeze source details |

Do not invent attribution anchors. Receipt time, entry time, sync time, approval time, and payment completion time are not automatically the period. Pick them only when the business object proves they are authoritative.

## Cost Inclusion State Gate

Use this before any cost enters a monthly bill, customer reconciliation, supplier reconciliation, profit sharing, report, or downstream settlement.

Core version:

| Item | Must Define |
|---|---|
| Cost source object | vehicle fee, payroll, supplier consumption, insurance, toll, energy, manual adjustment |
| Audit status | which audit/validation must pass |
| Payment status | unpaid, pending, paying, failed, paid, auto-completed, no external payment |
| Recognition state | recognized, amortized, deferred, reversed, voided, exception |
| Locked cost period | open, confirmed, locked, used downstream |
| Inclusion/exclusion | exact states allowed and excluded |
| Rebuild rule | before/after lock behavior when data changes |

If the business requires paid costs, say "audit passed and payment status is paid/auto-completed." If accrued but unpaid costs are allowed, define liability and payment follow-up.

For payroll-like costs, an eligible worker with no payable amount may need a zero payroll line. If a line is eligible but no payable amount exists because source data is missing, route to an exception page instead of silently writing zero.

## Amortization / Accrual Gate

Use for annual, prepaid, cross-period, subscription, insurance, maintenance package, rent, license, warranty, or one-time payment whose business cost belongs to several periods.

Separate cash payment from cost recognition:

| Item | Must Define |
|---|---|
| Cash payment | payer/payee, payment order, status, ledger effect |
| Cost recognition | periods receiving the cost |
| Period anchors | start/end/service/policy/contract/vehicle binding period |
| Allocation formula | straight-line monthly, daily proration, usage allocation, rounding |
| Generated schedule | schedule lines, monthly cost items, lock behavior |
| Early termination/refund | future schedule, locked periods, adjustment lines |

## Sync and Master Data

Synced cost/supplier data must define source system, direction, mode, cursor, duplicate/update rule, disabled/deleted data handling, and sync timestamp. If a supplier can be disabled after use, preserve historical visibility and future selectable scope.

## Good-Enough Example

```text
Driver payroll enters the monthly cost bill only after payroll audit passes and payment status is paid.
Drivers active in the period but with no payable amount receive a zero payroll line, proving they were considered.
If waybill data arrives after the cost period is locked, it creates an adjustment in the next unlocked period instead of rebuilding the locked bill.
```
