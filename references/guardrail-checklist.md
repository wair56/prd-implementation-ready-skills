# Guardrail Checklist

Solves: one staged source of truth for red flags, P0 blockers, and P1 warnings so SKILL.md and review files do not carry divergent blocker lists.
Read when: you are about to move stages, finalize a PRD, or review a generated PRD.
Pair with: `review-checklists.md`, `business-consistency.md`, and the current stage reference.

Priority:

- P0 Blocker: stop detailed writing until closed, explicitly marked pending with recommended default, or confirmed out of scope.
- P1 Warning: continue only if the tradeoff is stated and the missing detail is low-risk for the selected depth.

## Business Understanding

| Priority | Trigger | Required Action |
|---|---|---|
| P0 | Process-before-context or template-first PRD writing | Build context digest from user-provided context first |
| P0 | Business anchors contradicted or replaced by generic wording | Restore anchor or run change impact analysis |
| P0 | Existing product / v1 to v2 change lacks old rule, change intent, impact range, old vs new, migration, compatibility, rollback, decision record | Run Version Evolution Gate |
| P1 | Domain examples are finance/logistics-only for a C-end, content/social, consumer app, or tool SaaS product | Run Domain Adaptation Table |
| P1 | User stories are thin | Add context, trigger, preconditions, alternate path, success criteria, and emotion/friction |
| P1 | New term appears without user wording, existing system labels, confirmed business terms, or recommended naming basis | Run Human-Readable Wording Gate and Term Alignment Ledger |
| P1 | Same non-critical concept has different page label, PRD term, object field, formula wording, or export wording | Run Page-Document Term Alignment Gate |

## Flow Design

| Priority | Trigger | Required Action |
|---|---|---|
| P0 | Mainline changed but downstream objects/pages/data/rules were patched locally | Update global doc and affected modules together |
| P0 | External/other-module source data is used without source dataset, include/exclude conditions, source state, scope, period window, permission, availability, unavailable reason, snapshot, and refresh timing | Run Source Data Filter and Eligibility Gate |
| P0 | Missing API/upstream contract: API/interface, wrapper API, upstream provider, callback/webhook, cache, or error-code design lacks raw field mapping, input combination matrix, result completeness, cache state matrix, error envelope, or measurable limits | Run API / Upstream Contract Gate |
| P0 | Money or non-finance resource moves without source, authoritative anchor, pool, trace, reserve/consume/release, reversal, and role visibility | Run Resource / Value Flow Gate |
| P0 | Money/resource term changes across page label, PRD term, calculation field, ledger/accounting effect, or fund destination and could change payer, receiver, owner, balance, or settlement result | Run Page-Document Term Alignment Gate and Resource / Value Flow Gate |
| P0 | Formula uses guarantee, max/min, base amount, rate, tier, threshold, or adjustment without operand semantics | Run Formula Operand Semantics Gate |
| P0 | Amount composition names cost basis, source total, service fee, adjustment lines, or included detail list without component source lineage, snapshot, drilldown/backtrace, and detail-sum reconciliation | Run Amount Composition Source Lineage Gate |
| P0 | Amount, list scope, permission, receiver, or report dimension depends on current binding/current relationship/latest configuration/real-time fetch without relationship source, effective period, anchor time, snapshot, and correction behavior | Run Relationship Temporal Anchor Gate |
| P0 | Analysis metric, attribution, occupancy share, or cost attribution is used as a hard gate for settlement/payment/refund/release without reliable source and business action consumer | Run Business Action Input vs Analysis Metric Gate |
| P0 | Payment order lacks source business object, business type, payer/payee, receiver account, amount split, transaction metadata, trigger/status, reverse/retry | Fix Payment Order Origin Payload |
| P1 | Parallel, master-child, event-driven, cycle, or state-machine flow is flattened into one line | Split flow lines and mark convergence |

## Page Design

| Priority | Trigger | Required Action |
|---|---|---|
| P0 | Page lists fields/buttons but not why the page exists, what users see first, or how operations complete | Run Page Task Closure Gate |
| P0 | Page presentation stops at page purpose, large page regions, or "list + detail" without naming visible elements, controls, actions, states, and trace surfaces | Run Page Element Inventory Gate |
| P0 | Missing field-level detail: an element-only page specification names filters, cards, tables, drawers, modals/forms, files, or operations but omits their concrete fields, source/authority, display/input rules, validation, permission, writeback, or consumer | Run Field Specification Gate |
| P0 | Field table exists but complete detail records cover only selected/high-risk fields, or detail record count is lower than field count | Add a per-field complete definition and verify field count = detail record count |
| P0 | Missing-link data is marked exception but has no workbench/recovery/re-enter path | Add exception workbench or mark out of scope |
| P1 | A visible field/card/button has no user decision, action, risk-control, traceability, or acceptance purpose | Remove, move to detail/export, or explain why shown |
| P1 | Notification appears only as a side effect | Run Notification Design Gate |

## Final Review

| Priority | Trigger | Required Action |
|---|---|---|
| P0 | Missing post-event writeback: completion, payment success, audit pass, refund success, batch creation, sync completion, or callback changes existing objects but no writeback inventory exists | Use `data-flow.md#post-event-writeback-gate` |
| P0 | Progress-control field such as last paid month, handled-through period, next period, eligibility marker, or last issued time drives the next action but lacks reader action, default derivation, advance event, failure behavior, and correction behavior | Run Progress-Control Field Loop |
| P0 | Cost enters bill/report/settlement without audit status, payment status, recognition state, locked period, inclusion/exclusion states, and rebuild behavior | Run Cost Inclusion State Gate |
| P0 | Ghost status appears in body, Tab, filter, or exception but not lifecycle/state axis | Define the status or delete stale wording |
| P1 | Non-functional requirements are generic while performance target, SLA, data visibility latency, consistency, retry, degradation, rate limit/abuse, or backup changes product behavior | Run Product-Level Non-Functional Gate |
