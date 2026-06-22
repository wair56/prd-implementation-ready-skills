---
name: prd-implementation-ready
description: Use when writing, reviewing, or filling gaps in PRDs, product requirements, BRDs, functional specs, or SaaS/admin/tool/consumer workflows that must become implementation-ready. Especially useful for business flows, user scenarios, object states, page UX, data sources, permissions, notifications, non-functional decisions, exceptions, finance/payment, formulas, and writebacks.
---

# PRD Implementation Ready

## What This Skill Does

Use this skill to figure out the product with the user before writing the document. The output must be readable for humans and complete enough that a developer or AI can implement it without asking another business question.

Use it for PRDs, BRDs, functional specs, SaaS/admin workflows, C-end/content products, tool SaaS, consumer apps, finance/payment/settlement flows, product iteration from v1 to v2, or any requirement where state, data, permission, page UX, notification, non-functional product behavior, or cross-module linkage can break implementation.

When NOT to use it: one field label, copy edit, visual-only tweak, pure technical implementation task, or a single-step requirement with no state/data/source/cross-role risk.

Core bar:

- **Context-first business understanding**: user-provided context is the first source of truth. Build a context digest before applying the workflow.
- **Product quality plus completeness**: a complete bad product is still bad. Research the domain, test whether the flow feels natural, and improve clumsy UX instead of only filling tables.
- **Reader-first module PRD structure**: write Module purpose, Main flow, Page presentation, Key operations, Risks and exceptions, Page implementation details, Server-only automatic flows, and acceptance in that order.
- **Close pending decisions** when safe: use business anchors, source data, existing behavior, or low-risk best-practice defaults. Leave pending only when the user explicitly says it is undecided, sources conflict, or no safe default exists.
- **Do not invent terms**: use the user's wording, existing system labels, or confirmed business terms.
- **Human-readable wording**: PRD正文 must stay business-readable and aligned with the requirement. Do not replace user terms with abstract, decorative, or void-created terms.

Default first response shape: context digest based on the user's context; deep business understanding plan; one-sentence business goal; known/unknown scope, users, objects, and risks; initial flow shape; likely small flows from observation lenses; inferred high-risk control loops for repeated/cyclic operations; 3-6 confirmation questions with recommended defaults.

## How To Run It

`references/stage-gates.md` is the authoritative workflow. In plain text: stage-gates.md is the authoritative workflow. The progressive workflow summary is:

1. Business understanding first: context digest, deep business understanding package, external research, flow awkwardness analysis, micro-flow discovery, exception/reverse understanding, business anchors, and scope.
2. Flow shape next: identify single-line, parallel, master-child, state-machine, event-driven, cycle/periodic, or mixed flow. For parallel flows, name convergence points instead of flattening the work.
3. Navigation and diagrams before detail: the main PRD / 00 file should include a side/module/page/function navigation map and a business flow atlas before detailed rules.
4. Rules after flow: object lifecycle, data flow, source documents, writebacks, idempotency, permissions, exceptions, boundaries, and rollback.
5. Pages after business shape: page task closure, target touchpoint, page carrier, component choice, interaction states, and feedback/recovery.
6. Final writing after confirmation: show confirmed rules, recommended defaults, open high-risk decisions, and out-of-scope boundaries.

Diagram requirements: always consider flow shape, parallel flow convergence, business flow atlas, Mermaid flowcharts, `stateDiagram-v2`, object status changes, and diagrams before detailed rules when the PRD spans multiple objects/modules or has lifecycle/state risk.

For existing products or v1 to v2 changes, run the Version Evolution Gate before rewriting: old rule, change intent, impact range, old vs new, migration, backward compatibility, release/rollback, and decision record.

For module docs, keep details near the page/action/server flow where they matter. Page implementation details must include a page element inventory, fields, data source, status/writeback, permissions, idempotency, validation, error/empty/disabled states, feedback/recovery, and acceptance. Server-only automatic flows stay inside the module with trigger, input source, processing rule, persisted object, status/writeback, idempotency/retry, failure/compensation, visibility/log, and acceptance.

## Reference Router

Read only what the current stage needs.

| Stage / Situation | Read |
|---|---|
| Business understanding stage | `references/stage-gates.md`, `references/business-context.md`, `references/research-and-flow.md` |
| Domain fit, C-end, consumer app, content/social, or tool SaaS | `references/domain-adaptation.md`, `references/user-stories-scenarios.md` |
| Existing behavior, v1 to v2, migration, or compatibility | `references/version-evolution.md`, `references/business-anchors.md` |
| Rules design stage | `references/coverage-matrix.md`, `references/data-flow.md`, `references/business-consistency.md` |
| Finance, funds, costs, reconciliation, invoice, payroll, profit sharing | `references/finance-operations.md` then the indexed finance topic file |
| A metric may be action input or only analysis/risk-control, such as cost attribution, usage share, occupancy, contribution, or profitability | `references/data-flow.md`, `references/business-consistency.md` |
| Amount composition or amount breakdown has hidden component sources such as cost basis, source total, service fee, adjustment lines, included detail list | `references/data-flow.md`, `references/business-consistency.md`, `references/page-ui.md` |
| Formula, list scope, amount basis, permission, or report dimension depends on binding, dedicated relationship, current relationship, latest configuration, or real-time fetch | `references/data-flow.md`, `references/business-consistency.md`, `references/page-ui.md` |
| Page design stage | `references/page-ui.md`, `references/output-structure.md` |
| Notifications, todos, pushes, emails/SMS, bot messages, webhooks | `references/notifications.md`, `references/data-flow.md` |
| Product-level non-functional decisions | `references/non-functional.md` |
| Source language, unsupported details, technical wording | `references/source-language-guards.md` |
| Public release, bilingual wording, or confusing Chinese/English terms | `references/terminology.md`, `references/source-language-guards.md` |
| Final review stage | `references/review-checklists.md`, `references/guardrail-checklist.md` |

Gate authority:

- Authoritative gate: `references/data-flow.md#post-event-writeback-gate`.
- Authoritative gate: `references/data-flow.md#resource--value-flow-gate`.
- Finance topic index: `references/finance-operations.md`.
- Unified staged red flags and P0/P1 blockers: `references/guardrail-checklist.md`.

## Guard Rails

Stop and return to the workflow when any of these appears:

- Process-before-context or template-first PRD writing: starting from a generic structure without digesting user-provided context.
- Module writing before the deep business understanding package is complete enough for the selected depth.
- Mainline-only thinking that misses business observation lenses, likely small flows, role handoffs, server-only flows, recovery paths, or page actions.
- Finance/logistics-only examples used for a C-end, consumer app, content/social, or tool SaaS product without the Domain Adaptation Table.
- A v1 to v2 change treated as a fresh PRD without old rule, change intent, impact range, old vs new, migration, compatibility, rollout, rollback, and decision record.
- Thin user stories that omit context, trigger, preconditions, alternate path, success criteria, or emotion/friction.
- Missing product-level non-functional decisions when performance target, SLA, data visibility latency, consistency, retry, degradation, or abuse control changes behavior.
- Scattered notification design: a notification, todo, push, email, SMS, bot message, or webhook appears only as a side effect without trigger event, receiver, channel, dedupe, read/task state, recall/update, preferences, failure handling, and deep link.
- Invented terminology: a new term appears without user wording, existing system labels, confirmed business terms, or a clear recommended naming basis.
- Analysis metric used as an action hard gate: attribution, occupancy, or cost attribution blocks settlement/payment/refund/release without reliable source and a named business action consumer.
- Hidden amount composition source: amount breakdown names cost basis, source total, service fee, adjustment lines, or included detail list without source lineage, drilldown, snapshot, and detail-sum reconciliation.
- Unqualified real-time relationship: amount, list scope, permission, or report dimension says current binding, current relationship, latest configuration, or real-time fetch without relationship source, effective period, anchor time, snapshot, rebuild policy, and correction behavior.
- Field-only page design: columns/buttons are listed but why shown, what decision or action they support, operation surface, disabled reason, and recovery are missing.
- Large-region page design: page presentation stops at page purpose, big areas, or "list + detail" without a complete visible element inventory for page header, summary cards, filters, tabs, table/list, actions, drawers/modals, logs, import/export, and interaction states.
- Created downstream records are listed after an event, but exact persisted field writebacks on existing objects are missing.

For the full staged list, use `references/guardrail-checklist.md`. Any P0 blocker means stop detailed writing until the issue is closed or explicitly marked pending with a recommended default.
