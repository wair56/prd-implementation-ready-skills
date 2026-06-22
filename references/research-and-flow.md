# Research and Flow Guidance

Research and flow推演 happen during discussion, not only before writing the final PRD.

## Research Inputs

Use current information to look for:

- Mature SaaS/product patterns in the same or adjacent domain.
- Domain adaptation patterns across B2B SaaS/admin, tool SaaS, C-end content/social, consumer app/ecommerce, and finance/logistics. Translate finance-named gates into local product concepts before applying them.
- SaaS / multi-tenant / 多租户 patterns, including same customer across multiple tenants, same legal entity with multiple roles, tenant isolation, shared identity, data ownership, cross-tenant permissions, migration, merge/split, and compatibility with external IDs.
- GitHub/open-source systems that reveal object models, workflows, approvals, billing, settlement, invoices, tasks, or admin UI patterns.
- Existing project docs such as design.md, ui-design-spec.md, interface docs, screenshots, old PRDs.

Do not invent products, repositories, version numbers, or links. If browsing is unavailable or sources are not confirmed, say it is based on general product practice.

## Possibility Scan During Research

Research is not only looking for one similar product. Enumerate plausible business variants and decide which ones the current PRD must support, block, or leave for later. Search broadly across mature SaaS, marketplace, ERP, CRM, billing, approval, and partner-portal patterns when the domain may involve organizations, accounts, money, contracts, or external partners.

For SaaS / multi-tenant / 多租户 compatibility, include:

| Possibility | Why It Matters | Support / Block / Later | Required Product Rule | Source / Evidence | Confirm |
|---|---|---|---|---|---|
| Same customer across multiple tenants | A customer may cooperate with several tenants or business units |  |  |  |  |
| Same legal entity has multiple roles | One company/person may be customer, supplier, contractor, payer, or receiver |  |  |  |  |
| Tenant isolation vs shared identity | Prevent data leaks while avoiding duplicate identity confusion |  |  |  |  |
| Cross-tenant data ownership and permissions | Decide who can view, export, aggregate, transfer, or operate records |  |  |  |  |
| Tenant migration / merge/split / re-bind | Existing data, contract, invoice, and account history must remain understandable |  |  |  |  |
| External system ID compatibility | Third-party systems may send IDs that are global, tenant-scoped, or unstable |  |  |  |  |

When sources are limited, state the scan as an assumption and close with a recommended default where safe, for example: "MVP tenant isolation is strict; same legal entity may exist in multiple tenants as separate business records; no cross-tenant aggregation except platform admin reports." Only leave it open when user confirmation is truly required.

## Use Research to Improve the Flow

Research should affect:

- Main flow.
- Object decomposition.
- State machine.
- Permissions.
- Source document and idempotency design.
- Exception handling.
- Page organization and interaction habits.

Output research as decision support:

| Reference / Pattern | What It Teaches | Current Requirement Impact | Adopt / Avoid | User Confirmation |
|---|---|---|---|---|

## Flow推演 Checklist

For every user-described flow, infer:

- Who triggers it.
- Preconditions.
- What object is created or changed.
- What state changes.
- What source document is used.
- What gets written back.
- Who handles failure.
- What can be automated.
- What is manual and why.
- Which user story or scenario walkthrough proves this flow is natural: role, context, trigger, preconditions, main path, alternate path, success criteria, and emotion/friction.
- Which notifications/tasks/webhooks and non-functional product decisions affect flow closure.

## Guide the User

When the flow is awkward, say so plainly and give a better option:

| Current Flow Point | Problem | Better Flow | Why Better | Cost / Tradeoff | Confirm |
|---|---|---|---|---|---|

Examples of awkward flow:

- Repeated manual entry.
- One status field mixing approval, payment, business, and push states.
- User must coordinate offline.
- Failure says "contact tech" with no product-side fallback.
- A later step could have been validated earlier.
- The same object is doing too many lifecycle jobs.

Offer options, but do not replace user confirmation for high-risk choices.

## Deep Business Understanding Stage

External research, flow awkwardness analysis, micro-flow discovery, and exception/reverse understanding belong in the business understanding stage. They are not final review cleanup.

Before locking the flow, do external research across the same or adjacent domain: mature SaaS, ERP, CRM, marketplace, approval, billing, settlement, admin-console, partner-portal, or open-source patterns. Use it to identify likely objects, states, pages, failure handling, and missing flows. If sources are unavailable, label the conclusion as general product practice or a recommendation.

Run flow awkwardness analysis early:

| Awkwardness | What To Check |
|---|---|
| Redundant steps | user repeats input, confirms the same thing twice, or moves data manually |
| Offline coordination | users must message/call/export outside the system to finish the job |
| Late validation | errors are discovered only after submit/payment/reconciliation |
| Mixed lifecycle object | one object carries approval, payment, invoice, push, and recovery without clear state axes |
| Unclear owner | no role owns retry, correction, exception, or final closure |
| Over-manual work | deterministic work is forced into human review without business judgment |
| Missing recovery | failure tells users to contact someone but gives no product-side path |

Also understand exception/reverse behavior before writing modules: failure, rejection, cancel, refund, void, red flush, timeout, retry, partial success, rollback, reverse paths, and what users do next.

## Observe Business Before Freezing Flow

During product discussion, do not treat the user's first mainline as the whole product. Use business observation lenses to look for micro-flow discovery candidates before module writing:

- Role lens: another role must know, approve, receive, retry, or recover.
- Object lifecycle lens: an object can be draft, locked, failed, disabled, reopened, voided, or archived.
- Trigger/event lens: something happens by callback, schedule, timeout, sync, or correction, not only by a user click.
- Resource/value lens: a controlled amount, quota, permission, capacity, or visibility changes.
- Data/source lens: a displayed value has an authority, a copied value, a generated value, a writeback, or late correction.
- Page/task lens: a user needs a place to notice, decide, drill down, act, and see feedback.
- Exception/recovery lens: a failure needs a product-side path, not "manual handling" in prose.
- Tenant/organization lens: one real-world subject can appear across tenants, orgs, projects, contracts, or roles.

When a small flow is found, decide whether it is a module, a page operation, a drawer/modal, a server-only flow with visibility, a notification, a log, an exception recovery entry, or out of scope. This keeps business understanding ahead of document structure.
