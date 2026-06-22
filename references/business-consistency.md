# Business Consistency and Computability

Use this when reviewing multiple PRD files, finance/payment/settlement requirements, or any flow involving funds, payment orders, audit, refunds, allocation, cost, profit sharing, amount calculation, inventory, coupon, quota, permission, capacity, entitlement, or other resource/value movement.

## P0 Rule Pointer

Authoritative staged blockers live in `references/guardrail-checklist.md`. Keep the P0/P1 trigger list there so SKILL.md, review checklists, and consistency guidance do not drift.

Use this file for the detailed consistency and computability methods behind those blockers: cross-document conflicts, data source authenticity, formula operand semantics, resource/value computability, audit/payment disambiguation, and ghost status cleanup.

Common P0 examples routed to the guardrail checklist: missing post-event writeback, missing payment order origin payload, missing Cost Inclusion State Gate, missing progress-control loop, unclear real-payment nature, and ghost status.

Authoritative gate: `references/data-flow.md#post-event-writeback-gate`.

## Cross-Document Consistency Scan

Run this scan only across documents inside the user-selected scope. If the user-selected scope is one directory or one file, sibling directories and old versions are out-of-scope document sources, even if they are larger or older. Ask before expanding scope.

When reviewing multiple docs, actively compare the same term across files. Use a table like:

| Object / Term | Location A | Location B | Conflict | Why It Blocks | Recommended Question |
|---|---|---|---|---|---|

Check especially:

- business anchors, pricing/fund/source/status premises, and "obvious" domain facts;
- visible data source markers, authoritative sources, generated-data rules, and copied-field update rules;
- attribution anchor, source detail eligibility, relative period phrase definitions, source occupation, and occupation release rules;
- document links, module numbers, reading order, and "this doc overrides..." statements;
- object names: cost bill, payment order, reconciliation bill, profit bill, invoice, receipt, refund;
- actions: create, submit, confirm, audit, pay, refund, reverse, void, rebuild, push;
- states: pending audit, pending payment, paying, failed, refunded, exception pending confirmation;
- money words: available, frozen, occupied, released, deducted, transferred, allocated, shared.
- non-finance resource words: inventory, coupon, quota, points, capacity, permission, entitlement, slot, reservation, grant, revoke, consume, release.

## Data Source Authenticity Guard

When a PRD names a data source, verify that the source is not only plausible but usable:

| Data Item | Claimed Source | Source Exists In Flow? | Authority Consistent? | Timing Valid? | Internal Generation Complete? | Conflict Location | Required Fix |
|---|---|---|---|---|---|---|---|

Include internal sources such as generated bills, snapshots, status counts, to-do tasks, callback results after idempotency, imported staging records, and manually adjusted records. They must be checked the same way as external systems.

If a source comes from an external system, other module, upstream object, import, sync result, configuration, or source detail list, also run `data-flow.md#source-data-filter-and-eligibility-gate`. Source authenticity is not enough: the PRD must define the business eligibility filter, UI query filter, include condition, exclude condition, owner scope, permission scope, source state, period window, selectable/visible but unavailable behavior, unavailable reason, dedupe/occupation, snapshot, refresh timing, sync status, stale data, external failure, and correction behavior.

## Money and Formula Computability Guard

Every amount or fund movement must be computable from business information. Require:

| Item | Must Define |
|---|---|
| Source amount | Which source document / contract / bill / fee item provides it |
| Formula | Operand, operator, rate, cap/floor, tax handling, rounding |
| Allocation | If one amount enters multiple pools, define split rule, priority, and conservation check |
| Snapshot | Whether the calculation freezes source data, rule version, and result |
| Timing | When the calculation happens and when it can be recalculated |
| Reversal | How cancel, refund, reject, or rebuild affects the amount |

If any part is missing, write "不可计算，需确认" and explain what cannot be tested.

Examples:

- "来账确认后增加货主冻结资金和运营支持金" is incomplete unless it defines how one receipt amount is split between the two pools.
- "分润金额" is incomplete unless it defines profit formula, sharing parties, sharing ratio/source, rounding, snapshot, and negative/zero handling.
- "按月包车应收金额" is incomplete unless it defines whether the source is shipper contract, monthly fixed amount, route/package agreement, manual adjustment, or imported bill.
- "金额构成 = 成本基数 + 服务费 + 调整项" is incomplete unless each component has source object/source field, source state, business period, snapshot, formula/operand, drilldown/backtrace, detail sum reconciliation, and correction behavior. Use `data-flow.md#amount-composition-source-lineage-gate`.

- "Use current binding / current relationship / latest configuration / real-time fetch" is incomplete when that relationship decides an amount, list scope, permission, receiver, or report dimension. Use `data-flow.md#relationship-temporal-anchor-gate` to define relationship source, source authority, binding record, effective period, anchor time, business period, snapshot, rebuild policy, and correction behavior.

## Settlement Input vs Analysis Metric Computability

Before requiring a derived number in final PRD wording, classify it with `data-flow.md#business-action-input-vs-analysis-metric-gate`.

- If a metric is a settlement hard constraint, every operand must be computable before the business action runs. Define source, timing, snapshot, rounding, correction, and reverse behavior.
- If a metric is an analysis metric, define its source, freshness, caveat, drilldown, and role visibility, but do not force allocation and do not let it block invoice, settlement, payment, refund, or resource release.
- If a cost attribution, usage split, contribution score, or occupancy share has no reliable source, it cannot be a settlement prerequisite. Mark it as risk analysis / post-event analysis or ask for the missing source.
- Preserve dimensions for diagnosis and later modeling even when the action uses a reliable total. Mapping corrections for analysis-only metrics should rebuild analysis outputs and does not change financial results unless the user explicitly confirms a financial restatement rule.

| Relationship operand | If the operand scope comes from a relation such as dedicated vehicle or vehicle-customer binding, define the relationship source, effective period, anchor time, snapshot, and correction behavior |

Diagnostic questions:

- What business action consumes this value: billing, invoice, settlement, payment, refund, approval, risk analysis, dashboard, or profitability review?
- Does the value change money/resource movement, or only explain whether the movement was profitable, risky, or worth optimizing later?
- If the value is unavailable, should the action stop, proceed with a reliable total, or proceed while marking the analysis as unavailable?

## Formula Operand Semantics Gate

This gate catches the "the formula looks obvious, but the business term is undefined" failure. A formula is not complete because it has operators. It is complete only when each operand has business meaning and a source.

Use it for finance and non-finance formulas involving guarantee/保底, max/min, take greater/take smaller, cap, floor, tier, threshold, rate, coefficient, base amount, detail-calculated amount, adjustment, score, quota, usage, points, or ranking.

Require:

| Item | Must Define |
|---|---|
| Formula purpose | What business decision/result this formula creates |
| Operand semantics | What each operand means in business language, not only its label |
| Source / authority | Which contract, configuration, bill, order, waybill, attendance, usage, source record, or manual adjustment provides it |
| Entity scope | Driver, vehicle, customer, supplier, contractor, tenant, order, resource, or period owner |
| Period | Daily, monthly, billing cycle, service period, employment period, policy period, or custom cycle |
| Eligibility | Which records/people/resources qualify and which are excluded |
| Comparison basis | For max/min/take greater: same scope, same period, same unit/currency/tax basis, and comparable states |
| Proration | New join/leave, partial period, inactive days, no source records, missing mapping, or mid-period rule changes |
| Adjustment timing | Whether adjustments happen before comparison, after comparison, inside one operand, or as separate lines |
| Boundary behavior | Zero/negative, missing operand, tie, cap/floor, rounding, precision, locked period correction |

Diagnostic questions:

- 保底是什么保底 / guarantee of what: monthly guaranteed pay, daily guaranteed amount, route guarantee, vehicle guarantee, contract minimum, or platform subsidy?
- What is it compared with: detail-calculated amount, order amount, waybill-based freight, usage value, score, quota, or another configured base?
- Are plus/minus items included before comparison or after comparison?
- Does the result need explainable line items so users can audit why the max/min branch won?

Example:

Driver payroll is incomplete if it only says `max(guaranteed pay, waybill-based freight) + adjustments`. It must define the guaranteed pay source and period, which driver/entity scope it applies to, how waybill-based freight is calculated and filtered, whether absent/new/left drivers get proration, whether adjustments are before comparison or after comparison, and how the chosen branch is shown on the payslip.

## Resource and Value Computability Guard

This is the non-finance version of money computability. Any controlled resource/value must be testable and conserved.

Require:

| Item | Must Define |
|---|---|
| Source | Where the resource/value comes from and who owns it |
| Authoritative anchor | The business event/field that proves the resource exists or belongs to a period/object; do not invent one from an easy timestamp |
| Pool | Current available/reserved/occupied/consumed/released amount or state |
| Trace | Source record/history used for audit or source-level control |
| One-to-one rule | Whether use/release must match a specific source trace or can come from the current pool |
| Combine rule | Whether multiple resources/pools can combine under partial insufficiency |
| State effect | Which object status, visibility, or downstream flow changes |
| Reversal | How cancel, timeout, reject, disable, revoke, refund, rebuild, or correction restores or preserves the resource |
| Role perspective | What actor, owner, counterparty, tenant, or admin can see and operate |

Examples:

- Coupon use is incomplete unless it defines grant source, validity anchor, reserve-on-submit, consume-on-paid, release-on-cancel, and whether several coupons can combine.
- Inventory reservation is incomplete unless it defines source stock pool, reserve timing, partial insufficiency behavior, release timeout, and oversell prevention.
- Content permission is incomplete unless it defines who grants it, which object it attaches to, who can see it, revoke behavior, and downstream visibility changes.

## Audit / Payment Disambiguation

When a doc says "create payment order" or "audit", ask:

| Question | Purpose |
|---|---|
| Is the object a cost item, payment order, fund adjustment, or only an accounting record? | Avoid mixing cost audit and payment audit |
| Does it trigger real external payment? | Avoid fake payment orders creating payment status noise |
| Who audits it? | Separate contractor confirmation, operator audit, finance audit, platform risk audit |
| What changes after approval? | Define side effects: payable amount, paid amount, fund freeze, ledger entry, external call |
| Which state axis changes? | Avoid putting business confirmation, audit, and payment into one status |

## Ghost Status Guard

Every status label must close across the PRD:

| Mentioned In | Must Also Appear In |
|---|---|
| Body text | State axis or explicitly marked as sub-label |
| Tab / filter | Business inclusion / exclusion rule and source state |
| State machine | Trigger, actor, previous state, next state |
| Exception handling | Recovery path and terminal behavior |

If a label such as "异常待确认" appears but is absent from the status enum / state machine, decide whether it is:

- a primary state;
- a sub-status / risk flag under another primary state;
- a Tab label composed from multiple states;
- stale wording to delete.

If a Tab, field, or正文 mentions an in-process state such as "处理中", "待确认", "部分成功", "失败待处理", or "退款中", the state machine must show the intermediate state, its trigger, retry / cancel behavior, timeout handling, and terminal transition. Do not jump directly from "发起" to "已完成 / 已失败" while displaying a middle state elsewhere.

Do not leave ghost states in final PRD正文.
