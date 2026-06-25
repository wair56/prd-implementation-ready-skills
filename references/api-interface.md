# API and Upstream Interface PRDs

Solves: wrapper API contracts, upstream service dependencies, request/response mapping, matching rules, cache state, error contracts, and measurable API behavior.
Read when: the PRD describes an API endpoint, wrapper API, upstream provider, external service call, callback, webhook, model gateway, search/query service, or system-to-system integration.
Pair with: `data-flow.md`, `non-functional.md`, `business-consistency.md`, and `review-checklists.md`.

An API PRD is not implementation-ready because the business rule is clear. It is ready only when another developer can map raw upstream fields into normalized fields, test every input combination, know whether returned evidence is exhaustive, and handle cache/error/timeout behavior without guessing.

## API / Upstream Contract Gate

Use this gate whenever the product wraps an upstream API, calls an external provider, or exposes a system API to clients.

Define the endpoint contract:

| Item | Must Define |
|---|---|
| Business purpose | What business question or action the API answers |
| Caller / consumer | Frontend, internal service, external client, job, webhook, partner, or admin tool |
| Endpoint and method | Public path, method, protocol, version, idempotency expectation |
| Auth and signing | Token, app key, signature, timestamp, nonce, IP allowlist, tenant context |
| Request schema | Field name, type, required, nullable, max length/count, enum, default, example |
| Response schema | Field name, type, meaning, nullable, enum, example, and whether it is stable |
| Upstream services | Provider, endpoint, method, auth, timeout, retry, rate limit, and call order |
| Mapping lock status | confirmed, pending provider document, pending联调, external source required, or out of scope |

For every upstream response used by rules, add a field mapping table:

| Normalized Field | Raw Field Path | Upstream Meaning | Required? | Empty Handling | Example | Mapping Lock Status |
|---|---|---|---|---|---|---|

Rules:

- Do not invent raw field path values. If provider docs are unavailable, write "pending provider document / 联调确认" and mark Mapping Lock Status as external source required.
- Do not let "以实际接口为准" be the only contract. It can appear only next to a table that says which fields must be locked before formal acceptance.
- A normalized field is not enough. The PRD must say where it comes from, whether it is required, what empty/null/missing means, and how tests will verify it.
- If upstream has several versions or environments, list which one the PRD targets and how sandbox differences are handled.

## Input Combination Matrix Gate

Use this when an API has optional fields, arrays, filters, modes, strategy flags, or "required but can be empty" lists.

| Combination | Valid? | Skip Stage / Call Stage | Business Behavior | Response / Error | Test Case |
|---|---|---|---|---|---|

Cover:

- Required fields that may be empty arrays or empty strings.
- Mutually exclusive, at-least-one, and all-empty combinations.
- Whether empty input means skip stage, no result, default query, or parameter error.
- Max list size and what happens beyond the limit.
- Per-item validation and whether errors point to array index, item ID, or field path.
- Partial success behavior when one item fails and others can still continue.

Red flag: `compareEnterprises=[]` or `comparePersons=[]` style inputs are declared valid but the PRD does not say which upstream calls are skipped and what result shape returns.

## Early Stop / Result Completeness Gate

Use this when a rule stops after the first match, priority match, cache hit, timeout, partial success, or cheaper upstream service.

Clarify whether early stop is a business choice or a performance optimization:

| Situation | Result Completeness | Meaning For Caller | Evidence Returned | Evidence Not Checked | Field / Wording |
|---|---|---|---|---|---|

Recommended field when evidence may be partial:

- `resultCompleteness = PRIORITY_RULE_RESULT`: priority rule matched; result is not exhaustive.
- `resultCompleteness = ALL_APPLICABLE_RULES_CHECKED`: all rules that should run under this input were checked.
- `resultCompleteness = PARTIAL_UPSTREAM_RESULT`: some upstream calls failed or were skipped by degradation.
- `resultCompleteness = FAILED`: no usable business result due to parameter/upstream/system failure.

Rules:

- `related=true`, `matched=true`, or `exists=true` must not imply exhaustive evidence unless the PRD explicitly says every applicable rule/source was checked.
- If an upstream call is skipped after an early hit, tell the caller what category of evidence is not exhaustive.
- If early stop is only for performance, decide whether clients can request full evidence through a flag or separate endpoint.

## Wildcard / Matching Semantics Gate

Use this when matching uses wildcard, fuzzy search, masked name, masked ID, partial phone, transliteration, normalization, synonym, or regex-like behavior.

| Matching Item | Must Define |
|---|---|
| Scope | Which rule/field allows wildcard or fuzzy behavior, and which rules treat symbols as literal |
| Pattern side | Whether caller input, upstream value, stored value, or either side can be the pattern |
| Literal behavior | How to represent a literal star or other special character |
| Multiple wildcards | Whether multiple wildcards are allowed and how each one matches |
| Character semantics | Unicode/Chinese/ASCII handling, normalization, spaces, case, punctuation |
| Length and count limits | Max field length, wildcard count, and abuse protection |
| Security boundary | Avoid raw regex injection; escape regex metacharacters unless raw regex is explicitly supported |
| Examples | Match and no-match examples, including masked and normal values |

Do not write only "supports * matching". Say whether `*` is a wildcard or literal star in each rule, whether it matches exactly one character or any length, whether multiple wildcards are allowed, and how ordinary regex characters are handled.

## Cache State Matrix Gate

Use this when API results, upstream responses, normalized records, search results, or eligibility decisions are cached.

| Cache Situation | Cache? | Cache State | TTL | Caller Behavior | Refresh / Fallback | Log / Trace |
|---|---|---|---|---|---|---|
| Successful result with data | yes/no | `SUCCESS_WITH_DATA` |  |  |  |  |
| Successful empty result | yes/no | `SUCCESS_EMPTY` |  |  |  |  |
| Successful result with missing non-critical fields | yes/no | `SUCCESS_WITH_MISSING_FIELDS` |  |  |  |  |
| Upstream failure | yes/no | `UPSTREAM_FAILED` |  |  |  |  |
| Cache read failure | no/continue | `CACHE_READ_FAILED` |  |  |  |  |
| Cache write failure | continue/fail | `CACHE_WRITE_FAILED` |  |  |  |  |
| Expired cache + refresh success | update |  |  |  |  |  |
| Expired cache + refresh failure | fail or stale fallback | `STALE_FALLBACK_USED` |  |  |  |  |

Rules:

- Distinguish successful empty result from upstream failure.
- Decide whether missing fields are cached as a usable degraded result or treated as failure.
- If stale fallback is allowed, response and logs must mark it; if not allowed, expired cache cannot silently become a success result.
- State whether cache key includes tenant, caller, input list order, normalized identifiers, rule version, and upstream version.
- State whether cache stores raw upstream payload, normalized payload, evidence snapshot, or final decision only.

## Error Contract Gate

Use this for every public API and any internal API consumed by another team/service.

Define one error envelope:

| Field | Meaning |
|---|---|
| HTTP status | Transport-level status |
| Business code | Stable product error code |
| Message | Human-readable message safe for caller |
| Details | Field-level, item-level, or upstream-level error details |
| Field path | Request or response field path that caused the issue |
| Array index | Index for list item validation or partial failure |
| Request ID | Trace ID for logs and support |
| Retryable | Whether caller should retry |

Minimum error rows:

| Error | HTTP Status | Business Code | Retryable? | Details Required |
|---|---|---|---|---|
| Parameter invalid | 400 |  | no | field path, array index when applicable |
| All-empty required inputs | 400 |  | no | involved fields |
| Upstream service failed | 502/504 |  | maybe | provider, stage, retry count |
| Partial upstream failure | 206/200/502 by product choice |  | maybe | failed item indexes and provider |
| Cache unavailable and no direct query fallback | 500/503 |  | maybe | cache operation |
| Rate limited | 429 |  | yes | retry after |
| System error | 500 |  | maybe | request ID |

Rules:

- Do not only describe exception scenes. Define the exact response shape that carries the scene.
- If the API promises partial success, response must carry per-item status, array index, and whether the final business decision is trustworthy.
- Avoid leaking raw provider secrets/errors to callers; log raw detail internally and expose stable business wording.

## Measurable API Non-Functional Gate

Use this when API behavior depends on speed, volume, reliability, upstream stability, or abuse control. Generic words like "reasonable", "fast", and "business acceptable" are not testable.

| Area | Must Define |
|---|---|
| Max list size | Max items per array/list and behavior beyond max |
| Max payload size | Request body, file, or response size limit |
| Concurrency | Per caller, per tenant, per provider, and per request-stage concurrency |
| Upstream timeout | Timeout per upstream call |
| Overall timeout | Whole request deadline |
| Retry | Retry count, retry interval/backoff, retryable errors |
| Rate limit | Per caller/tenant/IP/key limit and response behavior |
| Performance target | p95/p99 for cache hit, real upstream query, partial result, and batch request |
| Degradation | What still returns when cache/upstream/partial item fails |
| Observability | request ID, upstream request ID, stage timings, cache hit/miss, error stage |

Recommended MVP defaults can be proposed, but mark them as recommended defaults until confirmed. Once confirmed, write exact numbers in the PRD and acceptance criteria.

## Good-Enough Example

```text
The wrapper API checks shareholder evidence first. If shareholder evidence matches, it returns `related=true` and `resultCompleteness=PRIORITY_RULE_RESULT`; personnel evidence is not queried and the result is not exhaustive.
If the enterprise comparison list is empty but person comparison list is not empty, the shareholder upstream call is skipped and only the core enterprise personnel call runs.
The upstream field mapping table lists normalized field, raw field path, required flag, empty handling, example, and mapping lock status. Raw paths that are not in provider docs are marked external source required and cannot pass formal acceptance.
Successful empty upstream responses are cached as `SUCCESS_EMPTY`; upstream failures are not cached as usable business results. Expired cache can be used only when stale fallback is explicitly enabled and marked in the response/log.
```

