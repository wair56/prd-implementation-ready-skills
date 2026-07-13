# Page, Menu, UX, and Component Guidance

Solves: page task closure, touchpoint choice, menu/page carrier decisions, component selection, and operation feedback/recovery.
Read when: a PRD describes pages, tabs, fields, buttons, workbenches, drawers, modals, mobile/PC portals, or UI behavior.
Pair with: `output-structure.md`, `data-flow.md`, `notifications.md`, and `review-checklists.md`.

Use this after the business objects and main flow are known. Page structure is part of broad UI / interaction style, not just visual design. The goal is to choose the right target端, page carrier, and components so users can understand the task, operate efficiently, and avoid mistakes. Apply `necessity-gate.md`: do not specify pages or components unless they solve a real user task, workflow constraint, risk, or comprehension need.

## Target端 / Touchpoint

Before designing menus or pages, decide where the user should complete the task.

| Touchpoint | Use When | Avoid When |
|---|---|---|
| PC backend / admin console | High information density, review, reconciliation, audit, configuration, batch work | Field user needs quick mobile action |
| Role-specific portal | External partner, supplier, contractor, customer, or service provider needs self-service | Internal-only operation or sensitive cross-tenant data |
| Mobile / H5 / mini app | On-site, driver, sales, approval on the go, photo/file capture, quick confirmation | Complex tables, heavy reconciliation, dense comparison |
| Message / todo center | User only needs to be notified, approve, retry, or handle exception | Full context or complex editing is needed |
| API / system job only | No human decision is needed and rules are deterministic | Business requires confirmation, exception handling, or audit |
| Import/export tool | Large batch correction, offline reconciliation, migration | Real-time guided operation is required |

Use:

| Scenario / Task | Primary User | Recommended端 / Touchpoint | Why | Fallback入口 | Confirm |
|---|---|---|---|---|---|

## Role vs Touchpoint Scope Gate

Do not confuse a system side, an organization scope, and an operator role. "Finance-side" is often an operator role inside the platform-side, not the whole platform scope. A platform-side page may let platform can see all subjects, while a finance operator role may only perform finance actions within that platform scope.

Define:

| Item | Must Define |
|---|---|
| System side / touchpoint | platform-side, customer portal, supplier portal, contractor portal, mobile, API/system job |
| Organization scope | tenant, platform, branch, project, contractor, customer, supplier, or cross-tenant |
| Operator role | finance, operations, admin, approver, auditor, external partner, system |
| Viewed subject | whether the page shows self only, all subjects, or a switchable subject such as contractor/customer/supplier |
| Data scope | what "all" means: all platform records, all tenant records, all authorized records, or only owned records |
| Action scope | which role can view, operate, export, audit, switch subject, and recover exceptions |

If a page says "财务端可切换承包人", check whether that really means "platform-side page; finance operator can switch contractor within authorized/all platform scope". Do not write a platform page as if it were only one finance user's personal account.

## Menu, Page, and UX Are Business Decisions

Do not choose target端, menu, page structure, or components by taste. Decide from:

- Business domain.
- Core objects and lifecycle.
- Data volume.
- Field count.
- Operation complexity.
- Role differences.
- Usage frequency.
- User's working context: desk work, mobile work, approval, exception handling, batch processing.
- Risk level: money, contract, invoice, privacy, irreversible action.
- Need for comparison, traceability, explanation, or guided operation.
- Existing navigation/design conventions.

## Page Task Closure Gate

Before listing fields, close the page task. A page is not complete because it has columns and buttons. It is complete when the user can enter the page, understand the situation, decide what to do, complete the operation, and recover from failure without guessing.

For each page, define:

| Item | Must Define |
|---|---|
| Why this page exists | the user job, business risk, operation, analysis, or exception it owns |
| What the user sees first | default summary, default Tab, default filters, empty state, and the first thing the user should understand |
| Business question answered | what the page lets the user judge: available amount, payable status, supplier risk, missing mapping, payment progress, invoice consistency, or another domain question |
| Primary path | the shortest normal path from entering the page to completing the main operation |
| Information hierarchy | summary card, main list, status tags, drilldown, detail drawer, operation area, log/timeline, and what belongs in each layer |
| Visible item intent | why it is shown, what decision or action it supports, and whether it should be always visible, folded, in a drawer, or only in export |
| Operation surface | where the user clicks, what context is visible before clicking, which modal/drawer/page carries the operation, and what validation/confirmation appears |
| Feedback/recovery | success state, failure state, disabled reason, retry entry, undo/void/correction entry, notification, and audit trail |

For visible information, do not stop at field names:

| Visible Item / Area | Why It Is Shown | What Decision Or Action It Supports | Data Source | Default / Sort / Grouping | Interaction | Empty / Disabled Reason | Drilldown / Export |
|---|---|---|---|---|---|---|---|

If no one can explain why a field, card, drawer, or button changes the user's decision, operation, risk control, traceability, or acceptance test, remove it or move it to detail/export.

For an amount breakdown area, add a local source explanation instead of only listing component labels. Each displayed amount component should show or link to its source explanation: source object/source field, source state, business period, formula/operand, snapshot time, and drilldown/backtrace. The UI must make it clear how a user reconciles the displayed total to source details or adjustment lines, and where rounding/exclusion differences appear.

Amount breakdown examples that need this treatment: cost basis, service fee rate, service fee, adjustment line, order amount total, included order list, tax amount, discount, credit consumption, remaining balance, or any generated subtotal.

When a displayed amount, list, filter result, or button eligibility depends on a relationship, show the relationship source instead of hiding it behind "current relation". Examples: dedicated vehicle, vehicle-customer binding, supplier-project mapping, user-tenant role, entitlement package, or content visibility group. The page should show the relationship source, effective date, source link, snapshot time/version when frozen, and the disabled/exception reason for missing, multiple, or overlapping relations.

## Page Element Inventory Gate

Do not stop at page purpose, big areas, or "list + detail". A page spec is incomplete until every visible element and every operation carrier has a named place, purpose, data source, permission/visibility rule, interaction behavior, and state behavior.

Apply this to every page, page-level Tab, workbench, portal screen, detail page, detail drawer, modal, import/export flow, and exception/recovery surface. The goal is not to make a decorative UI spec; it is to prevent missing buttons, filters, source explanations, audit traces, and recovery entries that implementation needs.

Start with a page-level inventory:

| Page / Surface | Element / Area | Required? | Why Shown / Decision Supported | Data Source / Metric Source | Permission/Visibility | Default Value / Default State | Interaction | Disabled Reason / Empty State | Feedback/Recovery | Acceptance |
|---|---|---|---|---|---|---|---|---|---|---|

Then scan the standard element set. Skip an element only when the reason is obvious from the business flow or explicitly stated.

| Element Set | Must Specify |
|---|---|
| Page header | title, breadcrumb, viewed subject switch, status tag, primary action, secondary actions, export/import entry, and permission/visibility |
| Summary cards | metric name, metric source, calculation period, refresh timing, click/drilldown target, empty state, and permission/visibility |
| Filters and search | filters, search fields, option source, default value, reset behavior, saved view if any, and whether filters affect summary cards |
| Tabs and status views | page-level Tabs vs list filter Tabs, count metric source, include/exclude conditions in business language, default Tab, and empty state |
| Table/list | table columns, column source, row status tags, default sort, sticky or important columns, pagination, selection, row actions, batch actions, and export scope |
| Operation bar | primary action, secondary action, batch action, enabled condition, disabled reason, validation, confirmation, submitting state, success feedback, and failure recovery |
| Detail drawer / detail page | sections, fields, related lists, source links, drilldown/backtrace, timeline, operation log, attachments, and cross-module links |
| Modal/form | trigger entry, fields, default value, validation, confirmation copy, cancel behavior, submitting state, success state, failure state, and audit/log |
| Import/export | template/source scope, permissions, file limits, parsing result, partial success, result report, download content, and retry/recovery |
| Empty/loading/error states | empty, loading, error, disabled, submitting, success, stale data, permission denied, and retry/refresh behavior |
| Audit and trace | timeline, operation log, operator, time, before/after value, reason, external serial number, attachment, and source object link |

For each user-visible element, answer four questions in business language:

- What does the user learn or decide from this element?
- What action can the user take from it, if any?
- Where does the value come from, and when can it be stale or empty?
- What happens when the element is hidden, disabled, failed, or corrected?

For every visible label on a page, keep term mapping close to the element. The page spec must make the page label, PRD term, source field, formula component, status/operation wording, report/export wording, and fund/resource destination use the same wording or an explicit mapping. Do not let a summary card say "平台服务费" while the formula says "服务费", the ledger says "平台收入", and the operation result says "利润扣减" unless the mapping explains that these are different layers of the same concept.

Use:

| Page / Surface | Visible Label | Term Mapping / PRD Term | Source Field or Object | Formula Component | Ledger/Accounting Effect or Fund Destination | Same Wording? | If Different, Why | Forbidden Synonym |
|---|---|---|---|---|---|---|---|---|

If a PRD says "the page contains a list, detail drawer, and action buttons" but does not name table columns, row actions, batch actions, pagination, header actions, filters, drawer sections, modal fields, empty/loading/error/submitting states, or permission/visibility, run this gate again.

When a page lists or selects data from another place, expose the filter basis: source dataset, fixed business eligibility filter, editable UI query filter, result count, summary count/totals, option source, default filters, reset behavior, sync status, stale data warning, external failure state, visible but unavailable rows, unavailable reason, and cross-module/source link.

## Field-Level Detail Handoff

The Page Element Inventory Gate answers which carriers exist. It does not define the business fields inside a filter area, table, summary card, modal/form, detail drawer, attachment area, or server-only data flow.

After the element inventory, run `field-specification.md#field-specification-gate` for every field-bearing element. Keep the field details local to that element. An element-only page specification is incomplete even when every page region has a purpose and data-source label.

Do not satisfy field detail with generated phrases such as "module query API", "server amount field", "display according to rules", or "input according to page fields". Name the real business source and field, or mark the source mapping as pending without inventing it.

Before finalizing the page, run the Operation-Field Cross-Check so every operation's input and validation references defined fields, and every field has a user/operation/rule/trace consumer.

## Operation-to-Surface Map

Every important operation needs a home in the UI. Do not list operations only in a rule table; users must know where they start them and how they see the result.

| Operation | Entry Point | Page Carrier | Context Visible Before Action | Input / Validation | Confirmation / Verification | Success Result | Feedback/Recovery | Linked Page / Object |
|---|---|---|---|---|---|---|---|---|

Use this especially when the operation involves money, account changes, supplier status, source-detail selection, external payment, notification, or downstream side effects. If an operation has no entry point, it is either missing from the UI or should be a server-only flow with a visible log/task/alert.

## Business View Inventory Scan

When writing a page, actively scan which view surfaces are needed for the business, not only which fields the database has:

| Surface | Use When | Examples |
|---|---|---|
| Summary card | The user needs an immediate total, current state, risk count, or next action | wallet total, available/frozen/occupied amount, pending payment count |
| Per-counterparty aggregation | The same subject has balances, obligations, or progress split by customer/supplier/project/tenant | customer prepaid total by customer, supplier payable by supplier, contractor funds by shipper |
| Drilldown | A total must be explainable and auditable | click customer balance to see receipt/payment/settlement ledger |
| Detail drawer | Users need related details without leaving list context | related-driver drawer for labor supplier, source detail drawer for bill lines |
| Verification modal | Operation is high-risk but short | recharge confirmation with verification code, bind card, change phone |
| Cross-object link | The user must continue work in another page | jump to payment order, invoice, source bill, exception workbench |
| Notification side effect | Another role must act after this operation | supplier disabled sends driver handling message; payment completion pushes payroll output |
| Payment trace fields | The operation must be reconciled later | receipt, payment batch number, payment serial number, payer/payee info, initiated time, completed time |

Account and supplier pages are common places where field-only PRDs miss UX. Scan for account operations such as recharge, withdrawal, bind card, contact, and phone changes; account views such as wallet total, per-counterparty aggregation, balance board, and detail ledger; supplier views such as tax number, rate, payment method, cooperation status, related-driver drawer, disable notification side effect, payment transition state, merge payment, second-level reconciliation time, and invoice tax consistency checks. Do not add these blindly; use the scan to ask or recommend the page surfaces that fit the current business.

## Common Page Organization Patterns

| Pattern | Use When | Avoid When |
|---|---|---|
| Few first-level menus + page-level Tabs | Same business domain, same users, related objects, left menu should stay shallow, each Tab is an independent work page | Tab objects have unrelated lifecycles or role groups |
| First-level + second-level menus | Submodules need frequent direct access | It creates deep left navigation for low-frequency pages |
| Single list + filter Tabs | Same object and same fields, only status/type differs | Tabs are different objects or different operation flows |
| Detail sections / detail Tabs | Same object has many details, logs, attachments | Each section has independent list workflow |
| Drawer / modal | Lightweight view/edit/confirm while preserving list context | Complex forms, independent lifecycle, long workflow |
| Wizard | Import, account opening, signing, batch processing | Simple single-step input |
| Dedicated detail page | Complex object, many related records, audit trail, repeated return visits | One-off quick check |
| Split pane / master-detail | Users compare list and detail repeatedly | Mobile or narrow screen |
| Batch operation bar | Users select many homogeneous records and apply same action | Per-record validation differs too much |
| Exception workbench | Users triage failures, retry, assign, or close exceptions repeatedly | Exceptions are rare and simple |

## Page Spec Output

| Business Domain | Object | User Task | Target端 | Lifecycle Independent? | Data / Field Complexity | Operation Complexity | Recommended Carrier | Why | Confirm |
|---|---|---|---|---|---|---|---|---|---|

If page-level Tabs are recommended:

| First-Level Menu | Page-Level Tab | Independent Work Page? | Own Filters | Own List | Own Detail | Own Actions | Default Entry | Permission Difference |
|---|---|---|---|---|---|---|---|---|

Page-level Tabs are independent work pages. List Tabs are filters. Do not mix the two.

## Side / Module / Page / Function Map

Before detailed page specs, build an information architecture map for the product or 00 主文件. It should answer where each function lives, which side owns it, and how users enter it.

Use:

| 端 / Side | 模块 | 页面 / Page | 功能点 | Default Entry | Target Role | Permission | Data Scope | Cross-side / 跨端 Link | Notes |
|---|---|---|---|---|---|---|---|---|---|

Rules:

- "端" is a touchpoint or system side: platform-side, customer portal, supplier portal, contractor portal, driver mobile, API/system job.
- "模块" is a business domain or object group, not just a left-menu label.
- "页面" includes page-level Tabs, detail pages, workbenches, drawers only when they are real work carriers, and server-only flows when there is no page.
- "功能点" is what users or systems do there: view, create, audit, reconcile, pay, sync, import, export, configure, recover exception, analyze.
- Put cross-side links on the map when one side creates data and another side views/operates it.
- If a function cannot be placed on this map, either it is missing a page/entry or it should be a server-only flow with visibility/log entry.

## Role Perspective and Resource Visibility

For any page involving a controlled resource/value, design the role perspective explicitly. This applies to finance and non-finance resources: inventory, coupon, quota, capacity, permission, entitlement, membership rights, appointment slots, task capacity, or content visibility.

| Role Perspective | Page Must Explain |
|---|---|
| Actor | What can I use, reserve, allocate, consume, release, revoke, or retry now? |
| Owner | Which pool/source is mine, what is available/reserved/occupied/consumed/released, and what changed it? |
| Counterparty | Which counterparty funds/resources/status explain this operation, disabled reason, or settlement/result? |
| Approver / operator | What source, trace, risk, exception, and downstream side effect am I confirming? |
| Platform/admin/tenant admin | Cross-role totals, abnormal locks, source traces, tenant/organization boundaries, and recovery entries |

Do not show only the acting user's local field when the operation depends on counterparty context. If an action is disabled because another party's pool, lock, status, quota, permission, or trace is insufficient, stale, frozen, occupied, or missing, the page must show that reason in business language.

## Centralized Credit and Settlement Operations Page

When credit/support fund/frozen fund/profit-sharing/payment rules affect several modules, decide whether the platform side needs a centralized page instead of hiding operations in separate detail pages.

Use a centralized page when users must manage or audit cross-object pools, especially: support credit grant, credit occupation, repay, release, freeze/unfreeze, contractor own-funded advances, shipper/customer frozen funds, platform service fee collection, abnormal locks, and downstream profit-sharing recovery.

Page must define:

| Area | Must Show / Support |
|---|---|
| Summary cards | current credit pool, available, occupied, frozen, repayable, released, outstanding, platform pocket/service fee totals |
| Subject switch | contractor, shipper/customer, supplier, project, contract, tenant, or all platform records depending on role scope |
| Ledger table | source, trace, business object, amount, status, period, counterparty, operator, time, reason, downstream object |
| Operations | grant, adjust, occupy, repay, release, freeze/unfreeze, export, retry side effect, jump to source bill/payment/profit-sharing record |
| Disabled reason | insufficient pool, locked period, occupied by source detail, pending payment, missing counterparty, permission, duplicate/parallel bill conflict |
| Exception recovery | failed payment completion side effect, missing source mapping, stale callback, negative/zero settlement, void rollback pending |

If the page is not needed, state which existing page/workbench owns each operation and how platform/admin users still see the whole fund/credit situation.

## UX and Component Selection
 
For each page or important action, specify the UX carrier and component behavior. Do not stop at "add a page" or "add a button".

| Need | Recommended Component / Pattern | Must Specify |
|---|---|---|
| Scan many records | High-density table, sticky key columns, column configuration | primary fields, frozen columns, default sort, pagination, empty state |
| Compare statuses / progress | Status tags, progress steps, timeline, state reason tooltip | state meaning, reason source, terminal states |
| Confirm high-risk action | Confirmation modal, reason input, secondary verification | risk copy, affected objects, irreversible effects, audit log |
| Edit simple fields | Inline edit or small modal | editable fields, validation, save/cancel, permission |
| Edit complex object | Dedicated edit page or drawer with sections | required fields, validation, draft, submit result |
| Long multi-step setup | Wizard / stepper | steps, back/forward, draft, validation per step |
| Select source records | Selection table, filter panel, selected summary | data source, eligibility, default selection, lock/occupation |
| Handle exceptions | Exception queue, reason classification, retry button, processing log | owner, retry rules, terminal handling |
| Upload/download | Upload component, file list, template, progress, result report | file type, size, duplicate, parse failure, partial success |
| View audit/history | Timeline / operation log / change diff | operator, time, before/after, reason |
| Navigate across modules | Link/button with parameters and default filters | target page/Tab, params, permission, empty target |

Do not add a component just because the design system has it. Tie each component to the process constraint it solves: comparison, selection, validation, risk confirmation, context preservation, exception recovery, traceability, or guided setup.

For each page, define:

| Page / Action | User Goal | Main Information Hierarchy | Key Components | Primary Action | Secondary Action | Error/Empty/Loading | Risk Control | Confirm |
|---|---|---|---|---|---|---|---|---|

## UX Quality Gate

Before accepting a page design, ask:

- Can the user tell what to do first within 5 seconds?
- Is each page, Tab, component, and action necessary for a user task or process constraint?
- Are high-frequency actions fewer clicks than low-frequency actions?
- Are dangerous actions visually and procedurally harder than safe actions?
- Can users understand why an action is disabled?
- Can users recover from failure without leaving the page?
- Does the page show enough context before asking for confirmation?
- Are dense tables readable without hiding critical fields?
- Are mobile/portal/API-only choices justified by the user's actual working context?

## design.md / Existing UI

If a project has design.md, ui-design-spec.md, screenshots, components, or prior pages, read them before recommending UI.

Extract:

- Page hierarchy.
- Navigation depth.
- Table density.
- Tab behavior.
- Drawer/modal usage.
- Toast/error/empty/loading rules.
- High-risk confirmation.
- Component library conventions.
- Form/table/filter/drawer/modal/button patterns.
- Responsive behavior.

Default broad UI recommendation for B端后台:

> 沿用现有后台风格；列表高信息密度；复杂详情优先详情页或抽屉；高风险动作必须二次确认。
## Good-Enough Example

For an exception workbench:

```text
Purpose: operations staff resolves supplier consumption lines that cannot match a vehicle.
First view: summary cards for unmatched count and oldest pending age, then a table grouped by supplier and sync batch.
Primary action: open detail drawer, select vehicle from eligible list, confirm mapping, and trigger recalculation.
Disabled reason: source line is locked by a confirmed reconciliation bill or the operator lacks project permission.
Recovery: failed recalculation stays in the same row with retry and error reason; success moves the line back to the normal cost flow.
```

This is enough because it explains why the page exists, what users see first, where they act, which data is needed, disabled reasons, and recovery.
