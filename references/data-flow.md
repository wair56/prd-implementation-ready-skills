# Data Flow and Linkage Map

Use this after the main business flow is roughly understood, and again before final PRD output. The goal is to ensure a PRD explains how data moves and what each action links to, not only what pages and statuses exist.

## Four Flows

For complex requirements, separate four flows:

| Flow | Purpose | Output |
|---|---|---|
| Business flow | What users/systems do to complete the business outcome | Main process and branches |
| Object flow | How each business object is created, changes, and ends | Object lifecycle / state machine |
| Data flow | Where data comes from, how it is transformed, persisted, consumed, and written back | Data flow map |
| Linkage flow | What else changes when one action happens | Linkage / side-effect map |

Do not treat these as the same thing. A single business action may update multiple objects, create messages, change visibility, lock source data, update statistics, and trigger external sync.

## Data Flow Map

Use this table when data source, transformation, or downstream consumption is unclear:

| Data Item | Trigger / Step | Source / Authority | Input Conditions | Transform / Rule | Validation | Persisted Object / Field | Consumer | Writeback / Sync | Timing | Failure Handling | Confirm |
|---|---|---|---|---|---|---|---|---|---|---|---|

Check:

- Source: manual input, selected records, imported file, synced system, API callback, generated rule, configuration, historical snapshot.
- Authority: which source wins when copied data differs from source data.
- Transform: aggregation, split, mapping, rounding, dedupe, status conversion, field derivation.
- Timing: real-time, scheduled sync, manual refresh, after audit, after payment, after callback, T+N.
- Consumer: list, detail, downstream object, report/statistic, notification, permission, external system.
- Writeback: whether status, amount, lock, result, timestamp, or error reason is returned to upstream objects.

If a data item is displayed or used but has no source/authority/consumer, mark it as "数据流待确认".

## Linkage Map

Use this table for every high-impact action:

| Action / Event | Primary Object | Linked Object / Module | Linkage Type | Change / Side Effect | Trigger Timing | Reverse / Rollback | User Visibility | Risk | Confirm |
|---|---|---|---|---|---|---|---|---|---|

Common linkage types:

- Object linkage: create/update/void/rebuild another object.
- State linkage: one object's state changes another object's status, visibility, or selectable scope.
- Data linkage: copied fields, aggregated amounts, snapshots, source-detail occupation.
- Amount/ledger linkage: balance, frozen amount, payable/receivable, invoice amount, settlement amount.
- Permission/visibility linkage: disabled data still visible historically, selectable scope changes, role-specific buttons.
- Page linkage: jump parameters, default filters, detail entry, empty target behavior.
- Notification/task linkage: message, to-do, approval task, exception task.
- Report/statistic linkage: counters, success rate, amount totals, dashboard metrics.
- External linkage: push, callback, sync, retry, dedupe, timeout.
- Audit/log linkage: operation record, before/after value, reason, operator, time.

## Reverse Linkage

Every cancel, reject, refund, red flush, void, rollback, rebuild, disable, or re-sync must define:

| Reverse Action | Reverted Object | Released Lock / Occupation | Reverted Data | Recomputed Data | Not Reverted | Reason / Audit | Confirm |
|---|---|---|---|---|---|---|---|

If the forward action links to another object, the reverse action must say whether the linked object is rolled back, kept, voided, or rebuilt.

## Change Impact Questions

When a rule or flow changes, ask:

- Which existing objects are affected?
- Which source data must be reselected, recalculated, or re-synced?
- Which downstream objects need rollback, refresh, or versioning?
- Which pages, filters, counts, exports, and reports change?
- Which notifications, approvals, and external pushes are triggered or cancelled?
- Which historical records must remain visible for audit?

Avoid saying "同步更新 / 自动回滚 / 联动处理" without listing the linked objects, timing, and failure behavior.
