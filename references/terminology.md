# Terminology and Language

Solves: Chinese/English mixed wording, public publishing searchability, and unclear internal labels.
Read when: preparing the skill for public distribution, translating examples, or writing PRDs with Chinese business terms and English gate names.
Pair with: `source-language-guards.md`, `business-context.md`, and `output-structure.md`.

## Language Policy

Default audience is Chinese product/operations users, with English gate names kept as stable internal labels for search and routing.

Rules:

- Keep the user's business term as the primary wording in PRD output.
- Add a short English label only when it helps route to a gate or makes a concept reusable.
- Do not invent a new Chinese term if the user already has one.
- Treat any new term as recommended naming until confirmed; keep the original term nearby.
- Never replace concrete user language with decorative names or a void-created term.
- For public skill docs, write the first occurrence as `中文术语 (English label)` when the term is important and may be searched by English users.

## Glossary Starter

| Chinese Term | English Label | Use |
|---|---|---|
| 待确认 | Pending confirmation | only for unresolved high-risk business decisions |
| 业务锚点 | Business anchor | confirmed rule that later wording must not override |
| 主流程 | Main business flow | core happy path and branches |
| 小流程 | Micro-flow | small operation/side effect/recovery path hidden inside a module |
| 资金流 | Fund flow | finance-specific resource/value movement |
| 资源/价值流 | Resource/value flow | money, quota, coupon, inventory, permission, entitlement, visibility |
| 回写 | Writeback | event writes fields back to existing objects |
| 幂等 | Idempotency | duplicate request/callback protection |
| 计价口径 | Pricing/calculation basis | formula operands, source, scope, and period |
| 分润 | Profit sharing | settlement distribution after costs, fees, credits, and advances |

## File Naming Note

Current legacy filenames such as `business-context.md`, `business-anchors.md`, and `necessity-gate.md` are kept for compatibility. In new prose, explain their use by function:

- `business-context.md`: reader onboarding and plain-language business context.
- `business-anchors.md`: constraints and confirmed anchors.
- `necessity-gate.md`: scope and lean-detail gate.

If the skill later ships a breaking major version, these can be physically renamed with thin redirect files for one release cycle.
