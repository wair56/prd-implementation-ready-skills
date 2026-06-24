# Source and Language Guards

Use this when a generated requirement contains details that may not be sourced, when data origin is unclear, or when technical / abstract language appears.

## Human-Readable Wording Gate

PRD wording must be plain language and business-readable. The reader should feel the document is describing their work, not an internal taxonomy invented by the model.

Use the user's wording, existing system labels, and confirmed business terms as the primary vocabulary. Do not invent decorative names, clever labels, or a void-created term just to make the document look systematic.

Before introducing a new term, define its basis:

| Candidate Term | Source / Basis | Why Existing Term Is Not Enough | Recommended Naming? | Keep Original Term Nearby | Confirm |
|---|---|---|---|---|---|

Rules:

- If the user already has a term, keep it. Add clarification in parentheses instead of replacing it.
- If an existing system page, field, status, or menu label exists, use that label unless it conflicts with the business.
- If a shorter label is useful, mark it as recommended naming and keep the original term nearby until confirmed.
- Avoid abstract container words such as "strategy", "policy", "capability", "engine", "matrix", "center", or "domain object" unless the business actually uses them.
- Translate SQL, code enums, interface params, and database fields into business wording before PRD正文.

## Term Alignment Ledger

Use this when a PRD has many terms, multiple roles, or generated wording may drift from the user's language.

| User Term | Business Meaning | Source / Where User Said It | Existing System Label | Allowed Synonyms | Forbidden Replacement | Confirm |
|---|---|---|---|---|---|---|

This ledger prevents "professional" wording from changing the requirement. A term is aligned with the requirement only when its meaning, scope, and source match what the user described.

## Page-Document Term Alignment Gate

Use this when a term appears in a page, field list, button, summary card, formula, status, operation, report/export, ledger, or accounting/fund effect. The same business concept should not be called one thing on the page, another thing in PRD正文, a third thing in the formula, and a fourth thing in the money/resource destination.

Create a mapping before final wording:

| Business Concept | User Term | Existing UI Label / Page Label | Preferred Display Term | PRD Term | Business Object / Field | Calculation Field / Formula Component | Status / Operation Wording | Ledger/Accounting Effect | Fund Destination | Allowed Synonym | Forbidden Synonym | Confirm |
|---|---|---|---|---|---|---|---|---|---|---|---|---|

Rules:

- Choose one canonical term for the concept, and one preferred display term for page-visible wording. If they differ, explain why.
- Page label / visible label is a strong source. Use the same wording in page specs, operation tables, formulas, exports, and acceptance unless a deliberate difference is mapped.
- If user wording and existing UI label conflict, show both and recommend one; do not silently replace either.
- Technical field names, database names, and enum names may differ, but must map back to the preferred display term.
- A synonym is allowed only when it is listed in the ledger and does not change scope, payer/receiver, period, object, or state.
- Mark a forbidden synonym when it would mislead implementation or users, such as using a profit term for a fee, a page label for an accounting effect, or an analysis metric as an action amount.

Finance example:

| Business Concept | Preferred Display Term | PRD Term | Calculation Field / Formula Component | Ledger/Accounting Effect | Fund Destination | Forbidden Synonym |
|---|---|---|---|---|---|---|
| Platform fee charged on a profit-sharing bill | Platform service fee | Platform service fee | Platform service fee = profit-sharing reconciled amount total × contractor-level platform service fee rate snapshot | Platform income | Platform pocket; not contractor available balance | Profit, profit-sharing amount, contractor income |

In Chinese PRD wording, this means do not alternate between "平台服务费", "服务费", "平台收入", "平台口袋", "分润金额", and "利润" unless the table says which one is the page label, which one is the calculation component, which one is the ledger/accounting effect, and which one is forbidden in that context.

Bad:

> Create an "Operational Value Settlement Entity" to manage driver lifecycle incentives.

Better:

> Use the user's term "司机工资单". If a short internal label is needed, propose "Payroll Sheet (司机工资单)" as recommended naming and keep the original term in headings and page copy until confirmed.

## Requirement Source Trace

Every generated detail needs a source:

| Detail Type | Examples | Required Source |
|---|---|---|
| Interaction | 可勾选、默认全选、可批量、可导出 | User confirmed, existing UI convention, design.md, or recommendation marked待确认 |
| Data | 未发运单、可开票明细、待处理数 | System/object/table/interface, authoritative source, filters, refresh timing |
| State | 待处理、已完成、有效、未发 | State field, enum/business meaning, display text |
| Default | 默认全选、默认今天、默认当前组织 | Default rule, risk, whether user can change |
| Statistic | 成功率、失败数、覆盖率 | Object, period, include/exclude states, dedupe, refresh |
| Extension | New state/field/page/component/exception not directly stated by user | User confirmation, business anchor, source data, existing system behavior, research recommendation, or待确认 |

Use:

| Requirement Item | Added Detail | Source Type | Evidence / Source | Confirmed? | If Unconfirmed |
|---|---|---|---|---|---|

Do not treat "best practice" as a source of truth. It is a recommendation until it is checked against the business anchors and confirmed.

## Data Source Table

For lists, fields, stats, defaults, filters, and options:

| Data Item | Scene | Source System/Object/Table/API | Authoritative Source | Generation / Calculation | Include / Exclude | Refresh Timing | Fallback | Confirm? |
|---|---|---|---|---|---|---|---|---|

## State Axis Lifecycle Guard

Do not define a shared "status" by mixing lifecycle stages from different business axes. For every object, start from the object's creation point and check whether there are stages before audit, payment, invoicing, push, or settlement.

Before writing a status table, answer:

| Check | Required Question |
|---|---|
| Object birth | When is this object created, and by which source document / action? |
| Initial state | What is the first business state after creation? |
| Pre-audit stage | Is there a business confirmation, calculation, matching, claim, signing, or submission step before audit? |
| Axis owner | Which role/system changes this axis: user, operator, finance, platform, channel, third party? |
| Trigger | What action/event moves the object to the next state? |
| Terminal states | Which states end the lifecycle, and can they be reversed or rebuilt? |
| Display scope | Are the same display labels valid for every listed object, or only for part of them? |

If one object has a meaningful stage before audit, do not include it directly under "audit status" as if it starts at "待审核". Create a separate axis or object-specific lifecycle wording.

Example:

- Bad: Put "成本账单" under audit status with allowed labels "待审核 / 已审核 / 已拒绝".
- Better: Cost bill lifecycle first has "成本确认状态: 待确认", then after contractor confirmation it enters "审核状态: 待审核 / 已审核 / 已拒绝".

When uncertain, write "待确认: 该对象创建后的初始状态和进入审核前的业务阶段需确认" instead of guessing.

## Technical Expression Conversion

Do not put SQL, code enum, interface params, or database fields into PRD正文. Convert them to business language.

| Technical Input | Business PRD Wording |
|---|---|
| `incomeStatus in (CONTRACTOR_PENDING, CONTRACTOR_CLAIMED)` | 展示收入状态为“待承包人处理”或“承包人已认领”的记录 |
| `status = 1` | 展示业务状态为“有效”的记录，具体状态口径待确认 |
| `isShow = true` | 对用户展示该信息 |

If enum meaning is not known, write:

> ⚠️ 待确认：技术枚举 XXX 对应的业务展示文案和状态口径。

## Abstract Business Word Decomposition

Never use abstract words as idempotency keys, unique constraints, filters, or acceptance rules unless decomposed.

High-risk words:

- 计价口径
- 统计口径
- 结算口径
- 规则
- 策略
- 配置
- 版本
- 范围
- 条件

Decompose into fields:

| Abstract Word | Replace With |
|---|---|
| 计价口径 | Rule config ID, rule version, fee item range, source bill snapshot, cost version, tax version |
| 统计口径 | Object, period, include states, exclude states, dedupe, refresh timing |
| 结算口径 | Settlement period, settlement object, source document set, settlement rule version |
| 规则/策略 | Rule ID, name, version, scope, priority, enable state |

Example:

Bad:

> 分润单唯一性：承包人 + 月份 + 计价口径。

Better:

> 分润单唯一性按“承包人 + 分润周期 + 分润规则配置 ID + 分润规则版本 + 来源对账单快照 ID + 成本账单版本”判断。若来源单据集合、成本版本或规则版本变化，是否允许作废后重建待确认。
