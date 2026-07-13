# Review Checklists

Solves: final and lightweight PRD review, with quick entry points before the full checklist.
Read when: saying a PRD is complete, reviewing an existing/generated PRD, or choosing L2/L3/L4 review depth.
Pair with: `guardrail-checklist.md`, `stage-gates.md`, `data-flow.md`, and domain/topic references.

Use before saying a PRD is complete or when the user asks to review an existing PRD.

## L2 Quick Check - 10 Core Checks

Use this for lightweight requirements. If any answer is no, either fix it or move the work to L3/L4.

1. Business goal is clear and separated from system construction goal.
2. In/out scope is explicit.
3. User/role and trigger are known.
4. Main happy path is written in business language.
5. Important object/status changes are named.
6. Data source is visible for every displayed or calculated value.
7. Page or touchpoint explains why it exists, what users do first, and which visible elements/actions are on the page.
8. Primary operation defines success, failure, disabled reason, and recovery.
9. Any notification, resource, payment, or external sync has an owner and next step.
10. Open decisions are closed with recommended defaults or marked pending with reason.

## Stage and Flow

- Does the PRD stay lean: no needless entities, fields, states, pages, components, tables, or repeated prose?
- Did the business discussion start with context-first business understanding based on user-provided context, or did it fall into process-before-context / template-first PRD writing?
- Is there a context digest separating business facts, roles, objects, constraints, conflicts, gaps, and inferences before detailed workflow or module writing?
- Did the business understanding stage produce a deep business understanding package before module writing: external research, flow awkwardness analysis, micro-flow discovery, exception/reverse understanding, and decision summary?
- Is shallow business understanding visible as a blocker: no research, no awkward-flow analysis, no reverse paths, or no exception/recovery understanding before detailed PRD writing?
- Did the business discussion stage use business observation lenses before module writing, instead of discovering small flows only during final review?
- Is missing micro-flow discovery visible as a blocker: small operations, notifications, server-only flows, recovery paths, role handoffs, or page actions are either included with owner/trigger/entry/surface/downstream impact, merged deliberately, or marked out of scope?
- For each added object/status/field/page/component/flow step, is the user/business/process need clear?
- Are the agreed business mainline and business anchors written explicitly before detailed rules?
- Did the assistant preserve user-confirmed anchors when recommending best practices, rewriting, or polishing?
- If the user changed the business mainline or an anchor, was the change handled through an impact table before rewriting?
- Were affected business context, scope, objects, lifecycles, data flows, linkage, rules, pages, exceptions, acceptance criteria, indexes, and module references updated together?
- Were obsolete downstream口径 removed or marked superseded instead of coexisting with the new mainline?
- Does every extension beyond the mainline have a basis: user confirmation, anchor, source data, existing behavior, research recommendation pending user confirmation, or 待确认?
- Are obvious insider premises written clearly enough for a new reader?
- Can a new reader understand what business this is before reading rules?
- Is the business goal written separately from the system construction goal?
- Are role glossary, core concepts, and special/counterintuitive terms defined in plain language?
- Is human-readable wording preserved: business-readable language, user's wording, existing system labels, and confirmed business terms are used instead of invented terminology?
- Is every new term aligned with the requirement, with a source/basis, allowed synonyms, forbidden replacement, and confirmation state when needed?
- Is there page/document term drift: the same concept appears with different names across page label, PRD正文, business object, calculation field, formula component, status/operation wording, report/export, ledger/accounting effect, or fund destination?
- Do money/value/data flows among roles have a simple front-door explanation?
- Is every controlled resource/value closed loop before writing modules: source, reserve, allocate, consume, release, reverse, and role visibility?
- Are circular definitions avoided, especially for financial concepts?
- Is the business goal clear before details?
- Is scope explicit, including out-of-scope?
- Is the main flow confirmed before exceptions and states?
- Did the assistant compare original, recommended, and MVP flow when useful?
- Did it guide the user when flow was awkward?

## Product Design Breadth

- Is the PRD finance/logistics-only in examples, gates, or terminology when the actual product is C-end, content/social, consumer app, tool SaaS, or general B2B SaaS/admin?
- Was the domain adaptation table applied: each finance-named gate is translated to an equivalent concept or explicitly skipped with a reason?
- For v1 to v2 or existing-product changes, does the PRD include old rule, change intent, impact range, old vs new, migration, backward compatibility, release/rollback, and decision record?
- Are thin user stories visible as a blocker: stories name role/goal but omit context, trigger, preconditions, alternate path, success criteria, or emotion/friction?
- Do scenario walkthroughs cover happy path, common alternate path, exception/recovery path, and boundary/abuse path where relevant?
- Is there any missing product-level non-functional decision: performance target, SLA, data visibility latency, consistency level, idempotency/retry, degradation, concurrency/load, rate limit/abuse, security/privacy, backup, or observability?
- Is there any scattered notification design: notification/todo/push/email/SMS/webhook appears as a side effect but lacks trigger event, receiver, channel, deep link, dedupe/throttle, read/unread or task state, recall/update, user preferences, failure/retry, and audit/log?
- Do C-end or consumer scenarios include activation, retention, privacy/safety, unsubscribe/settings, cancellation/regret, and trust moments when they affect UX?
- Do tool SaaS scenarios include workspace/project, quota, background jobs, import/export, API key, webhook, retry, and partial success when relevant?

## Coverage Matrix

- Are all core business objects extracted from the notes, not only page/menu names?
- Did the assistant apply the necessity gate before adding new entities or details?
- For each core object, were source/creation, lifecycle, information delta, data flow, resource/value flow, linkage/side effects, actor, time/period, amount/formula, state axes, data fields, query/list, interaction, locking, reverse flow, cross-system, cross-module, and audit trail considered?
- For each core object, is there a flow / information ledger showing each step, actor/system, state change, newly introduced information, source/authority, validation, persistence target, side effect, and failure/retry?
- When a step creates or updates another object, was that downstream object added to the object list and covered too?
- Are open items marked as confirmed, recommended default pending user confirmation, 待确认, or out of scope?
- Did the assistant ask only the highest-risk missing decisions first instead of dumping every axis at once?
- Was the matrix re-run after the main flow changed?

## Reader-First Completeness

- Treat "Readable but incomplete" as a red flag, not as a successful simplification.
- Does the PRD preserve complete information for implementation, testing, UX, risk, operation, money movement, source data, state changes, exceptions, and acceptance?
- Was a coverage ledger used or mentally checked so every build-critical coverage item is Covered, Missing but required, Recommended default, Pending confirmation, Out of scope, or External source required?
- Are Missing but required items either written into the local section, converted to a recommended default pending user confirmation, or left as explicit Pending confirmation / External source required instead of silently omitted?
- Are details placed near the workflow, page, operation, object state, or server-only flow where the reader needs them?
- Did the assistant avoid fixing readability by deleting fields, states, source rules, eligibility conditions, period anchors, failures, reversals, writebacks, permissions, idempotency, audit, or acceptance?

## Confirmation Discipline

- Treat "Unconfirmed final wording" as a P0 issue.
- Is every final rule confirmed by the user, copied from a user-confirmed in-scope source without changing meaning, or accepted as a best-practice default after user confirmation?
- Are best-practice defaults marked as recommended until the user says OK?
- Is every inference, external-source import, market pattern, low-risk default, and assistant proposal not silently promoted into final wording?
- If the user said "you decide" or "best practice", did the PRD still show the recommended default and wait for confirmation before finalizing it?

## Data Flow and Linkage

- When a page, formula, operation, or server flow takes data from an external system, other module, upstream object, import, sync result, configuration, or source detail list, did it pass the Source Data Filter and Eligibility Gate?
- Does every referenced source dataset define business eligibility filter, UI query filter, include condition, exclude condition, owner scope, permission scope, source state, period window, availability, visible but unavailable behavior, unavailable reason, dedupe/occupation, snapshot, refresh timing, sync status, stale data, external failure, and correction behavior?
- Is there vague source selection wording such as "take data from", "sync from", "select source records", or "load upstream data" without source data filter?
- For API/interface PRDs, wrapper API, upstream provider, callback/webhook, or system-to-system service, did the PRD pass the API / Upstream Contract Gate?
- Does the API/interface PRD include upstream field mapping with raw field path, normalized field, required flag, empty handling, example, and mapping lock status?
- Does it define input combination behavior for required-but-empty arrays, all-empty inputs, skipped stages, partial success, max list size, and array index validation errors?
- If early stop or priority match exists, does the response or field wording state result completeness and whether evidence is not exhaustive?
- Does any wildcard, fuzzy, masked, or matching rule define scope, literal star behavior, multiple wildcard behavior, length/count limits, and regex injection protection?
- Does caching distinguish successful empty result, successful missing-field result, upstream failure, expired cache, stale fallback, and cache read/write failure?
- Does the API have an error contract with HTTP status, business code, message, details, field path, array index, request ID, and retryability?
- Are API non-functional requirements measurable: max list size, concurrency, upstream timeout, overall timeout, retry, rate limit, p95/p99, degradation, and observability?

- Is there a data flow map for high-risk data: source/authority, trigger, transform/rule, validation, persistence target, consumer, writeback/sync timing, and failure handling?
- Does every list, field, status, statistic, formula operand, selectable record set, and default value show a visible data source marker in正文 or its local table?
- Were visible data source markers checked for真实性 and consistency: source exists in the flow, contains the data used, authority is clear, timing is valid, and cross-doc wording is consistent?
- For every amount composition / amount breakdown, does each displayed amount component have source object/source field, source state, business period, snapshot, derived formula/operand, drilldown/backtrace, and detail sum reconciliation against the displayed total?
- Is any hidden data source buried behind labels such as cost basis, base amount, source total, order amount total, service fee, adjustment line, included detail list, or tax amount?
- If a formula, list scope, permission, receiver, or report dimension says current binding, current relationship, latest configuration, dedicated relationship, or real-time fetch, did it pass the Relationship Temporal Anchor Gate with relationship source, source authority, effective period, anchor time, business period, snapshot, rebuild policy, and correction behavior?
- Is there any unqualified real-time relationship that can change a frozen amount, statement, permission, settlement, notification receiver, or report result after the fact?
- For internal generated data, are trigger, input data, generation rule/version, persisted object, authority after generation, consumers, and correction/rebuild behavior clear?
- For non-finance resources such as inventory, coupon, quota, points, capacity, permission, entitlement, slots, or visibility rights, are source, authoritative anchor, pool, trace, reserve, allocate, combine, consume, release, partial insufficiency, and role perspective defined?
- For any attribution/share/occupancy/cost attribution metric, did the PRD classify whether it is a settlement hard constraint or an analysis metric, and name which business action consumes it?
- If an analysis metric is unavailable or only model-based, is it prevented from becoming a hard gate for invoice, settlement, payment, refund, release, or approval?
- Is there a linkage map for high-impact actions: linked objects/modules, linkage type, side effect, trigger timing, reverse/rollback, user visibility, and risk?
- For payment completion, are attached actions defined with idempotency, retry, partial failure handling, visibility, and reverse/compensation?
- For transaction completion / payment success / audit pass / refund success / batch creation / callback completion, is there a post-event writeback inventory covering target object, exact persisted field, source event timestamp, business period, writeback policy, consumer, idempotency, and reverse/correction behavior?
- For repeated/cyclic operations, did the assistant proactively identify any progress-control field such as last paid month, handled-through period, next period, eligibility marker, or last issued time, then define reader action, locked/default value, success event that advances it, non-advance events, duplicate prevention, and reverse/correction behavior?
- Missing progress-control loop is a P0 blocker when repeated/cyclic operations use last/previous/handled-through/paid-through/next/progress fields to drive the next action.
- Are "同步更新 / 自动回滚 / 联动处理" decomposed into affected objects, affected fields, timing, and failure behavior?
- For reverse actions, are released locks, reverted data, recomputed data, non-reverted data, and audit reason clear?

## P0 Consistency and Computability

- If multiple PRD files exist, are the same objects, actions, statuses, and money terms consistent across files?
- Are business anchors consistent across all module docs?
- Before rewriting a section, were the original business conclusions preserved or explicitly changed with confirmation?
- If a confirmed anchor changed, does every downstream module now follow the new anchor and no stale wording remain?
- Are document links, file ranges, module indexes, and reading order accurate after docs are merged, deleted, or renumbered?
- If an item is marked pending / out of scope / not in acceptance, is it prevented from appearing later as confirmed implementation detail?
- Does every fund movement define source amount, split rule, timing, side effect, and reversal?
- Does every controlled resource/value movement define source, pool, trace, one-to-one rule, combine rule, state effect, role perspective, and reversal?
- If a page says platform-side, finance-side, admin-side, or operator-side, are system side, organization scope, operator role, viewed subject, and data/action scope separated?
- If the platform can see all records, is that platform scope preserved instead of being narrowed to one operator role's self view?
- Are invoice, payment trace, exception record, allocation detail, attachment, adjustment, receipt, refund, approval task, or external sync record treated with a child object lifecycle when users need to query/operate/audit them?
- If a rule uses a single total for summary calculation, are detail dimensions such as vehicle dimension, customer/shipper, supplier, contract, route, project, cost type, and period still preserved for dimension analysis when useful?
- If a cost attribution or allocation source is missing, does the PRD avoid inventing a real-time allocation formula and keep it in post-event risk analysis unless the user confirms a reliable source?
- For annual/prepaid/cross-period costs, are cash payment and cost recognition separated, with monthly amortization or another allocation rule when needed?
- Does every cost entering downstream bills pass a Cost Inclusion State Gate: audit status, payment status, recognition state, locked cost period, inclusion/exclusion states, and rebuild behavior?
- Does every formula define operand semantics, source/authority, entity scope, period, eligibility, comparison basis, proration, rates, caps/floors, tier, threshold, rounding, snapshot, and zero/negative handling?
- For guarantee/保底, max/min/取大取小/take greater, base amount, detail-calculated amount, and adjustments, does the PRD answer guarantee of what, what is being compared, and whether adjustments happen before comparison or after comparison?
- Does every amount field have an authoritative source document, contract, bill, fee item, import, or manual adjustment source?
- If a payment order is created, is it clear whether it triggers real external payment or only records a business/accounting action?
- If a payment order is created from another module, does it carry payment order origin payload: source object, business type, payer/payee, receiver account, amount split, transaction metadata, trigger/status, and reverse/retry?
- Are platform service fee payer, receiver/platform pocket, rate snapshot, fee base, ledger effect, and collection/deduction timing defined?
- Does profit sharing define the profit-sharing funds waterfall, contractor own-funded advances, insufficient/continuing deduction, zero/negative profit sharing, and profit-sharing void fund rollback?
- If credit/support fund is central to the product, is centralized credit management specified for grant, occupation, repay, release, limits, page operations, and ledger reconciliation?
- Is every "audit" tied to a specific audited object, audit actor, audit type, and post-approval side effect?
- Are ghost statuses eliminated: every status in body text, Tab/filter, exception handling, and state machine is defined in one consistent place?

## Finance Operations

- Was the general Resource / Value Flow Gate applied first, instead of treating finance as a special pile of isolated rules?
- Does every monthly aggregation define attribution date, month boundary, current-month exclusion, late data handling, and lock/freeze behavior?
- For payments/recharges, are payer, payee, balance types, deduction priority, insufficient-balance handling, ledger effects, and reversal defined?
- For costs, are audit status, payment status, cost recognition state, locked cost period, late/missing source handling, and zero payroll line behavior defined where relevant?
- For supplier reconciliation, are supplier types separated, and do non-prepaid supplier flows include payment trace, payment status, and invoice management?
- For insurance, rent, license, maintenance package, or other annual/prepaid costs, is cash payment separated from monthly amortization/cost recognition?
- For driver payroll or freight settlement, are guaranteed pay, waybill-based freight, comparison basis, adjustment timing, period/entity scope, eligibility, and proration defined?
- For synced supplier/cost data, are source system, sync direction, sync mode, cursor, duplicate/update rule, disabled/deleted data handling, and sync timestamp defined?
- If a supplier is disabled after use, are historical visibility, future selectable scope, notifications, and effects on existing bills/contracts/drivers defined?
- For reconciliation, are selectable source records, grouping, locking, audit effects, invoice lifecycle, write-off, cancel/reject rollback, red flush, and void behavior defined?
- For reconciliation duplicate prevention, does the unique key include the real billing unit, and does it allow parallel projects / project/contract/billing unit / partial reconciliation when the business allows them?
- For customer/shipper billing, does monthly charter stay by vehicle when the commercial object is vehicle-based, and does transport-volume billing use shipper freight on the waybill or confirmed receivable source?
- For billing/profit-sharing, are vehicle, waybill, customer, supplier, contract, route, project, cost type, and period dimensions preserved for analysis when useful?
- For monthly charter billing, does cost basis explain its source cost bill, cost inclusion state, cost type scope, locked period, amortization/accrual treatment, drilldown/backtrace, and whether corrections rebuild the bill or create adjustment lines?
- For cross-module jumps, are source action, target menu/page/Tab, parameters, default filters, empty state, and permission behavior defined?
- If confirm and view-detail reuse a modal, are confirm mode, view mode, pagination/search, grouping, and amount-summary consistency defined?

## Source and Language

- Does every added interaction/default/filter/list/field have a source or待确认?
- Are data source, authoritative source, include/exclude conditions, and refresh timing clear?
- Are data sources visibly marked where the PRD uses them, not only implied in reasoning?
- Are internal sources such as generated records, snapshots, callbacks after idempotency, statistics, tasks, and copied fields treated as data sources with authenticity and consistency checks?
- Are technical fields, SQL conditions, code enums, and interface params converted to business wording?
- Are abstract terms such as 计价口径 / 统计口径 / 规则 / 策略 decomposed into fields, enums, IDs, versions, or source document sets?
- Does every high-risk term have one canonical term, one preferred display term, allowed synonyms, forbidden synonym, and page-visible label mapping when it appears in both pages and formulas?

## Rules and Objects

- Are core objects identified?
- Are state fields separated by axis: business confirmation, audit, payment, invoice, receipt, profit sharing, push?
- For each object, is the lifecycle traced from creation / initial state instead of starting midway at audit or payment?
- If an object has a pre-audit stage such as confirmation, calculation, matching, claim, signing, or submission, is that stage modeled separately?
- Are shared status labels valid for every listed object, or should some objects have object-specific state axes?
- Are state transitions clear, with trigger, actor, next state, side effects, forbidden scenes?
- Are source documents, unique constraints, idempotency keys, and writebacks defined?
- Are idempotency keys made of stable, comparable fields?

## Exceptions and Boundaries

- Network, permission, data, concurrency, duplicate submit, callback delay, callback repeat, callback disorder.
- Partial success and retry.
- Rollback, cancel, refund, reverse, rebuild rules.
- Empty, max, min, invalid input, file, amount, date, pagination.

## Page and UI

- Pages that display or select external/other-module source data show filter basis, result count, fixed business filters, editable UI filters, reset behavior, unavailable reason, sync/stale/external failure state, and source links.

- Does the 00 主文件 include a main file navigation map / information architecture mind map showing 端 / 模块 / 页面 / 功能点 before module details?
- Can a reader immediately tell where each function lives, including default entry, target role, permission, data scope, and cross-side / 跨端 links?
- Does each page pass the Page Task Closure Gate: why this page exists, what the user sees first, what business question it answers, what primary action it enables, and what feedback/recovery appears after success or failure?
- Does each page pass the Page Element Inventory Gate: every visible element/control is named with purpose, data source, permission/visibility, interaction, state behavior, and acceptance?
- Does each field-bearing element pass the Field Specification Gate: concrete fields are listed under filters/search, summary cards, table columns, detail drawers/pages, modal/forms, attachments/files, and server-only field groups where relevant?
- Does the Full-Document Field Coverage ledger count every in-scope role-specific page and server-only touchpoint, rather than treating one sample page or one module aggregate as complete?
- Does `in-scope field-bearing page count` equal `page field-specification coverage count`, and is the `uncovered page list` empty?
- For every covered page, does its concrete field count equal its complete field detail record count?
- Is there any element-only page specification where a container name such as "detail modal", "list", "amount and status", or "information area" is being counted as field definition?
- Does every field define business meaning, placement/control, source object/field, source authority, snapshot or real-time behavior, type/format/unit/precision, enum/option source, default/derivation, required/editable state, validation/error wording, visibility/disabled condition, writeback target, downstream consumer, permission/masking, and empty/unavailable behavior where relevant?
- Does the complete field detail record count equal the concrete field count, rather than providing details only for high-risk fields?
- Does every operation's input and validation name defined fields instead of saying "input according to page fields" / "按页面字段输入"?
- Are generic sources such as "module query API", "server amount field", "business master data", or "system generated" replaced by a concrete business source/field or explicitly marked pending source mapping?
- Does every page element have a field list or `No business fields` with an explicit reason?
- For readable HTML, does each compact field row expose its own complete definition through a per-field disclosure, with print/export preserving every field record?
- If any answer above is no, run Field Specification Gate before accepting the page.
- Are there missing visible elements: page header, breadcrumb, status tag, primary action, summary cards, filters, search, sort, tabs, table columns, row actions, batch actions, pagination, import/export, detail drawer, modal/form, timeline, operation log, attachments, empty/loading/error/disabled/submitting/success states?
- Does any page spec only describe large page regions such as "overview + list + detail" without table columns, operation entries, disabled reasons, default values, or recovery behavior?
- Is there any field-only page where columns/buttons are listed but why shown, decision supported, drilldown, operation surface, disabled reason, and recovery are missing?
- Target端 / touchpoint confirmed from user task and working context, not assumed.
- Menu/page structure confirmed from business reality, not style taste.
- Each page, Tab, component, and interaction is tied to a workflow constraint, user task, risk control, or comprehension need.
- For each important visible field/card/drawer/modal, is it clear why it is shown and what decision or action it supports?
- For each important visible label, is the term mapping clear between page text, PRD term, source field, formula component, ledger/accounting effect, fund destination, export wording, and acceptance?
- For each important operation, is there an operation-to-surface map: entry point, carrier, visible context, validation, confirmation, success result, failure recovery, and linked object/page?
- Page-level Tabs vs list filter Tabs are separated.
- UX carrier and components are specified for key pages/actions: table, detail, drawer, modal, wizard, batch action, exception workbench, upload/download, timeline/log.
- Important actions define primary/secondary actions, disabled reasons, confirmation copy, validation, recovery, and audit trace.
- High-density tables define key fields, default sort, pagination, frozen/important columns, empty state, and export needs.
- Cross-module links define target page/Tab, parameters, default filters, permission, and empty target behavior.
- design.md / existing UI conventions checked if available.
- Loading, empty, disabled, submitting, success, failure, high-risk confirmation covered.
- Exception workbench exists for missing vehicle/customer/supplier/contract/source mappings, with manual completion, validation, recalculation, and re-enter downstream flow.

## Output Placement

- Global overview contains only cross-module summary rules and main flow.
- Page-level Tab filters, field tables, button rules, and sort rules are in the relevant module/page.
- Detailed tables do not appear only in the index/global overview.

## Red Flags

- "通用上更正确" wording that replaces a user-confirmed business fact.
- Invented terminology, decorative names, or void-created term replacing the user's wording.
- "大家都知道 / 默认就是 / 这个不用写" around pricing, funds, roles, source data, or status.
- Detailed implementation that cannot point to a user discussion, anchor, source, existing behavior, or recommendation label pending user confirmation.
- "数据来源：系统生成 / 平台同步 / 自动统计" without trigger, input, rule, persisted object, authority, and consistency check.
- "按规则处理", "具体以实际为准", "计价口径", "有效单据" without definition.
- Long sections or tables that do not change behavior, testing, UX, risk, or implementation.
- New status/object/page/component added only for neatness, not because the flow requires it.
- `incomeStatus in (...)` or code enum in PRD正文.
- "可勾选", "默认全选", "自动处理" without source.
- Global overview stuffed with page-level Tab filter tables.
