# Research and Flow Guidance

Research and flow推演 happen during discussion, not only before writing the final PRD.

## Research Inputs

Use current information to look for:

- Mature SaaS/product patterns in the same or adjacent domain.
- GitHub/open-source systems that reveal object models, workflows, approvals, billing, settlement, invoices, tasks, or admin UI patterns.
- Existing project docs such as design.md, ui-design-spec.md, interface docs, screenshots, old PRDs.

Do not invent products, repositories, version numbers, or links. If browsing is unavailable or sources are not confirmed, say it is based on general product practice.

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
