# Finance Fund Flow

Solves: fund, balance, credit, support fund, prepayment, recharge, refund, freeze, occupation, release, pool/trace, and role ledger closure.
Read when: money or finance-like credit can be granted, reserved, deducted, mixed, returned, released, or shown to multiple roles.
Pair with: `data-flow.md` for the authoritative Resource / Value Flow Gate and `page-ui.md` for ledger/page visibility.

## Finance Fund-Flow Gate

Use this before final PRD wording for any balance, credit, support fund, prepayment, recharge, refund, freeze, occupation, release, write-off, profit sharing, or payment action.

Core version:

| Item | Must Define |
|---|---|
| Source pool | which account/pool owns the amount |
| Usable amount | available/frozen/occupied/reserved formula |
| Action nature | real external payment, operating payment, internal ledger registration, fund adjustment, quota allocation, or accounting-only record |
| Priority and mix | deduction/occupation order, mixed funding, partial insufficiency, single-source boundary |
| Ledger effect | which pool increases, decreases, freezes, releases, or occupies |
| Closure and reverse | write-off, release, refund, recovery, profit sharing, cancel, timeout, correction |
| Visibility | which role ledger shows it and whether counterparty funds are visible |

Do not invent attribution anchors or source pairing. Receipt time, entry time, sync time, approval time, or payment completion time is only authoritative when the business object proves it. Separate **pool balance** from **source trace**: pool controls current availability; trace explains history. A one-to-one pairing is required only when the business needs source-level locking.

## Fund and Payment Priority

For every role-to-role payment, recharge, or occupation, define payer/payee, balance types, deduction priority, whether mixed funding is allowed, insufficient balance behavior, ledger effect, and reversal.

Mixed funding is a first-class case. Do not reduce a flow to single-source "choose available balance or support fund" unless confirmed. If partial insufficiency can happen, define priority, split ledger, and disabled reason when combined funds are still insufficient.

## Payment Nature and Recharge Classification

| Nature | Meaning |
|---|---|
| Operating payment | money actually goes to a supplier/driver/partner/platform |
| Supplier prepayment / pre-recharge | real money is prepaid to a supplier, then allocated internally |
| Internal ledger registration | no money leaves the system; only internal balance/quota/usage is recorded |
| Quota allocation | existing quota/balance is assigned from one object to another |

Do not call an operating payment or supplier pre-recharge an internal ledger registration merely because the amount is later allocated to vehicles/assets/users.

## Source-Specific Closure Paths

| Source | Closure Path |
|---|---|
| Platform-granted support credit | can be returned directly only when rule allows; may be deterministic no-audit and auto-completed |
| Shipper/customer funds | usually released through refund, reconciliation/write-off, or settlement |
| Contractor own balance | may be deducted, withdrawn, refunded, or used for operating payment |
| Supplier prepayment | consumed by allocation, reconciliation, refund, or reversal |
| Frozen funds | event freezes, event consumes/releases, reverse reopens or preserves |

Use deterministic no-audit automation when the rule is deterministic and usable amount is sufficient. Add audit only for human judgment: exception amount, dispute, risk review, source conflict, or policy override.

## Frozen Funds Closed Loop

Frozen funds closed loop must define what event freezes the amount, whether it participates in reconciliation and profit sharing, which event consumes or releases it, which reverse action reopens the lock, and which terminal state preserves history. Do not write that frozen funds are excluded from reconciliation unless the business explicitly confirms the value loop still closes.

## Centralized Credit and Ledger Views

If support credit, operating credit, frozen shipper funds, contractor advances, or quota can be granted/occupied/repaid/released, add a centralized credit management entry. It should cover grant, occupation, repay, release, limits, page operations, ledger reconciliation, source trace, and abnormal locks.

Role ledger views must show more than "my balance":

| View | Must Show |
|---|---|
| Contractor | own available balance, platform-granted/support credit, shipper/customer funds, available/frozen/occupied, source trace |
| Platform | contractor fund status, shipper fund status, support credit grant/occupation/return, abnormal locks |
| Shipper/customer | receipt balance, refundable balance, frozen/used/written-off amount, linked contractors |
| Supplier | prepayment/recharge balance, consumption/allocation, reconciliation/payment/refund status |

## Good-Enough Example

```text
Operating recharge uses contractor available balance first, then platform-granted support credit.
If available balance is partial, the remaining amount can use support credit; if combined funds are still insufficient, submit is disabled with the split shortage reason.
Payment completion creates ledger entries for both pools and updates the supplier prepayment balance.
Cancel before payment releases occupied amounts; payment failure keeps the source order retryable; duplicate callbacks reuse the same idempotency key.
```
