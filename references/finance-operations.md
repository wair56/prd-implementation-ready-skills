# Finance Operations Guards

Use this for PRDs involving cost management, supplier management, reconciliation, invoicing, receipts, refunds, recharge/payment, fund balances, monthly bills, or synced financial data.

## Finance Fund-Flow Gate

Use this gate before final PRD wording for any balance, credit, support fund, prepayment, recharge, refund, freeze, occupation, release, write-off, profit sharing, or payment action.

First prove the fund loop closes:

| Item | Must Define |
|---|---|
| Source pool | Which account/pool owns the money or quota: own balance, platform-granted credit, shipper receipt, supplier prepayment, frozen amount, occupied amount |
| Usable amount | Available/frozen/occupied/reserved fields and the formula for usable balance |
| Action nature | Real external payment, operating payment, internal ledger registration, fund adjustment, quota allocation, or accounting-only record |
| Deduction / occupation order | Priority, whether mixed funding is allowed, and how partial insufficiency is covered |
| Ledger effect | Which pool increases/decreases/freezes/releases/occupies, and which ledger entry is created |
| Source trace | Whether the movement is traceable to source receipts/orders/bills; whether trace is audit-only or controls release |
| Closure path | How the amount returns, is written off, released, refunded, recovered, profit-shared, or becomes terminal |
| Reverse path | Cancel, refund, reject, red flush, void, duplicate callback, timeout, or correction effect |
| Page visibility | Which role ledger view shows it and whether users can see counterparty funds |

If any row is missing, do not hide it in a generic "按规则处理". Mark the gap or ask the smallest business question needed.

## Period Attribution

Every cost, income, bill, payment, and reconciliation source must define its attribution period. Do not write "按月汇总" without source date and boundary rules.

### Attribution Gate

Before recommending late-data handling, lock behavior, refund impact, recovery timing, or monthly aggregation, first define the attribution anchor. Do not ask late-data handling questions until the attribution anchor is explicit.

Do not invent attribution anchors. Receipt time, entry time, sync time, approval time, or payment completion time may be convenient system fields, but they are not automatically the business object period. Pick them only when the business object and business reason prove they are authoritative. If the real anchor is service month, consumption month, bill period, payment completion time, reconciliation confirmation time, or another source field, use that instead and name non-authoritative dates as display/search/audit-only.

Use this template:

```text
[Object] is attributed to [period] by [authoritative date/time field].
The period boundary is [calendar month/accounting month/custom cycle], timezone [timezone], interval [start inclusive, end exclusive].
[Other date fields] are used only for [display/search/audit], not for attribution.
After the period is locked, delayed/backfilled/corrected data [goes to next unlocked period / creates an adjustment item / is blocked / requires explicit user confirmation].
```

Require:

| Item | Must Define |
|---|---|
| Attribution anchor | the authoritative field that makes the object belong to a period: payable date, service month, sync time, payment completion time, sign-off time, invoice time, confirmation time, entry time |
| Month boundary | calendar month, accounting month, timezone, include start/end |
| Current month rule | what "current month / this period" means and which authoritative time field is used |
| Non-authoritative dates | which dates are display/search/audit only and do not affect attribution |
| Late data | how resynced, delayed, corrected, or backfilled data affects an approved month |
| Lock rule | whether confirmed/audited bills freeze source details |

Example checks:

- Vehicle fee by payable date.
- Driver labor by labor month.
- Energy/toll by data sync time month.
- Other cost by payment completion time.
- Monthly bill cannot be confirmed while included source items are unpaid.

### Relative Period Phrase Gate

Any phrase such as "本月", "当月", "本期", "账期内", "周期内", "同月", "下月", "current month", or "this period" must be expanded before final PRD wording.

Required definition:

| Item | Must Define |
|---|---|
| Business object | waybill, cost item, income item, reconciliation bill, payment order, quota ledger, profit-sharing record |
| 权威时间字段 | creation time, dispatch time, departure time, sign-off time, completion confirmation time, cost confirmation time, entry time, payment completion time, bill generation time |
| Period boundary | calendar month/accounting month/custom cycle, timezone, start/end inclusion |
| Cross-month rule | if create/sign-off/confirm/pay happen in different months, which one wins |
| Correction rule | whether a changed timestamp or delayed confirmation recomputes the period after lock |

Do not write "本月运单" as final wording. Write "waybills whose [authoritative time field] falls in [period boundary]".

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

Mixed funding is a first-class case. Do not reduce a payment/recharge/occupation to a single-source "二选一" flow unless the user or source docs explicitly confirm it. If one source has partial insufficiency, define whether the remaining amount can be covered by another source, the priority order, the resulting split ledger, and the disabled reason when combined funds are still insufficient.

Do not leave "优先自有余额 / 权益金靠后 / 支持合并支付" as final wording without the above details. Avoid single-source wording such as "choose available balance or support fund" when the business may need available balance + support fund in one action.

## Payment Nature and Recharge Classification

For every recharge/prepayment/payment-like word, decide whether it is:

| Nature | Meaning | PRD Must Say |
|---|---|---|
| Operating payment | Money actually goes to a supplier/driver/partner/platform for business operation | Payee, payment trigger, payment order or completed adjustment, external payment status, failure/retry |
| Supplier prepayment / pre-recharge | Real money is prepaid to a supplier, then allocated internally to assets, vehicles, users, or orders | Supplier fund pool, later allocation rule, balance visibility, refund/reversal |
| Internal ledger registration | No money leaves the system; only internal quota/balance/usage is recorded | Why there is no external payment, ledger entry, audit trail, downstream use |
| Quota allocation | Existing quota/balance is assigned from one object to another | Source quota, target object, release/reclaim rule |

Do not call an operating payment or supplier pre-recharge an internal ledger registration merely because the amount is later allocated to vehicles/assets/users. Allocation after payment is not proof that no external payment happens.

## Fund Pools, Source Trace, and Closure Paths

Separate pool balance from source trace:

- **Pool balance** controls whether money/quota is usable now: available/frozen/occupied/reserved.
- **Source trace** explains where the amount came from: receipt, platform grant, contract, bill, manual adjustment, import, supplier refund.
- A pool may be traceable for audit without requiring a strict one-to-one pairing between a later occupation/release and the original source detail.

Do not assume one-to-one pairing between occupied credit and the same incoming receipt/source detail unless the business explicitly requires source-level locking. When refunding, releasing, or returning funds, decide whether the rule consumes from the current pool balance, from a source-level trace line, or from a configured priority. If the pool is sufficient, the system may deduct/release from the pool even when the exact original source line is not paired.

For every fund type, define its source-specific closure path:

| Source | Closure Path Questions |
|---|---|
| Platform-granted support credit | Can it be returned directly? Is return deterministic no-audit if quota is sufficient? Does it create an auto-completed ledger/payment adjustment? |
| Shipper receipt / customer funds | Can it only be released through refund, reconciliation/write-off, or settlement? Is direct "return support fund" forbidden? |
| Contractor own balance | Can it be deducted first, withdrawn, refunded, or used for operating payment? |
| Supplier prepayment | How it is consumed by vehicle/order allocation, supplier reconciliation, refund, or reversal |
| Frozen funds | What business event freezes it, what event uses it, and what event releases it |

Do not merge source-specific closure paths into one generic "refund/return" flow. For example, platform-granted support credit and shipper funds may sit in related ledgers, but they can have different legal/business closure paths.

## Deterministic No-Audit and Auto-Completed Adjustments

Do not add an approval flow just because money is involved. If a rule is deterministic and the usable amount is sufficient, write the no-audit path explicitly:

- Trigger: user/system action and source object.
- Preconditions: sufficient available/credit/frozen/occupied amount, permitted source type, no conflicting lock.
- Ledger effect: deduction/release/occupation/write-off.
- Generated record: fund adjustment, payment order, or ledger entry.
- Status: auto-completed if no external payment or human decision is needed.
- Visibility: where the user sees the completed record and audit trail.
- Failure: insufficient amount, forbidden source, concurrent occupation, duplicate submit.

Use an audit only when a human/business judgment is required: exception amount, manual adjustment, dispute, risk review, source conflict, or policy override.

## Frozen Funds Closed Loop

Frozen/occupied funds are not automatically "unusable forever" or "excluded from reconciliation". Model the frozen funds closed loop:

1. What event freezes/occupies the amount.
2. Whether the frozen amount can still participate in reconciliation/write-off, settlement, profit sharing, or recovery.
3. Which event converts frozen/occupied amount into used/written-off/released amount.
4. Whether profit sharing returns support credit or releases the contractor's withdrawable profit.
5. Which reverse actions reopen the freeze/occupation and which terminal states do not.

If frozen funds are meant to secure later reconciliation, say that clearly. Do not write "frozen funds do not participate in reconciliation" unless the business explicitly confirms that exclusion and the resulting value loop still closes.

## Role Ledger View and Account Presentation

Every finance module with balances must define a role ledger view. Account pages should not only show "my balance"; they must show the counterparty funds that explain why the balance is available, frozen, occupied, or waiting for closure.

For each role ledger view, define:

| View | Must Show |
|---|---|
| Contractor | own available balance, platform-granted/support credit, shipper/customer funds related to the contractor, available/frozen/occupied, source trace, pending closure actions |
| Platform | contractor fund status, shipper fund status, support credit grant/occupation/return, abnormal locks, cross-role totals |
| Shipper / customer | receipt balance, refundable balance, frozen/used/written-off amount, funds linked to contractors, refund/reconciliation status |
| Supplier | prepayment/recharge balance, consumption/allocation, reconciliation/payment/refund status |

The page should expose counterparty funds enough for users to understand why an operation is allowed/disabled. Use terms the business already uses; do not invent new account names just to make the table neat.

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

### Supplier Reconciliation Completeness

Supplier reconciliation completeness requires more than consumption sync and reconciliation totals. Define invoice management and payment trajectory, especially for each supplier type.

| Supplier Type | Must Define |
|---|---|
| Prepaid / pre-recharge supplier | Prepayment balance, consumption deduction, allocation to vehicles/orders/assets, recharge payment, refund/reversal |
| Non-prepaid supplier | Payable generation, reconciliation bill, invoice management, payment application/order, payment completion, unpaid/part-paid/paid status |
| Mixed supplier | Which products/cost types are prepaid vs postpaid and whether one supplier has separate ledgers |

For non-prepaid supplier payment trace, define: payable source, payment trigger, payment order or auto-completed adjustment, payment status, callback/result source, failure retry, cancellation, and how the reconciliation bill shows paid/unpaid/part-paid.

Invoice management is a child object, not a line in "发票登记". Define invoice source, upload/import/sync, invoice number uniqueness, invoice amount vs bill amount matching, edit/void/red-flush, attachment/image source, invoice status, page entry, and writeback to reconciliation/payment.

When reconciliation is generated from selectable source details, also apply the Source Detail Eligibility Gate:

| Item | Must Define |
|---|---|
| Selectable source details | which orders, waybills, cost items, consumption items, or monthly bill lines can enter the bill |
| Eligibility states | completed, signed off, fee-confirmed, unaudited, unpaid, exception-free, or other required states |
| Amount readiness | receivable/payable amount exists, formula is valid, exceptions and zero/negative amounts are handled |
| Period filter | which attribution anchor and period boundary decide "current month / this period" details |
| Occupation | when selected details become occupied and cannot be selected again |
| Release rule | when cancel, reject, void, or rollback releases occupation; when confirmed/paid/profit-shared details stay occupied |
| Unavailable reason | how users see why an item cannot be selected |

## Amortization / Accrual Gate

Use this for annual, prepaid, cross-period, subscription, insurance, maintenance package, rent, license, warranty, or any one-time payment whose business cost belongs to several periods.

Separate cash payment from cost recognition:

| Item | Must Define |
|---|---|
| Cash payment | When real payment happens, payer/payee, payment order, payment status, and ledger effect |
| Cost recognition | Which periods receive the cost and whether monthly amortization is required |
| Authoritative period anchors | Start date, end date, service period, policy period, contract period, vehicle binding period, or usage period |
| Allocation formula | Straight-line monthly amortization, daily proration, vehicle count/usage allocation, manual split, rounding, remainder handling |
| Source and snapshot | Which policy/contract/payment document defines amount and period, and whether later edits rebuild schedule |
| Generated schedule | Amortization plan lines, monthly cost items, status, lock after bill/reconciliation/profit sharing |
| Early termination / refund | How cancellation, refund, vehicle disposal, supplier change, or policy correction affects future and locked periods |
| Page visibility | Where users see original payment, remaining unamortized amount, current-month cost, and schedule detail |

Do not write "annual insurance enters payment management" as the whole rule if the product needs monthly cost. The PRD must state whether insurance is paid annually but recognized monthly, and how monthly amortization enters cost management.

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
