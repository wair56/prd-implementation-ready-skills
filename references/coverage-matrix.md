# Universal PRD Coverage Matrix

Use this after identifying the business goal and core objects. The purpose is to cover requirements generically without dumping a giant checklist on the user.

## Method

1. Extract core business objects from the user's notes.
2. Map each object through the coverage axes below.
3. Apply `necessity-gate.md`: keep only axes/details that materially affect business flow, implementation, testing, UX, risk, or operation.
4. Ask only the highest-risk missing decisions in the current stage.
5. Mark each item as confirmed, recommended default,待确认, or out of scope.
6. Re-run the matrix after the main flow changes.

## Coverage Axes

| Axis | What To Check | Typical Miss |
|---|---|---|
| Business purpose | Why this object exists and what outcome it supports | Writing pages before business goal |
| Source / creation | Where the object comes from, who creates it, sync/import/manual rule | "列表展示 X" without data source |
| Lifecycle | Creation, initial state, transitions, terminal states, rebuild/reopen | Starting at audit/payment midway |
| Information delta | What new information is introduced at each step, where it comes from, and where it is stored | State changes without explaining data source |
| Data flow | Source, transform, persistence, consumer, writeback, sync timing | Data displayed but not traceable |
| Linkage / side effect | Linked objects, modules, messages, reports, locks, permissions, reverse effects | "联动处理" without affected objects |
| Actor / permission | Who can view, create, edit, approve, cancel, export, jump | One backend role assumed too broadly |
| Time / period | Attribution date, month boundary, current month rule, late data | "按月汇总" without date source |
| Amount / formula | Source amount, formula, rate, rounding, split, snapshot | Fund or profit amount not computable |
| State axes | Business, audit, payment, invoice, receipt, push, exception separated | Mixed "status" field |
| Data fields | Required fields, optional fields, display source, editability | Field list copied without source |
| Query / list | Filters, default sorting, Tab meaning, include/exclude rules | Global doc contains page-level filters |
| Interaction | Empty/loading/disabled/submitting/success/failure/high-risk confirmation | "可勾选/默认全选" invented |
| Lock / occupation | When source records are occupied, released, rolled back | Reconciliation source reused twice |
| Reverse / exception | Cancel, reject, refund, red flush, void, retry, timeout, partial success | Only happy path written |
| Cross-system | Push, callback, duplicate, delay, sync timestamp, stale data | Sync data treated as instant truth |
| Cross-module | Source action, target page/Tab, params, default filters, permissions | "查看详情跳转" unspecified |
| Audit trail | Operation log, before/after value, operator, time, reason | High-risk finance action no trace |
| Necessity | Whether this object/detail must exist or can merge/defer/remove | Complete-looking but bloated PRD |

## Output Shape

Use a compact table during discussion:

| Object | High-Risk Missing Axis | Current Understanding | Recommended Default | Need User Confirm |
|---|---|---|---|---|

## Object Flow and Information Ledger

For each core object, do not stop at a status list. Build a step ledger that explains how the object moves and what information is added or changed along the way.

| Object | Step | From State | Action / Event | Actor / System | New Information Introduced | Information Source / Authority | Validation | Persisted To | Side Effect / Next Object | Failure / Retry | Confirm |
|---|---|---|---|---|---|---|---|---|---|---|---|

Rules:

- If a step introduces no new data, explicitly say "no new information; only status / lock / visibility changes" and define the side effect.
- If a step creates or updates another object, add that object to the core object list and run the coverage matrix for it too.
- Treat selected records, uploaded files, approval opinions, timestamps, amounts, invoice numbers, payment results, callback results, sync timestamps, exception reasons, jump parameters, and manual adjustments as information that needs a source.
- Separate authoritative source from copied/displayed fields. A copied value still needs a source and update rule.
- When new information can arrive late, be corrected, or be re-synced, define whether it updates the current object, creates a version, or is blocked after lock/audit.
- If the source of a new information item is unknown, mark that step "信息来源待确认" instead of writing the value as final PRD口径.
- If the step moves data across objects, systems, pages, or reports, also use `data-flow.md` to create a data flow map and linkage map.

Do not ask every axis for every object at once. Prioritize:

- money, contract, invoice, payment, settlement, approval;
- cross-system/source data;
- irreversible or hard-to-roll-back actions;
- state or formula contradictions;
- items the user marked "待产品确认" or "待更新".

## Confirmation Discipline

When the user says "你来 / 按最佳实践", give a recommended default and mark it as pending confirmation unless it is harmless and reversible.

When a note says "待产品确认 / 待产品提供 / 暂无 / 留空", preserve it as an explicit open item or out-of-scope rule. Do not silently complete it.
