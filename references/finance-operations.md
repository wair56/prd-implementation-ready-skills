# Finance Operations Guards

Use this for PRDs involving cost management, supplier management, reconciliation, invoicing, receipts, refunds, recharge/payment, fund balances, monthly bills, or synced financial data.

## Period Attribution

Every cost, income, bill, payment, and reconciliation source must define its attribution period. Do not write "按月汇总" without source date and boundary rules.

Require:

| Item | Must Define |
|---|---|
| Source date | payable date, service month, sync time, payment completion time, sign-off time, invoice time |
| Month boundary | calendar month, accounting month, timezone, include start/end |
| Current month rule | whether current month is excluded and why |
| Late data | how resynced, delayed, corrected, or backfilled data affects an approved month |
| Lock rule | whether confirmed/audited bills freeze source details |

Example checks:

- Vehicle fee by payable date.
- Driver labor by labor month.
- Energy/toll by data sync time month.
- Other cost by payment completion time.
- Monthly bill cannot be confirmed while included source items are unpaid.

## Fund and Payment Priority

When one role pays another or a recharge/payment order is created, define:

| Item | Must Define |
|---|---|
| Payer / payee | contractor, platform, supplier, shipper, driver |
| Balance types | own balance, equity/support fund, frozen amount, available amount |
| Deduction priority | exact order and whether mixed/merged payment is allowed |
| Insufficient balance | block, partial pay, combine payment, retry, cancel |
| Ledger effect | which balance increases/decreases/freezes/releases |
| Reversal | refund, cancel, audit reject, red flush, void, duplicate callback |

Do not leave "优先自有余额 / 权益金靠后 / 支持合并支付" as final wording without the above details.

## Sync and Master Data

For synced data and supplier master data, define:

| Item | Must Define |
|---|---|
| Source system | platform, value-added system, third party, import |
| Direction | source to target, target to source, two-way |
| Mode | full sync, incremental sync, manual refresh, scheduled task |
| Cursor | update time, business time, id, version, event |
| Duplicate/update rule | create, update, ignore, merge, tombstone |
| Deleted/disabled data | whether hidden, retained, or still visible when already used |
| Sync timestamp | whether shown on list/detail and how stale data is handled |

If a supplier can be disabled after use, keep historical records and define future visibility, selectable scope, notification, and effect on existing bills/contracts/drivers.

## Reconciliation and Invoice Lifecycle

For supplier or shipper reconciliation:

| Item | Must Define |
|---|---|
| Source details | which consumption/order/monthly bill records can be selected |
| Grouping | by supplier, shipper, month, cost type, contract, project |
| Locking | when source details become occupied and when they are released |
| Audit | who audits, what approval changes, what rejection rolls back |
| Invoice | invoice status, upload/editable fields, amount lock, file source |
| Receipt/write-off | balance deduction, insufficient balance, reverse write-off |
| Cancel/reject | source detail rollback and status clearing |
| Red flush / void | same-month void vs cross-month red flush, invoice tracking update, source status after rollback |

Do not state "撤销 / 拒绝 / 红冲 / 作废" without the affected object, rollback scope, and status after rollback.

## Cross-Module Navigation

For "查看详情，带参跳转到 X 对应 Tab" define:

| Item | Must Define |
|---|---|
| Source page/action | where user clicks |
| Target menu/page/Tab | exact target carrier |
| Parameters | supplier id, contractor id, cost type, month, bill id, status |
| Default filters | what auto-fills and whether users can clear them |
| Empty/missing data | what happens when target record no longer exists or is not visible |
| Permission | whether the target page/action is allowed for this role |

Treat cross-module jumps as requirements, not UI garnish.

## Modal / Detail Reuse

If confirm and view-detail reuse one modal, define mode differences:

| Mode | Must Define |
|---|---|
| Confirm mode | editable actions, confirmation button, blocking conditions, validation |
| View mode | read-only fields, audit info, history, hidden actions |
| Shared list/detail | pagination, whether search is supported, default grouping |
| Amount summary | cost type grouping, total consistency with details |
