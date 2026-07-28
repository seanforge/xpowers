---
name: implement
description: Use when an approved Xpowers plan is ready for implementation in the current repository.
---

# Implement

Implement every in-scope commitment in the latest approved plan, including its executable evidence, as a reviewable change.

## Ground the implementation

- Find the latest approved plan for the current change in `docs/plans/` and read repository instructions; inspect relevant code, tests, configuration, and history. If the plan is unavailable, ask the user to invoke the `xpowers:plan` SKILL first.
- Translate the plan's behavioral commitments, implementation decisions, and testing decisions into an ordered task list using the harness's native task tool. Use engineering judgment to shape cohesive, independently verifiable tasks around behavior and meaningful module, interface, or seam boundaries; account for dependencies and integration order. Avoid file-by-file tasks, horizontal layers, mechanical micro-tasks, and fixed size thresholds. Keep the task list current and persist no duplicate checklist or workflow state.
- Verify facts instead of trusting plan-era assumptions or model memory. Decide implementation-local details autonomously. If evidence invalidates a plan-owned outcome, product boundary, or architectural decision, pause, present the evidence, obtain user approval, revise the existing plan in place, then reconcile the task list and continue. Never silently deviate from the plan.

## Implement each task

Complete one task at a time.

Invoke the TDD skill (`tdd`) before implementation. Establish the test seams agreed in the plan before writing tests; if the plan does not establish the required seams, pause and confirm them with the user. Use the TDD skill's red-green vertical slices to drive the implementation.

Invoke the `skills:clean-code` SKILL before changing production code or tests.

Invoke the `skills:valuable-tests` SKILL before changing tests.

Invoke the `codebase-design` SKILL before changing a module, interface, seam, adapter, or architecture. Invoke the `diagnosing-bugs` SKILL when a failure remains unexplained, and invoke every repository- or technology-specific SKILL whose trigger applies.

Build production code and its executable tests together; do not defer planned coverage to another phase.

Run focused tests and applicable typechecks throughout the task. Stay within scope and repository PR-size guidance.

### Review the task

After the task's implementation and focused checks, start a fresh read-only reviewer subagent scoped to that task. Do not substitute the implementer's self-review for this independent review.

Give the reviewer the task's plan commitments, relevant implementation and testing decisions, repository instructions, attributable code and test changes, and verification evidence. Require it to treat implementation reports as unverified claims and independently assess:

- **Requirements satisfied:** the task is complete, correctly understood, within scope, and consistent with the plan.
- **Task quality approved:** the implementation is correct, maintainable, appropriately tested, and supported by sufficient executable evidence.

Require the reviewer to invoke the `skills:clean-code` and `skills:valuable-tests` SKILLs, plus the `codebase-design` SKILL when the task changes a module, interface, seam, adapter, or architecture. Do not pre-judge or suppress findings in the reviewer prompt.

Keep the reviewer confined to the task's commitments, attributable changes, and focused evidence. Do not ask it to inspect unrelated work, review the whole change, or declare the change clean. The `xpowers:review` SKILL owns cross-task interactions, integration risks, and whole-change correctness.

If either judgment fails, fix the findings, rerun affected checks, and resume the same task reviewer. Repeat until both judgments pass. Only then complete the task in the native task tool and begin the next task with a fresh reviewer. This task-level review does not invoke or replace the formal `xpowers:review` SKILL.

## Finish

Reconcile the implementation against the latest approved plan, commitment by commitment. Every in-scope commitment must be implemented or removed through an approved plan revision; never hand incomplete planned work to review. Run the complete change-relevant verification set once. If a defect appears, return to the affected behavior and re-run its checks.

This is a completeness gate, not a correctness judgment. Self-authored tests are implementation evidence; the `xpowers:review` SKILL determines correctness and the `xpowers:test` SKILL validates it.

Report the outcome, deviations, verification actually run, and remaining risks. Recommend invoking the `xpowers:review` SKILL and ask whether to proceed; never invoke it automatically or create a handoff artifact.
