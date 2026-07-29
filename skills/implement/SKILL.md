---
name: implement
description: Use when an Xpowers plan is provided for implementation in the current repository.
---

# Implement

Implement every in-scope commitment in the provided plan, including its executable evidence, as a reviewable change.

## Ground the implementation

- Resolve the provided plan from the prompt, conversation, and repository context. Read repository instructions and inspect relevant code, tests, configuration, and history. If the plan is missing or genuinely ambiguous, ask the user to invoke the `xpowers:plan` SKILL first or identify the intended plan.
- Treat the provided plan as the scope of one PR. Other plans in its series provide context and dependencies, not current implementation scope.
- Translate the plan's behavioral commitments, implementation outline, and testing decisions into an ordered task list. Track it using the active harness's native task-management or progress-tracking tools. Use engineering judgment to shape cohesive, independently verifiable tasks around behavior and meaningful module, interface, or seam boundaries; account for dependencies and integration order. Avoid file-by-file tasks, horizontal layers, mechanical micro-tasks, and fixed size thresholds. Keep the task list current and persist no duplicate checklist or workflow state.
- Treat model memory as a source of search terms, never as evidence. Before relying on external knowledge or any potentially time-sensitive claim to make or validate an implementation decision, verify it against current primary sources regardless of how familiar or certain it feels. Use official documentation, release notes, specifications, source code, and maintainers' technical guidance; seek strong current open-source implementations when they can provide useful design or integration evidence. Evaluate sources against repository constraints rather than copying them, and separate verified facts from inference.
- Revalidate plan-era assumptions and the planned PR boundary against current code and evidence. Decide implementation-local details autonomously. When concrete evidence invalidates a plan-owned decision or shows that the actual surface no longer fits one cohesive, reviewable PR under the applicable guidance, pause before accumulating more scope and present the evidence. When the approved boundary remains sound, remove only accidental out-of-scope changes attributable to the current implementation; never remove pre-existing, unrelated, or user-owned work, and ask when ownership is uncertain. When the plan or boundary must change, ask whether the user wants to invoke the `xpowers:plan` SKILL. Resume against the revised unit only after it regains user satisfaction and isolated reviewer approval, then reconcile the task list and continue with the resulting current slice. Never silently deviate from the plan, absorb a sibling plan, or split mechanically by files or line count.

## Implement each task

Complete one task at a time.

Invoke the TDD skill (`tdd`) before implementation. Establish the test seams agreed in the plan before writing tests; if the plan does not establish the required seams, pause and confirm them with the user. Use red-green vertical slices for behavior-changing work. For a planned behavior-preserving refactor, first pin the preserved behavior with existing tests or characterization tests, then refactor in small verified steps.

Invoke the `skills:clean-code` SKILL before changing production code or tests.

Invoke the `skills:valuable-tests` SKILL before changing tests.

Invoke the `codebase-design` SKILL before changing a module, interface, seam, adapter, or architecture. Invoke the `diagnosing-bugs` SKILL when a failure remains unexplained, and invoke every repository- or technology-specific SKILL whose trigger applies.

Build production code and its executable tests together; do not defer planned coverage to another phase.

Run focused tests and applicable typechecks throughout the task. Stay within scope and repository PR-size guidance.

### Review the task

After the task's implementation and focused checks, start a fresh, isolated, read-only reviewer subagent scoped to that task through the active harness's native context-isolation mechanism, with no inherited conversation context. Give it deliberately selected context sufficient for an independent judgment; do not substitute the implementer's self-review for this review.

Each task owns its reviewer session. Preserve and resume it until both judgments pass, and do not terminate or discard it while task review remains open.

Brief the reviewer on the task's intent, scope, result to establish, material focus, and useful evidence. Inputs may include the task's plan commitments, relevant parts of the implementation outline, testing decisions, repository instructions, attributable code and test changes, and verification evidence. Require the reviewer to treat implementation reports as unverified claims; it chooses its reasoning tools, additional evidence, and exploration depth while independently assessing:

- **Requirements satisfied:** the task is complete, correctly understood, within scope, and consistent with the plan.
- **Task quality approved:** the implementation is correct, maintainable, architecturally sound where applicable, and supported by the valuable executable tests and other sufficient evidence warranted by its behavior and risks.

Do not pre-judge or suppress findings in the reviewer prompt.

Keep the reviewer confined to the task's commitments, attributable changes, and focused evidence. Do not ask it to inspect unrelated work, review the whole change, or declare the change clean. The `xpowers:review` SKILL owns cross-task interactions, integration risks, and whole-change correctness.

If either judgment fails, fix the findings, rerun affected checks, and resume the same task reviewer. Repeat until both judgments pass. Only then mark the task complete using those native tracking tools and begin the next task with a fresh reviewer. This task-level review does not invoke or replace the formal `xpowers:review` SKILL.

## Finish

Reconcile the implementation against the provided plan, commitment by commitment. Every in-scope commitment must be implemented or removed only through the substantive plan-revision contract above; never hand incomplete planned work to review. Run the complete change-relevant verification set once. If a defect appears, return to the affected behavior and re-run its checks.

This is a completeness gate, not a correctness judgment. Self-authored tests are implementation evidence; the `xpowers:review` SKILL determines correctness.

Report the outcome, deviations, verification actually run, and remaining risks. Recommend invoking the `xpowers:review` SKILL and ask whether to proceed; never invoke it automatically or create a handoff artifact.
