# Stage Gates

Solves: the authoritative workflow order for PRD work, from depth selection through final confirmation.
Read when: starting a PRD conversation, deciding the next stage, or checking whether the assistant is jumping into details too early.
Pair with: `guardrail-checklist.md`, `coverage-matrix.md`, `research-and-flow.md`, and `output-structure.md`.

Use this file to keep the PRD process ordered. The most common failure mode is jumping into state tables, Tab filters, or exceptions before the user has confirmed what the business does and how the main flow should work.

At every stage, apply `necessity-gate.md`: add only the structure needed to make the product understandable, buildable, usable, or testable.

## Stage 0: Mode

If the user has not chosen a depth, recommend one:

| Mode | Use When | Output Depth |
|---|---|---|
| L2 快速 | Small requirement or early idea | Goal, scope, main flow, key rules |
| L3 标准 | Normal product requirement | Complete PRD with exceptions and fields |
| L4 详尽 | Finance, payment, contract, approval, settlement, multi-system workflow | Full objects, states, source docs, idempotency, data, UI, risk |

Default to L4 when money, contract, invoice, settlement, payment, approval, third-party callback, or cross-system data is involved.

## Stage 1: Business Understanding

Confirm before anything else:

- Context-first business understanding: create a context digest from the user-provided context before applying the workflow. Extract business facts, roles, objects, constraints, conflicts, gaps, and inferences; mark what is confirmed vs inferred.
- Domain adaptation table: use `references/domain-adaptation.md` to translate finance/logistics gates into the current domain. Decide whether the product is B2B SaaS/admin, tool SaaS, C-end content/social, consumer app/ecommerce, finance/logistics, or mixed.
- Deep business understanding package before module writing: external research, flow awkwardness analysis, micro-flow discovery, exception/reverse understanding, and business decision summary.
- Plain-language business context: what business this is, who it serves, and how value/data/money moves.
- Business goal: what real-world business problem this solves, separate from the system construction goal.
- Agreed business mainline and business anchors: the non-negotiable premises, obvious domain facts, pricing/fund/source/status rules, and constraints that later details must not override.
- Scope: what this phase does / does not do.
- Users and roles.
- Role glossary and core concept dictionary, especially counterintuitive business or financial concepts.
- Core business objects, then run `coverage-matrix.md` lightly to identify the highest-risk missing axes.
- Business observation lenses and micro-flow discovery: before module writing, scan role lens, object lifecycle lens, trigger/event lens, resource/value lens, data/source lens, page/task lens, exception/recovery lens, and tenant/organization lens to find small flows hidden behind the mainline.
- User story and scenario discovery: use `references/user-stories-scenarios.md` to capture important role/context/goal/trigger/preconditions/main path/alternate path/success criteria/emotion-friction scenarios, especially for C-end, consumer app, content/social, and tool SaaS products.
- Business risk: money, contract, invoice, privacy, approval, integration, migration.
- Product-level non-functional risk: use `references/non-functional.md` when performance target, SLA, data visibility latency, consistency level, retry, degradation, rate limit/abuse, security, backup, or observability affects the product promise.
- Repeated/cyclic operations: whether the business has "next run", "last run", "paid through", "handled through", "already issued", "prevent repeat", or "auto default next period" behavior. If yes, identify the likely progress-control field and ask/derive the loop before page fields are written.
- External research: same or adjacent domain patterns, mature products, existing docs/screenshots/code, SaaS/ERP/CRM/marketplace/admin practices, before locking the flow.
- Flow awkwardness analysis: redundant steps, offline coordination, repeated entry, late validation, one object carrying too many jobs, unclear ownership, missing recovery, and over-manual work.
- Exception/reverse understanding: reverse paths, rollback, cancel, reject, refund, void, red flush, retry, timeout, partial success, and what users do next.
- Existing systems or historical constraints.

Output:

Do not move to module writing until the deep business understanding package is complete enough for the selected depth: context digest, external research, flow awkwardness analysis, micro-flow discovery, exception/reverse understanding, and decision summary.

| Area | Current Understanding | Missing / Risk | Recommended Default | Need User Confirm |
|---|---|---|---|---|

Do not move to detailed states, exceptions, pages, or fields until the mainline and high-risk anchors are either confirmed or explicitly marked待确认.

## Stage 2: Main Business Flow

Only after Stage 1, reconstruct the flow:

| Flow Version | Summary | Benefits | Risks / Cost | Recommendation |
|---|---|---|---|---|
| User Original Flow |  |  |  |  |
| Recommended Flow |  |  |  |  |
| MVP Flow |  |  |  |  |

While reconstructing the flow, identify which core objects move through the flow, where new information is introduced, and which data/linkage paths are affected. For repeated/cyclic flows, explicitly identify what tells the system "where the previous run ended" and "what the next run should cover"; this is often the source of duplicate prevention and locked defaults. Keep this coarse at Stage 2; detail it in Stage 3.

If the work is v1 to v2 or modifies an existing product/PRD/implementation, run `references/version-evolution.md` before proposing the final flow: old rule, change intent, impact range, old vs new, migration, backward compatibility, release/rollback, and decision record.

Keep a micro-flow discovery ledger beside the main flow. It should list the likely small flows found during product discussion, their owner object, trigger, entry/surface, downstream impact, and whether each is in scope, merged into another flow, server-only, or out of scope.

Ask the user to confirm the flow before expanding detailed rules.

When recommending a better flow, explain whether it preserves or changes any business anchor.

## Stage 3: Rules After Flow

After the main flow is accepted, confirm:

- Extension basis: every derived rule, state, field, page, interaction, exception, and component traces to user confirmation, a business anchor, source data, existing system behavior, research recommendation, or待确认.
- Per-object coverage matrix: source/creation, lifecycle, actor, period, amount/formula, state axes, data fields, list/query, interaction, locking, reverse flow, cross-system, cross-module, and audit trail.
- Necessity of every proposed object, state, field, flow step, exception, and table; merge or defer anything that does not serve a confirmed user/business/process need.
- Object flow and information ledger: for every step, define action/event, actor/system, from/to state, new information introduced, authoritative source, validation, persistence target, side effect, failure/retry, and whether the step creates another object.
- Data flow and linkage map: for every important data item/action, define source/authority, transform/rule, validation, persisted object/field, consumer, writeback/sync timing, linked object/module, side effect, reverse/rollback, and failure behavior.
- Notification design gate: use `references/notifications.md` for every message, todo, push, email/SMS, webhook, bot message, recall, or user preference. Define trigger event, receiver, channel, dedupe/throttle, read/unread or task state, recall/update, deep link, failure/retry, and audit/log.
- Product-level non-functional gate: use `references/non-functional.md` to define performance target, SLA, consistency level, data visibility latency, idempotency/retry, degradation, concurrency/load, rate limit/abuse control, security/privacy, backup, and observability when they affect business behavior.
- Progress-control field loops: for every last/previous/paid-through/handled-through/progress field, define reader action, next-period/default derivation, editability, continuity/duplicate rule, advance event, non-advance events, reverse/correction, and recovery visibility.
- Core object lifecycle.
- State fields and transitions, including each object's creation point, initial state, pre-audit / pre-payment stages, actor, trigger, terminal states, and whether shared status labels really apply to every object.
- Cross-document consistency for repeated objects, actions, statuses, audit wording, payment wording, and money terms.
- Money movement and formula computability: source amount, split / allocation rule, rate, rounding, timing, snapshot, reversal, and side effects.
- Finance operation rules: period attribution, fund deduction priority, sync/incremental data handling, supplier disabled/history behavior, reconciliation source locking, invoice upload/status, receipt write-off, red flush/void rollback, and cross-module jump parameters.
- Source documents, unique constraints, idempotency.
- Data sources and authoritative sources.
- Permissions and visibility.
- Exceptions, boundary cases, concurrency, rollback.

## Stage 4: Pages and UI

After business objects and flow are clear, confirm:

- Target端 / touchpoint: PC backend, role portal, mobile/H5/mini app, message/todo center, API/system job, import/export tool.
- Menu and page structure.
- Page task closure before field lists: why the page exists, what the user sees first, what business question it answers, what primary action it enables, and what feedback/recovery appears after action success or failure.
- Page element inventory followed by field specification: every field-bearing filter, card, table, detail, drawer, modal/form, attachment area, and server-only field group defines concrete fields instead of only naming the container.
- Operation-field cross-check: operation inputs and validation name defined fields; every field has a user decision, operation, rule, trace, or downstream consumer.
- Page-level Tabs vs list filter Tabs.
- Page specs based on data volume, field count, lifecycle, frequency, role differences.
- UX carrier and component selection tied to workflow constraints: table, detail page, drawer, modal, wizard, batch bar, exception workbench, upload/download, timeline, operation log.
- Notification and task surfaces: message center, todo queue, push settings, channel preferences, deep links, read/handled states, recall/update behavior.
- design.md / ui-design-spec.md / existing system conventions.
- Interaction habits: empty, loading, disabled, submitting, success/failure, recovery, high-risk confirmation, reason input, audit trail.

## Stage 5: Full PRD

Before writing the full PRD, show a confirmation gate:

| Section | Confirmed Rule | Source | Still Open |
|---|---|---|---|

Run the outsider readability gate from `business-context.md`: a new developer/tester/product manager should understand the business, roles, key concepts, main flow, and scope before detailed rules.
Run the business anchor gate from `business-anchors.md`: confirmed anchors are preserved, and any change to them is explicitly confirmed.
Run product-design breadth gates: `references/domain-adaptation.md`, `references/version-evolution.md`, `references/user-stories-scenarios.md`, `references/non-functional.md`, and `references/notifications.md` when their trigger conditions apply.

Do not write final PRD until the user confirms "继续 / 按这个写 / 没问题 / 可以开始".
