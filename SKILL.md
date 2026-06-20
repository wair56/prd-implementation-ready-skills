---
name: prd-rigorous-completion
description: Use when writing, reviewing, or filling gaps in a product PRD or 需求文档 of any kind — turning a rough requirement into an implementation-ready spec. Covers business/user flow, states, data source, page/UX structure, edge cases, and abstract or undecided terms. Strongest for complex B端/后台 and finance/payment/settlement/对账/分润/approval flows, but applies to any product domain (B端, C端, content, commerce, tooling).
---

# PRD Rigorous Completion

## Core Principle

The job is not "fill in a document." The job is **to figure out the product together with the user** — align on the real need, research the domain, design the whole flow and UX — and then capture that as a doc a developer (or an AI) can implement directly. Work progressively: confirm what the business is trying to do, reconstruct the main flow, then expand into object flow, data flow, linkage, rules, exceptions, states, data, page structure, and final detail. **Be complete but lean** — do not add words, entities, statuses, pages, fields, or components unless they make the product clearer, safer, more usable, or more testable. **Never dump every checklist at the user at once, and never let a generated detail enter the PRD without a source or a 待确认 mark.**

## Definition of Done — The One Bar

A good PRD passes one test: **a developer or AI can implement it without coming back to ask a single business question. The only thing left open is technical 选型 (framework, library, storage), never business behavior.**

Concretely, every one of these must be answered in the doc, not in the reader's head:

- **Flow** — who triggers each step, preconditions, what object/state changes, what gets written back, who handles failure.
- **Boundaries** — what is explicitly in scope and out of scope; what the system does NOT do.
- **Exceptions** — every reverse/abnormal path. Transactional: 驳回, 失败, 作废, 重复提交, 回调重复/乱序/延迟, 并发, 金额为零或负. Content/social/C端: 内容删除后悬空引用, 拉黑/举报, 重复操作(重复点赞/打卡), 权限变更(关注后对方转私密), 上传失败重试, 跨时区日界. Pick the abnormal paths that actually exist in *this* domain.
- **Pages** — every page's content: list fields + data source, filters/sort/Tabs in business language, detail/drawer/modal behavior, operations + which states allow them, empty/loading/disabled/submitting/success/error states, high-risk 二次确认.
- **Data** — each field's source object/snapshot, authoritative source, include/exclude conditions, refresh timing.
- **Rules** — every formula decomposed into operands (no black-box 计价口径); uniqueness keys from stable fields; idempotency; permissions and 端-level data isolation.

If a reader would have to ask "but what happens when…" or "where does this number come from?", it is not done. Anything genuinely undecided is written as **待确认 + a recommended default**, so the reader sees a marked decision, not a silent hole.

**When the user's draft is a downgrade of a doc that already exists**, do not silently backfill it into a fresh "complete" version — that creates two contradictory docs. Diff the draft against the existing locked doc, reconcile to its 口径, and ask the user which differences are intentional changes vs accidental omissions. Treat a regression as a question, not a gap to quietly fix.

## The Other Bar — Is the Product Any Good?

Definition of Done checks whether the doc is *complete*. But a complete doc can still describe a clumsy product. A good PRD also has to pass a **quality bar**: the flow is smooth and the UX is good — not just fully specified. Both bars must hold; completeness without quality ships a well-documented bad product.

So part of the job is to actively make the product better, not just record it:

- **Find best practice / research the domain** — before locking a flow, look at how mature products in this domain (and adjacent ones) solve the same problem. Aligning to a proven pattern beats inventing one. (Research Is a Hard Gate, below.)
- **Analyze flow smoothness and optimize it** — for the agreed flow, ask: redundant steps, things the user re-enters, a step that could be validated earlier, one object carrying too many jobs, offline coordination the system could remove. Where the flow is awkward, say so and propose a cleaner one with the tradeoff. (`references/research-and-flow.md`)
- **Analyze UX quality and optimize it** — judge whether each page/interaction is the right carrier (list vs detail vs drawer vs wizard), the right information density, the right defaults, the right empty/loading/error/confirm behavior. Good UX is a product decision tied to the workflow, not decoration. (`references/page-ui.md`)

Guide the user toward the better product; do not merely transcribe what they first described. Every improvement is a recommendation until confirmed.

## When NOT to Use

- Editing one field label, copy, or visual style — just do it.
- A simple, single-step requirement with no money/state/data-source/cross-system risk — answer directly.
- Non-business tasks (pure code, infra, docs unrelated to product requirements).

## Progressive Workflow

1. **Business First** — plain-language business context, agreed mainline, non-negotiable premises, scope (in/out), users, role glossary, core concepts, business objects.
2. **Main Flow Next** — reconstruct the user's flow, propose a better one, offer MVP. Guide toward the cleaner path; do not merely record.
3. **Research Before Locking** — see "Research Is a Hard Gate" below.
4. **Rules After Flow** — states, source documents, idempotency, data sources, permissions, exceptions, boundaries, rollback.
5. **Pages After Business Shape** — menu/page organization, page-level Tabs vs list Tabs, design.md/UI conventions, interaction habits.
6. **Write After Confirmation** — show confirmed rules, recommended defaults, open high-risk decisions, out-of-scope. Wait for confirmation.

## What a Locked Mainline Must Contain

"Align the mainline first" is meaningless without saying what a complete mainline is. **The mainline is the closed loop of how value/money is created, flows through objects, and is finally settled — not a one-line process summary.** A summary like "成本归集 → 应收 → 分润 → 支付" is usually an *open* line: it emits 应收 but never says how the money comes in or gets collected. Modules hung on an open mainline will not reconcile.

A mainline is not locked until all of these are present and the loop closes end-to-end:

1. **Value / business goal** — who it serves, what value it creates. Without it, scope and tradeoffs can't be judged.
2. **Parties (roles)** — who triggers each step, who receives each result. Every step needs a "who".
3. **Core object chain (the nouns)** — value rides on objects: 运单 → 成本单 → 应收/应付 → 分润单 → 付款单. This chain is the skeleton modules are cut along.
4. **Steps & triggers (the verbs)** — for each step: who triggers, precondition, which object it creates/changes, what it writes back.
5. **End-to-end value loop — the most-often-missed, most-fatal element.** Where value enters, how it flows, where it exits, who closes the loop. **This is not only about money.** In a transactional product the loop is monetary (every 应收 has a matching 收; every 应付 a matching 付; every 授信/预存 a matching 核销/解冻). In a habit/content/social product it is a motivation loop (user invests → gets immediate payoff → social/feedback reward → that reward drives the next visit). In a tooling product it is a job-to-done loop (input → transform → trusted output the user acts on). If you think "my product has no money so this doesn't apply," you are about to skip the single most important thing to lock. Whatever the currency, if the loop has an open end, the mainline is incomplete.
6. **Irreversible / key nodes** — where things lock, where 打款 happens, what cannot be undone. These anchor the state-machine trunk and the risk points.
7. **口径 anchors on the mainline** — the high-risk口径 the flow depends on (计价/结算/唯一性/幂等). Even if details are still 待确认, mark which step each hangs on, or modules will each invent their own.
8. **Reverse paths on the trunk** — not every exception, only the ones that *change the shape of the mainline*: 作废, 退款, 红冲. They decide whether the loop runs in reverse.

**Acceptance bar:** can you cut every module out by following the mainline, and does each module map back to a step on it? Does the money loop connect head-to-tail with no open end? If not, the mainline is not locked yet — keep working it before writing any module.

Before locking, the mainline can be challenged and improved (Progressive Workflow #2). Once locked, it becomes an anchor that generic best practices must not override (Operating Rules). Challenge first, anchor after.

## Research Is a Hard Gate, Not an Optional Step

Before locking the flow or defaults, you MUST first read what already exists: project docs (design.md, ui-design-spec.md, old PRDs, screenshots), the code/data layer if relevant, and mature market/open-source patterns. Aligning to an existing 口径 beats inventing a new one.

**If a more complete version of this exact doc/module already exists in the repo, diff against it first.** Do not rewrite a mature PRD back into a draft. Reconcile to its 口径, merge any genuinely new intent, and surface conflicts — handing the team two contradictory docs is worse than handing them none.

Under time pressure this is the first thing skipped. Do not. If you genuinely cannot read sources now, write the affected口径 as 待确认 — never as confirmed.

## When Someone Demands the Full PRD Now ("别问了，直接写完整版")

This is the highest-pressure failure point. The wrong move is to abandon confirmation and write a PRD full of invented-but-unmarked rules. The right move:

- **You may write the full PRD immediately** — pages, fields, states, exceptions all present.
- **But every detail you were not given and cannot source MUST be marked 待确认** with the default口径 you assumed. Confirmed items come only from the user's words or existing docs.
- **Lead with the main flow + the 3-6 highest-risk口径** (especially 计价/结算/唯一性/打款幂等) as a confirm-first block, so the reviewer sees known gaps, not buried landmines.
- Money/contract/invoice/settlement formulas: never invent a concrete formula and present it as decided. Decompose into fields and leave the formula as a 评审必锁项.

Speed and rigor are not in conflict: deliver fast AND mark every unsourced detail. Skipping the 待确认 marks is not "saving time," it is shipping landmines.

## Write Incrementally, One Module at a Time

A multi-module PRD does not have to be written in one sitting. Writing all modules at once is high-pressure and produces modules that silently contradict each other. Instead:

1. **Align the mainline first** — agree the main flow, core objects, and highest-risk口径 with the user before writing any module.
2. **Write the core/global doc** (`00_总览`) — main flow, core objects, global rules, cross-module dependencies, in/out scope. This is the authority; modules must not contradict it.
3. **Write modules one at a time**, in dependency order. Each module carries only its own rules; shared state axes / 幂等 / 回写 stay in the core doc. Finish and confirm one before starting the next — small, low-pressure steps.
4. **Review them together at the end** — a cross-module linkage pass so the whole set tells one consistent story.

### Change Propagation — the discipline that prevents contradictions

While writing one module you will often discover a change. Handle it by **where the change lands, immediately — do not just note it and keep writing the current module:**

- **Touches the mainline / a global口径 / core object** → stop, fix the core doc (`00`) completely first, then cascade to every module that references it. Never patch it only in the current module.
- **Touches another module** → go change that other module too, now, so the two never disagree. Do not leave it for "later."
- **Local to this module** → just handle it here.

The failure mode this prevents: six modules that each look internally complete but disagree on the same formula/status/wording. Detecting that at final review is far more expensive than propagating each change when you find it. If a change to a confirmed mainline anchor is involved, confirm it with the user before cascading.

## Load References as Needed

Read only the references needed for the current stage:

| Situation | Read |
|---|---|
| Starting a PRD conversation or deciding the next stage | `references/stage-gates.md` |
| Deciding whether to add an object, state, field, page, component, flow step, table, section, exception, or detailed rule | `references/necessity-gate.md` |
| Capturing user-agreed business mainline, obvious domain premises, non-negotiable constraints, or preventing later rewrites from losing them | `references/business-anchors.md` |
| Reader may not understand the business, roles, domain terms, business model, special concepts, or value/money flow | `references/business-context.md` |
| Need broad coverage across business objects, object flow, or information introduced during each step without dumping all questions at once | `references/coverage-matrix.md` |
| Need to clarify data flow, cross-object linkage, side effects, writebacks, notifications, statistics, locks, rollback, or downstream impacts | `references/data-flow.md` |
| User describes workflow, says "你来决定", or flow seems awkward | `references/research-and-flow.md` |
| Reviewing multiple docs, finding contradictory口径, or checking money / formula / fund movement computability | `references/business-consistency.md` |
| Finance operations include cost periods, fund priority, sync data, supplier states, reconciliation, invoice, red flush / void, or cross-module jumps | `references/finance-operations.md` |
| Details may be unsupported, data source unclear, state axes / initial states uncertain, technical terms or 计价口径-type words appear | `references/source-language-guards.md` |
| Discussing target端, touchpoints, menus, page Tabs, UX, component choice, UI style, design.md, interaction habits | `references/page-ui.md` |
| User asks for full PRD, multiple docs, index/global rules, final structure | `references/output-structure.md` |
| Before claiming a PRD is complete, or reviewing an existing PRD | `references/review-checklists.md` |

## Operating Rules

- Recommend best-practice defaults when the user hasn't decided, but label them as recommendations until confirmed.
- Keep questions small: 3-6 decisions per batch, each with a recommended option and tradeoff.
- Treat short answers ("加 / 不需要 / 你来 / 按最佳实践") as decisions to digest, not missing context.
- Use the minimum structure that makes the product buildable, testable, and understandable. Do not add entities, statuses, pages, components, tables, or prose just to look complete.
- When proposing a flow, page, component, or rule, explain the product constraint it solves and what would break without it.
- Write from the user-agreed business mainline. Generic best practices and polished wording must not override confirmed business facts.
- Every extension beyond the confirmed mainline needs a basis: user confirmation, business anchor, source data, existing system behavior, market research marked as recommendation, or "待确认".
- Keep PRD wording business-readable. SQL, code enums, interface params, database names inform the work but stay out of PRD正文 (technical appendix only).
- If a flow, field, or rule cannot be made testable, keep decomposing before writing it as final口径.

## Red Flags — STOP, you are about to violate the skill

- About to output exception matrices / page-Tab tables / state machines **before** business + main flow are confirmed.
- About to write the full PRD **without** marking invented details as 待确认, because the user said "别问、直接写".
- About to lock口径 / write defaults **without** first reading existing project docs (skipping research under time pressure).
- About to write an abstract term (计价口径 / 统计口径 / 结算口径 / 规则 / 策略) into a uniqueness key, filter, or acceptance rule **without** decomposing it into fields.
- About to put `status=1` / `incomeStatus in (...)` / code enums into PRD正文.
- Adding an entity/status/page/field just to look complete, with no product constraint behind it.
- Gave only one flow version when 原始/推荐/MVP comparison would help.

**Any of these means: stop and return to the progressive workflow.**

## Default First Response Shape

For a rough requirement, start with: (1) one-sentence plain-language business goal, (2) known/unknown about scope/users/objects/risk, (3) initial main-flow reconstruction marked as inferred, (4) the 3-6 highest-value confirmation questions with recommended defaults. Do not open with exception matrices, Tab tables, or detailed state machines.
