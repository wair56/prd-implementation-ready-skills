# Business Consistency and Computability

Use this when reviewing multiple PRD files, finance/payment/settlement requirements, or any flow involving funds, payment orders, audit, refunds, allocation, cost, profit sharing, or amount calculation.

## P0 Rule

Treat these as P0 blockers. Do not continue writing detailed PRD口径 until the user confirms them:

- A confirmed business anchor is contradicted, weakened, or replaced by generic wording.
- A derived implementation detail cannot trace back to user confirmation, a business anchor, source data, existing system behavior, research recommendation, or待确认.
- The same object/action/status is described differently in different files or sections.
- The document index, file references, reading order, or "higher than 01-07" style scope statement points to deleted, merged, or nonexistent docs.
- A topic is marked pending / out of scope / not in acceptance in one place but later written as confirmed detailed implementation.
- A money movement says funds increase/decrease/split/freeze/refund, but does not define the calculation rule.
- An amount field is used in a formula but has no authoritative source.
- A "payment order" is created but whether it triggers real payment is unclear.
- "Audit" is mentioned but the audited object, audit type, actor, and side effect are unclear.
- A status appears in body text, Tab names, filters, or exception handling but is missing from the state axis / enum / transition table.

## Cross-Document Consistency Scan

When reviewing multiple docs, actively compare the same term across files. Use a table like:

| Object / Term | Location A | Location B | Conflict | Why It Blocks | Recommended Question |
|---|---|---|---|---|---|

Check especially:

- business anchors, pricing/fund/source/status premises, and "obvious" domain facts;
- document links, module numbers, reading order, and "this doc overrides..." statements;
- object names: cost bill, payment order, reconciliation bill, profit bill, invoice, receipt, refund;
- actions: create, submit, confirm, audit, pay, refund, reverse, void, rebuild, push;
- states: pending audit, pending payment, paying, failed, refunded, exception pending confirmation;
- money words: available, frozen, occupied, released, deducted, transferred, allocated, shared.

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
