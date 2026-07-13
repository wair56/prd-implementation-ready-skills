# Field Specification

Solves: field-level completeness inside filters, lists, cards, forms, modals, drawers, details, files, and server-only business data.
Read when: a page element displays, collects, calculates, filters, selects, saves, exports, or writes back business data.
Pair with: `page-ui.md`, `output-structure.md`, `data-flow.md`, `api-interface.md`, and `review-checklists.md`.

Use this after the Page Element Inventory Gate. Page structure and field detail are different levels:

`page -> element -> field`

- Page: the user task and information hierarchy.
- Element: the carrier, such as a filter area, table, summary card, modal, or detail drawer.
- Field: the actual business value users see, enter, select, compare, or cause the system to persist.

A container name such as "monthly bill detail modal" or "driver payroll table" is not a field definition. A page element inventory can be complete while the PRD is still impossible to implement because the fields inside each element are unknown.

## Field Specification Gate

Run this gate for every field-bearing element. Cover at least:

- Filters and search: query fields, option source, default filters, reset behavior, and fixed business filters.
- Summary card and metric areas: metric field, calculation source, period, refresh timing, drilldown, and empty behavior.
- Table columns: visible columns, status tags, derived values, default sort, selection, row context, and export fields.
- Detail drawer / detail page: sections, fields, source links, related lists, timestamps, files, and audit information.
- Modal/form: input fields, read-only context, defaults, option source, validation, confirmation, and persisted result.
- Attachments, import/export, and generated files: file field, filename, file source, status, limits, visibility, and failure reason.
- Server-only business fields: event input, normalized value, persisted field, status/writeback, and downstream consumer when no page participates.

Each element must have either:

1. A complete field list; or
2. `No business fields`, with an explicit reason, such as a static divider or pure navigation label.

Do not count an element name, tab name, drawer name, modal name, or the phrase "list fields" as field coverage.

## Full-Document Field Coverage Gate

Run field specification coverage at document scope, not only on the page used as an example. A sample page does not satisfy full-document coverage, even when that sample is excellent.

Build a page coverage ledger before accepting the PRD:

| Module | End / Role | Page or Server Touchpoint | Has Business Fields? | Element Inventory Complete? | Concrete Field Count | Complete Detail Count | Pending Dimensions | Coverage Status |
|---|---|---|---|---|---:|---:|---|---|

Apply these rules:

1. Count each role-specific page separately. Platform and customer views of the same module are two pages when their data scope, fields, operations, or permissions differ.
2. Count server-only business touchpoints separately when callbacks, jobs, calculations, normalized fields, writebacks, or downstream effects have no user page.
3. Every in-scope field-bearing page must have an element inventory, a concrete compact field list, and one complete detail record per field.
4. A shared component may reuse an authoritative field definition, but each consuming page must name the shared component, inherited fields, page-specific visibility, permission, data scope, and exceptions. Do not silently mark sibling pages covered.
5. A page with no business fields must say `No business fields` and explain why. Static navigation does not excuse missing fields inside the destination page.
6. An external iframe or embedded product still needs boundary fields: local subject/context, launch URL/session, load/error state, external field families, callback/result fields, and any external contract dimensions that remain pending. Do not invent the external field contract.
7. Scan the implementation or source material page by page where available: filters, cards, table columns, drawers, forms, request/response fields, local state, files, operation context, error displays, and post-event writebacks. Page titles alone are not a field inventory.

Before saying the document is field-complete, report and verify exactly:

```text
In-scope field-bearing page count = N
Page field-specification coverage count = N
Uncovered page list = empty
Per-page field count = per-page complete detail record count
```

If the counts differ, list every uncovered page by `module + end/role + page name` and continue writing. Do not report a module-level aggregate as page coverage.

## Compact Field Table

Keep the first reading layer compact and local to the page element:

| Element | Field | Business Meaning / Why Shown | Control / Placement | Data Source | Display / Default | Edit / Validation / State |
|---|---|---|---|---|---|---|

This table should let product, design, development, and test understand the page without opening a separate global dictionary.

The compact row must still name the real business field. Do not merge several unrelated values into one row such as "amount and status" unless they are one deliberate composite display and the child fields are listed in expandable implementation details.

## Expandable Implementation Details

Every business field must have a complete field detail record. The detail record count must equal field count for the page or server-only field group. High-risk fields only is not complete field coverage; high-risk fields may receive more examples or deeper rules, but ordinary visible, filter, trace, and read-only fields still need the complete record below.

Each compact field row must expose a per-field disclosure or an equally direct local link to its own complete definition. A group-level disclosure that contains details for only some fields does not pass this gate.

| Detail Item | Must Define |
|---|---|
| Business meaning | What the field means, which object owns it, and what user decision or action consumes it |
| Location / control | Page, element, column/section, control type, read-only display, selector, upload, status tag, or derived display |
| Source object / field | Exact business object and source field; add API response path only when the contract is known |
| Source authority | Which source wins when page data, snapshot, master data, import, or upstream API differs |
| Snapshot or real-time | Whether the field follows current source data or freezes at creation, submission, approval, payment, or another named event |
| Type / format | String, amount, integer, percentage, date, datetime, enum, file, identifier, or composite display; include display format |
| Unit / precision | Currency/unit, decimal precision, rounding, timezone, date boundary, and zero/negative behavior where relevant |
| Enum / option source | Allowed values, business labels, option dataset, include/exclude filter, disabled option reason, and default sort |
| Required / optional | Which operation/status makes it required, optional, conditionally required, or forbidden |
| Default / derivation | Literal default, copied source, formula, previous value, progress field, or no default |
| Editable / read-only | Who can edit, in which states, whether later changes rebuild existing records, and why a locked value is read-only |
| Visibility / disabled condition | Role, tenant/organization scope, object state, feature flag, source availability, and the reason shown when hidden or disabled |
| Validation / error wording | Range, length, format, cross-field rule, uniqueness, server revalidation, and user-facing error wording |
| Empty / unavailable behavior | Empty display, unavailable reason, stale warning, source failure, retry/refresh entry, and whether the action is blocked |
| Writeback target | Persisted object/field, overwrite/append/accumulate policy, writeback event, and reverse/correction behavior |
| Downstream consumer | Later page, operation, duplicate check, status rule, report, notification, settlement, export, or external push |
| Permission / masking | View/edit/export permission, sensitive-data masking, audit access, and cross-tenant boundary |
| Example | One realistic display/input example when format or meaning is easy to misunderstand |

Inherited group defaults may reduce repeated prose only when the group names the shared value, every affected field explicitly inherits it, and field-level exceptions are written beside the field. Inherited group defaults cannot leave a detail dimension absent or hide that a field still needs a decision.

Do not turn the main document into a fourteen-column wall. For interactive HTML, show the compact field table first and provide a per-field disclosure from each compact field row. For print/export, expand every field's complete detail record so offline readers do not lose information.

Before accepting the page, report or verify:

```text
Field count = N
Complete field detail record count = N
Fields with explicit pending dimensions = named list
Fields without a detail record = 0
```

## Field Source Quality Gate

The following are placeholders, not usable data sources:

- `module query API` / `某某查询接口`
- `server amount field` / `服务端金额字段`
- `business master data` without naming the object and relation
- `system generated` without trigger, inputs, rule, persisted field, and authority
- `display according to rules` without naming the rule and source state

Use business-readable source wording first, then technical mapping when known:

```text
Business source: monthly bill -> billing month, frozen when the bill is created.
Implementation mapping: endpoint/response field, only when confirmed by code or interface documentation.
```

If the source object or field cannot be verified, mark `External source required` or `Pending source mapping`. Do not invent a field path to make the table look complete.

When the value comes from another module, external system, selection list, or configuration, also run the Source Data Filter and Eligibility Gate in `data-flow.md`. When raw request/response mapping matters, also run `api-interface.md`.

## Operation-Field Cross-Check

After field writing, run the check in both directions:

1. Every operation's `Input and validation` names each defined field it reads or accepts. `Input according to page fields` / `按页面字段输入` is forbidden.
2. Every operation result names the fields and objects it creates, changes, clears, locks, or writes back.
3. Every visible/input field has a named user decision, operation, rule, trace purpose, or downstream consumer. Remove decorative fields with no consumer.
4. Every amount, status, time, identifier, reason, file, option, and relationship mentioned in a flow or operation exists in the field list or a server-only field table.
5. Every selector defines its option source, eligibility filter, unavailable reason, and snapshot/refresh timing.
6. Every calculated field names operands, source fields, formula/version, rounding, and detail reconciliation where relevant.

If this cross-check fails, return to the element and field specifications before claiming the page is complete.

## Field Families That Need Extra Care

| Field Family | Extra Decisions |
|---|---|
| Identifier / subject | authoritative ID, display name, snapshot, copy/search behavior, cross-tenant scope |
| Amount / rate / quantity | source, currency/unit, precision, formula, zero/negative, snapshot, reconciliation |
| Status / progress | state axis, business label, trigger, actor, timestamp, available actions, reverse path |
| Date / month / time | business meaning, anchor event, timezone, period boundary, format, late/cross-period behavior |
| Selector / relation | option source, eligibility, current vs historical relation, multiple/missing relation handling |
| File / attachment | file type/size, upload result, filename, preview/download, status, permission, failure/retry |
| Reason / remark | required condition, length, history, visibility to other roles, audit and masking |
| Sensitive data | collection purpose, masking, view/edit/export permission, retention and audit |

## Good-Enough Example

For a monthly bill confirmation modal, "monthly bill detail and confirmation modal" is only the element. A good field layer separately defines billing month, viewed subject, each amount component, total amount, tax amount, source-detail count, current status, submission note, and confirmation action context. Each row names its source object/field, snapshot timing, format, editability, validation, permission, empty behavior, and downstream consumer. The operation then references those exact field names instead of saying "input according to page fields".
