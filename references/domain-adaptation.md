# Domain Adaptation

Use this when the product is not obviously finance/logistics, or when finance examples may make the skill feel too narrow.

## Domain Adaptation Table

Many gates are universal product-thinking patterns with finance names. Translate the gate before applying it.

| Original Gate | Equivalent Concept | B2B SaaS / Admin | Tool SaaS | C-end Content / Social | Consumer App / Ecommerce | Skip When |
|---|---|---|---|---|---|---|
| Finance Fund-Flow Gate | Controlled resource/value loop | seats, credits, approvals, tenant quota | API quota, storage, import jobs, export capacity | visibility rights, creator revenue share, badges, boosts | coupons, points, inventory, membership benefits | no scarce/controlled resource changes |
| Settlement and Payment Closure Gate | Lifecycle closure after a business event | task completion, approval closure, subscription billing | job completion, webhook delivery, quota consumption | post publish/delete, moderation result, appeal closure | checkout, refund, renewal, delivery confirmation | action has no downstream status/resource effect |
| Payroll Gate | Recurring payout/reward/credit calculation | partner commission, agent incentive | affiliate payout, creator bonus, usage credit | creator payout, referral reward | cashback, rider/merchant payout | no recurring compensation, reward, or payout |
| Amortization Gate | Time-based recognition/allocation | annual contract value split by month | annual plan quota, prepaid credits, license period | membership benefit period, content package validity | subscription, warranty, annual service, prepaid package | value is consumed immediately and never reported by period |
| Invoice/Supplier Gate | Evidence, compliance, counterparty, receipt artifact | vendor document, contract evidence, audit proof | usage invoice, receipt, compliance file | takedown notice, moderation evidence | receipt, return proof, warranty document | no billing, audit, legal, or customer-service evidence needed |
| Cost Inclusion State Gate | Eligible source record boundary | billable tasks, approved expenses | completed jobs, valid usage records | qualified creator activity, valid moderation item | fulfilled order, shipped item, settled refund | no downstream calculation reads source records |

## Domain Defaults

| Domain | First Questions | Common Objects | Common Missing Flows |
|---|---|---|---|
| B2B finance/logistics | who pays, who receives, what period, what source proves the amount | account, bill, payment order, invoice, ledger | refund, reversal, source occupation, writeback |
| B2B SaaS/admin | tenant, role, data scope, approval, audit | tenant, workspace, member, role, task, report | cross-tenant identity, invite/disable, audit export |
| Tool SaaS | project/workspace, quota, background job, import/export | project, API key, job, artifact, webhook, quota | retry, partial success, stale result, quota refund |
| C-end content/social | user motivation, activation, retention, safety, privacy | user, content, comment, like, follow, report | delete/restore, block/report, notification preferences |
| Consumer app/ecommerce | purchase, fulfillment, refund, membership, customer service | order, cart, coupon, points, shipment, subscription | cancellation, refund, coupon lock/release, after-sale evidence |

Rules:

- Do not force finance words into non-finance PRDs. Translate money into controlled resource/value only when that resource exists.
- Do not skip a universal gate because the finance label sounds irrelevant. A coupon, quota, permission, membership benefit, content visibility right, or API credit can break in the same way as money.
- If a gate is not relevant, write the skip reason: "No controlled resource changes in this module; Resource/Value Flow Gate skipped."
- Keep examples domain-local. For a content app, talk about publish/delete/report/appeal and push preferences before talking about invoices.

