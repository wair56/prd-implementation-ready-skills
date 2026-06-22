# Finance Operations Index

Solves: routes finance-heavy PRD work to the right focused finance reference instead of one large mixed checklist.
Read when: the requirement involves funds, costs, period attribution, payment, reconciliation, invoice, payroll, profit sharing, amortization, or supplier/customer settlement.
Pair with: `data-flow.md`, `business-consistency.md`, `page-ui.md`, and the topic file below.

## Topic Router

| Situation | Read |
|---|---|
| Balance, credit, support fund, prepayment, recharge, refund, freeze, occupation, release, priority, mixed funding, pool/trace, role ledger | `references/finance-fund-flow.md` |
| Cost inclusion, period attribution, paid/unpaid costs, annual/prepaid costs, amortization, accrual, synced cost data, zero payroll line | `references/finance-cost.md` |
| Reconciliation, invoice lifecycle, supplier/customer bills, payment order payload, payment completion side effects, profit sharing, payroll/freight formula | `references/finance-settlement.md` |

## How To Use

Start with the general Resource / Value Flow Gate in `data-flow.md`; finance is a specific case of controlled resource movement. Then open only the relevant finance topic file.

Authoritative finance gates:

- Finance Fund-Flow Gate: `finance-fund-flow.md`.
- Cost Inclusion State Gate and Amortization / Accrual Gate: `finance-cost.md`.
- Settlement and Payment Closure Gate, Payment Order Origin Payload, Reconciliation and Invoice Lifecycle, Billing and Profit-Sharing Settlement Gate, Payroll and Freight Formula Gate: `finance-settlement.md`.

## Good-Enough Example

For an annual insurance cost:

```text
Cash payment: platform pays insurer once per policy year through a payment order.
Cost recognition: monthly cost is generated from the policy start/end dates using straight-line daily proration.
Inclusion: a monthly cost line can enter the cost bill only after policy audit passes and the month is unlocked.
Visibility: vehicle cost detail shows original policy, paid amount, recognized amount this month, remaining unamortized amount, and schedule drawer.
Reverse: cancellation before a locked month rebuilds future schedule lines; locked months keep history and use adjustment lines.
```

That is enough to implement and test; the topic files provide the full tables when risk is higher.
