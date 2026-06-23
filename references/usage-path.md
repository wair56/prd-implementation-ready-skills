# Best Usage Path

Solves: how to start, route, and sequence this PRD skill without turning every request into the full heavy workflow.
Read when: the user asks how to use the skill, asks to optimize the skill flow, starts a new PRD effort, reviews generated docs, or changes an existing requirement.
Pair with: `stage-gates.md`, `business-context.md`, `output-structure.md`, `review-checklists.md`, and `guardrail-checklist.md`.

## Usage Path Router

Choose a mode before writing. Do not start with the full template. The skill is strongest when the current mode is explicit and the next output matches that mode.

| Mode | Use When | Main Output | Read Next |
|---|---|---|---|
| 0. Route | The user asks where to start, what path to use, or gives a mixed request | Recommended mode, scope, depth, and next action | This file, then only the selected mode files |
| 1. Business Grilling | The business is unclear, risky, new, or full of hidden rules | Context digest, decision tree, recommended defaults, one question at a time | `business-context.md`, `research-and-flow.md`, `stage-gates.md` |
| 2. PRD Drafting | Main decisions are stable enough to write | Reader-first module PRD with flow, pages, data, states, exceptions, and acceptance | `output-structure.md`, selected rule/page references |
| 3. PRD Review | A generated or existing PRD needs gap finding | Findings first, missing gates, concrete rewrite targets | `review-checklists.md`, `guardrail-checklist.md` |
| 4. Version Evolution | Existing product or v1 docs are changing | Old rule, change intent, impact range, old vs new, migration, rollout, rollback | `version-evolution.md`, `business-anchors.md` |
| 5. To Issues | PRD is stable and work needs implementation planning | Vertical slice issue list with dependencies and acceptance criteria | `output-structure.md`, implementation planning notes |

## Mode 1: Business Grilling

Use Business Grilling when the user is still discovering the business. The output should not be a final PRD yet.

Process:

1. Build a context digest from the user's source material.
2. Name the decision tree: roles, objects, flows, states, data sources, page surfaces, exceptions, and open risks.
3. Walk the decision tree one branch at a time. Ask one question at a time when the answer changes downstream logic.
4. For each question, provide a recommended answer and the impact if the recommendation is accepted.
5. Capture resolved terms, business anchors, and rejected options immediately.

Completion criterion: the main flow, core objects, source data, page responsibilities, and high-risk gates are stable enough to draft without inventing rules.

## Mode 2: PRD Drafting

Use PRD Drafting after the main decisions are stable. Drafting should be reader-first, not table-first.

Output order:

1. Module purpose.
2. Main flow.
3. Page presentation.
4. Key operations.
5. Risks and exceptions.
6. Page implementation details.
7. Server-only automatic flows.
8. Acceptance checklist.

For page implementation details, include the page element inventory, source data filter, data lineage, status/writeback, permissions, idempotency, validation, empty/loading/error/disabled/submitting states, feedback, and recovery.

Completion criterion: a developer or AI can implement the module without asking another business question.

## Mode 3: PRD Review

Use PRD Review when the user gives an existing file or generated output and asks what is missing.

Process:

1. Lead with findings, ordered by severity.
2. Tie each finding to a gate, page, object, status, source data set, formula, writeback, or exception path.
3. State whether the fix is a rewrite, a local patch, or a user decision.
4. Avoid rewriting the whole PRD unless the user asks.

Use `guardrail-checklist.md` for P0/P1 blockers and `review-checklists.md` for the broader scan.

Completion criterion: every blocker has a concrete fix path, recommended default, explicit pending decision, or out-of-scope note.

## Mode 4: Version Evolution

Use Version Evolution when the user is changing an existing product, old PRD, generated PRD, or confirmed rule.

Always capture:

| Item | Required |
|---|---|
| Old rule | What the product or PRD said before |
| Change intent | Why the user wants the change |
| Impact range | Modules, pages, objects, source data, statuses, permissions, analytics, and downstream docs affected |
| Old vs new | What stays, what changes, what is deleted |
| Migration | Historical data, drafts, in-progress flows, locked records, and compatibility |
| Rollout/rollback | Release order, fallback, and reversal |
| Decision record | Who decided, when, and why |

Completion criterion: no stale wording or old rule remains downstream unless explicitly marked historical or out of scope.

## Mode 5: To Issues

Use To Issues after the PRD is stable and the user wants implementation work broken down.

Break work into vertical slice issues, not horizontal layers. A vertical slice is a narrow but complete path across data, server behavior, page behavior, permissions, exceptions, and tests.

Each issue should include:

- What to build.
- User story or business outcome covered.
- Dependencies and blocked-by list.
- Acceptance criteria.
- Source PRD section.
- Key risks or gates that must be preserved.

Completion criterion: each issue is independently grabbable, demoable or verifiable, and small enough for a fresh implementation session.

## Context Hygiene

Keep Business Grilling, PRD Drafting, and To Issues in the same context window when possible, because the decisions build on each other.

Use handoff when the thread is too large, when branching into a prototype, or when another agent/session must continue. The handoff should preserve resolved decisions, open decisions, source files, and the current mode.

Use compact only at intentional phase breaks, not mid-discovery. Compacting during unresolved discovery can lose the exact wording that made a decision meaningful.

## Wrong Path Signals

- Writing final PRD while discovery is still open.
- Reviewing a generated PRD as if starting from zero, instead of preserving confirmed rules.
- Asking every possible checklist question at once.
- Applying finance-heavy gates to a consumer/content/tool product without domain adaptation.
- Starting To Issues before the PRD has stable source data, states, page actions, exceptions, and acceptance.
- Treating a review finding as fixed because text was added, without checking downstream impact.
