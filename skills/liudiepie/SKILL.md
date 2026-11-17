---
name: liudiepie
description: High-efficiency 4-phase engineering workflow (Grill and Align, Investigate and Scope, Build with Constraints, Verify with Evidence) that prevents wasted tokens, eliminates AI code slop, and proves correctness with runtime evidence. Use when building features, fixing bugs, refactoring modules, or executing any multi-step coding task.
---

# liudiepie

A 4-phase engineering workflow that eliminates wrong-assumption rewrites, duplicate abstractions, and unverified code slop by pairing upfront requirement alignment with root-cause investigation, executable constraints, and concrete runtime proof.

## When to Use

- Building a new feature or changing behavior across multiple files
- Fixing a bug whose root cause has not yet been reproduced and verified
- Refactoring modules, APIs, or shared abstractions
- Any coding task where jumping straight to implementation risks wasted tokens or regressions

## Phase 1: Grill & Align

Eliminate wrong-assumption rewrites before spending tokens on code.

1. Read the relevant entry points and existing project conventions first so you never ask questions the codebase already answers.
2. Identify genuine product, UX, edge-case, or architectural ambiguities.
3. Ask up to 5 numbered questions in a single compact round, providing a recommended default for each question so the user can reply quickly (for example, "Q1 agree, Q2 use Redis instead").
4. Repeat only if an answer opens a critical downstream branch. Once settled, summarize the agreed scope in 3 to 5 bullet points and move to Phase 2.

Fast-track escape hatch: If the task is trivial or has a single unambiguous specification, skip questioning and state a 2-line execution plan before editing.

## Phase 2: Investigate & Scope

Prevent duplicate utilities and speculative over-engineering by grounding every change in real code paths.

1. Trace the actual execution chain from entry point (UI event, API route, CLI command, or worker) through business logic to storage or external boundaries.
2. Search the repository for existing helpers, types, and patterns. Reuse or simplify what already exists before introducing new code.
3. For bug fixes, write or run a failing reproduction first to confirm how the failure occurs and why it happens.
4. Keep diffs strictly surgical: modify only files required by the agreed scope, and remove dead code or obsolete workarounds encountered in those paths.

## Phase 3: Build with Constraints

Make regressions mechanically impossible instead of relying on fragile prose reminders.

1. Follow red-green-refactor: write or update automated tests that fail before your change and pass after it.
2. Encode invariants in types, schemas, assertions, or linter rules rather than defensive workarounds or narrative comments.
3. Do not add comments that narrate what code does or excuse hacks; express intent through clear names and straightforward control flow.
4. Keep modules and functions single-purpose with no speculative abstractions.

## Phase 4: Verify with Evidence

Deliver concrete before/after proof rather than unverified "looks good to me" diffs.

1. Run the project's typechecker, linter, and relevant test suite, resolving any errors or warnings introduced by your edits. If the project's CLAUDE.md has a `## liudiepie verification` section (written by `/setup-liudiepie`), use exactly those commands.
2. Exercise the real code path end-to-end (CLI command, API request, or browser/UI check) and capture concrete runtime output proving the feature works or the bug is fixed.
3. Run a 2-pass self-review on the final diff:
   - Intent pass: Confirm every requirement and edge case agreed in Phase 1 is satisfied.
   - Slop pass: Strip unused imports, dead branches, narrating comments, and unrelated formatting churn.
4. Present a concise final report with: the changes made, before/after verification evidence, and any reusable guardrail (test, lint rule, or type constraint) added to prevent regressions.

## Gotchas

- Never ask questions that grep or reading entry points can answer immediately.
- Never bundle unrelated refactors into a feature or bug-fix diff.
- Never treat static inspection or compilation alone as verification; always execute the real path and show output.
- If context grows noisy across failed attempts, pause and summarize verified facts, current state, and remaining steps cleanly before continuing.
