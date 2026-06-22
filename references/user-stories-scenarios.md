# User Stories and Scenario Walkthroughs

Use this when the PRD needs to explain who the product is for, how the user reaches the page, and what a real usage moment feels like. This is especially important for C-end, content, social, consumer app, and tool SaaS products.

## User Story Gate

A useful user story is not only "As a role, I want X." It should make the decision context testable.

| Field | Must Define |
|---|---|
| Role | user, admin, creator, operator, approver, buyer, seller, workspace owner, guest, system |
| Context | what happened before this moment; device/touchpoint; first-time vs returning user |
| Goal | what the user is trying to finish or understand |
| Trigger | click, notification, scheduled moment, external event, error, invitation, checkout, publish, comment |
| Preconditions | login, permission, membership, quota, source data, object state, network, version |
| Main path | shortest successful path and what the user sees first |
| Alternate path | branch for common variation: no data, permission missing, quota insufficient, rejected, cancelled, offline |
| Success criteria | user-visible result, persisted object/state, downstream side effect, metric or acceptance signal |
| Emotion/friction | anxiety, trust, speed, clarity, privacy, regret, confidence, or motivation that changes UX decisions |

## Scenario Walkthrough

Use one table per important scenario:

| Scenario | Actor | Entry | Steps | Data Used | Page State | System Action | Failure / Edge | Acceptance |
|---|---|---|---|---|---|---|---|---|

Rules:

- Include one happy path, one common alternate path, one exception/recovery path, and one boundary/abuse path when relevant.
- For C-end products, include activation and retention scenarios: first use, return use, reminder/push entry, cancellation/regret, privacy/safety concern.
- For tool SaaS, include workspace/project setup, invitation, quota usage, import/export job, retry, webhook/API failure, and collaboration handoff where relevant.
- For admin products, include high-frequency scanning, batch operation, audit, exception handling, export/report, and cross-role handoff.
- Do not invent personas with decorative names. Use simple role labels and business context.

## Story Output Shape

| Story | Role | Context | Goal | Trigger | Preconditions | Main Path | Alternate Path | Success Criteria | UX Notes |
|---|---|---|---|---|---|---|---|---|---|

If stories feel thin, stop and ask what real situation the user is in before the screen appears.

