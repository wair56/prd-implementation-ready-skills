# Non-Functional Requirements

Use this when product behavior depends on speed, reliability, visibility delay, consistency, security, scale, retry, degradation, or operations. These are product decisions, not only engineering choices.

## Product-Level Non-Functional Gate

| Area | Product Decision To Make | PRD Output |
|---|---|---|
| Performance target | which user action must feel instant, acceptable wait time, batch wait time | p95/p99 target or business-readable target by action |
| SLA / availability | which functions must be available and what outage means | availability target, maintenance window, customer-visible promise |
| Concurrency / load | peak users, records, jobs, imports, callbacks, messages | supported scale and overload behavior |
| Consistency level | strong consistency vs eventual consistency by action | what users see during delay and when data becomes authoritative |
| Data visibility latency | acceptable delay between event and visible list/report/statistic | real-time, T+N, scheduled, manual refresh, or delayed banner |
| Idempotency and retry | what can be retried safely and by whom | idempotency key, retry trigger, retry limit, duplicate behavior |
| Degradation | what still works when upstream/payment/search/notification/export fails | fallback UI, disabled action, cached view, queue, manual path |
| Disaster recovery / backup | recovery point and recovery time that product/business accepts | RPO/RTO, export/backup expectation, restore visibility |
| Security / privacy | sensitive data, masking, audit, consent, role scope | permission boundary, audit record, privacy controls |
| Rate limit / abuse control | how to protect public APIs, login, messaging, upload, content actions | rate limit, anti-abuse state, user feedback, appeal/recovery |
| Observability / alert | who needs to know about failures and what action follows | dashboard, alert, exception queue, owner role, escalation |

Rules:

- Tie each non-functional requirement to a user/business outcome. "Fast" is not a requirement; "checkout submit p95 <= 2s or show payment-pending state" is.
- For delayed visibility, tell users what is authoritative: source event, pending status, latest sync time, or report generation time.
- For retry, define whether repeated requests create duplicate records, reuse the same object, or create an adjustment/compensation.
- For degradation, say what the product does before asking users to contact support.
- Do not over-specify low-risk MVP screens. Apply this gate where reliability, trust, money, safety, collaboration, or repeated operations depend on it.

## API Measurability Shortcut

When the product exposes an API, wrapper API, upstream query, webhook, or callback, pair this file with `api-interface.md#measurable-api-non-functional-gate`.

At minimum, replace generic wording with numbers or confirmed defaults for max list size, max payload size, per-stage concurrency, upstream timeout, overall timeout, retry count, rate limit, cache hit p95, upstream-query p95, degradation behavior, request ID, stage timing log, cache hit/miss log, and upstream error observability.
