# Notifications

Use this when an event needs another person, role, system, or future self to notice, act, approve, recover, or understand a result.

## Notification Design Gate

| Item | Must Define |
|---|---|
| Trigger event | exact object/state/action that creates, updates, recalls, or cancels the notification |
| Receiver | role, user scope, tenant/org/project scope, owner/counterparty/admin/system |
| Channel | in-app, todo, email, SMS, push, webhook, bot, external callback |
| Purpose | inform only, require action, approve, retry, warn, confirm, recover, market/retention |
| Content/template | title, body variables, source object, amount/status/reason, privacy masking |
| Deep link | target page/Tab/detail/drawer, parameters, permission fallback, expired-link behavior |
| Priority | normal, important, urgent, blocking, digest-only |
| Dedupe/throttle | dedupe key, merge window, rate limit, digest rule, repeated event behavior |
| Read/unread / task state | read, unread, pending, handled, expired, failed, recalled |
| Recall/update | when message is withdrawn, corrected, superseded, or marked obsolete |
| User preferences | opt-in/opt-out, quiet hours, channel choice, required system messages |
| Failure/retry | send failure handling, retry, manual resend, fallback channel |
| Audit/log | who sent, when, source event, delivery result, read/handled result |

## Notification Scenario Table

| Event | Receiver | Channel | Message Purpose | Deep Link | Dedupe / Throttle | Read/Task State | Recall / Update | Preference | Failure Handling |
|---|---|---|---|---|---|---|---|---|---|

Rules:

- Do not hide notifications in a linkage row only. If a user must act from it or it affects trust, write the notification lifecycle.
- For C-end products, notification timing, frequency, privacy, quiet hours, and unsubscribe/settings are product behavior.
- For admin products, notifications often become tasks. Define owner, queue, deadline, retry, escalation, and handled state.
- For webhooks/external callbacks, treat them as notifications to a system: trigger event, payload source, idempotency, retry, failure visibility, and replay entry.

