# Finance Settlement

Solves: payment order payloads, payment completion side effects, supplier/customer reconciliation, invoice lifecycle, profit sharing, service fees, payroll/freight formulas, and cross-module payment closure.
Read when: finance work creates bills, payment orders, invoices, profit-sharing records, payroll records, or downstream settlement.
Pair with: `finance-fund-flow.md` for ledger effects and `finance-cost.md` for cost eligibility.

## Payment Order Origin Payload

Every payment order must carry the original business payload. Payment management should not guess.

Core version:

| Item | Must Define |
|---|---|
| Source business object | cost item, reconciliation bill, payroll bill, vehicle fee, refund, manual adjustment |
| Business type | what the payment is for |
| Payer/payee | legal/business party and role |
| Receiver account | account name/type/channel/masked identifier/source |
| Amount split | total, tax/fee, receiver lines, mixed funding, rounding |
| Transaction metadata | purpose, bill number, period, contract/project/vehicle/supplier/customer, attachments |
| Trigger/status | completed adjustment, external payment, channel callback |
| Reverse/retry | cancel, fail, retry, duplicate callback, refund, void |

## Payment Completion Side-Effect Gate

Payment success is a business event. Define attached actions, idempotency, retry, partial failure, visibility, and reverse behavior. Examples: source status writeback, supplier payroll detail output, invoice/write-off update, balance release, allocation creation, message, external push, report/statistic update.

## Reconciliation and Invoice Lifecycle

For supplier or customer reconciliation, define selectable source details, grouping, source occupation, audit, invoice status, receipt/write-off, cancel/reject rollback, red flush/void behavior, payment trajectory, and unavailable reason.

Invoice management is a child object, not only an operation row. Define invoice source, upload/import/sync, invoice number uniqueness, amount match, edit/void/red-flush, attachment source, invoice status, page entry, and writeback.

Duplicate prevention key must match the true billing unit: party + period + project/contract/billing unit + source detail set/version when parallel projects or partial reconciliation exist.

## Supplier Reconciliation Completeness

Supplier reconciliation completeness requires separating supplier types and payment trajectory. A non-prepaid supplier must define payable generation, reconciliation bill, invoice management, payment application/order, payment completion, unpaid/part-paid/paid status, failure retry, and cancellation. A prepaid supplier must define prepayment balance, consumption deduction, allocation, recharge payment, refund, and reversal.

## Billing and Profit-Sharing Settlement Gate

Monthly charter should stay tied to the real commercial object, often by vehicle. Transport-volume billing should use shipper freight on the waybill or confirmed receivable source. Preserve vehicle, waybill, customer, supplier, contract, route, project, cost type, and period dimensions when they support analysis.

Platform service fee is platform revenue. Define payer, receiver/platform pocket, rate snapshot, fee base, timing, ledger entry, and whether it is deducted before profit sharing or collected separately.

Profit-sharing funds waterfall must cover customer funds, frozen funds, platform-granted credit, contractor own-funded advances, platform service fee, remaining contractor profit, insufficient/continuing deduction, zero/negative profit sharing, and profit-sharing void fund rollback.

## Payroll and Freight Formula Gate

A payroll/freight formula must define base amount, detail-calculated amount, comparison basis, period/entity scope, eligibility, proration, adjustment timing, evidence, and reversal.

Driver payroll is incomplete if it only says `max(guaranteed pay, waybill-based freight) + adjustments`. It must answer guarantee of what, what source creates waybill-based freight, which records participate, and whether adjustments happen before or after comparison.

## Cross-Module Navigation and Modal Reuse

For cross-module jumps, define source page/action, target menu/page/Tab, parameters, default filters, empty/missing data, and permission. If confirm and view-detail reuse one modal, define confirm mode, view mode, shared list/detail behavior, and amount summary consistency.

## Good-Enough Example

```text
Supplier reconciliation selects unpaid, exception-free consumption lines for the supplier and period.
Submit occupies selected source lines; reject releases them; confirmed bills keep occupation.
Invoice upload creates an invoice child object with invoice number, tax rate, excluding-tax amount, tax amount, total amount, attachment, and consistency check against the bill.
Payment order carries source bill, supplier receiver account, amount split, period, and payment purpose.
Payment success writes the bill paid status and triggers supplier-side output; failed output remains retryable while payment stays completed.
```
