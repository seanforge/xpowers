---
name: implement
description: Use when an approved Xpowers plan is ready for implementation in the current repository.
---

# Implement

Coordinate fresh subagents to implement every in-scope commitment in the approved plan, including its executable evidence.

## Prepare

- Find the latest approved plan for the change in `docs/plans/` and treat it as the authoritative implementation spec. Read repository instructions and inspect relevant code, tests, configuration, and history. If no plan exists, ask the user to invoke the `xpowers:plan` SKILL first.
- Act as coordinator. Use your native task tool to split the plan into ordered implementation tasks and keep it current. Do not create progress ledgers, task briefs, review packages, or any other duplicate task or workflow state. Apply engineering judgment: each task needs cohesive behavior, explicit scope, a clean boundary, known dependencies, and enough focus for an implementer and reviewer to assess independently. Avoid monoliths, mechanical micro-tasks, and fixed size thresholds.
- Before dispatching the first task, scan the complete task set for contradictions, missing dependencies, and conflicts with repository constraints. Resolve implementation-local details autonomously. If evidence invalidates a plan-owned outcome, boundary, or architectural decision, pause, present the evidence, obtain user approval, revise the plan in place, and reconcile the native task tool. Never silently deviate from the plan.

## Run each task

Do not implement production code or tests yourself. Run one task at a time. When a task begins, dispatch a fresh implementer subagent. Never allow multiple agents to modify files concurrently.

### Implement

Give the implementer the plan path, exact task scope and relevant commitments, dependencies and established interfaces, repository instructions, and only the additional context needed for that task.

Every implementer prompt must explicitly name and require:

- Invoke the `skills:clean-code` SKILL before changing production code or tests.
- Invoke the `skills:valuable-tests` SKILL before changing tests.
- Invoke the `tdd` SKILL where possible, only at seams agreed in the plan.
- Invoke the `codebase-design` SKILL when changing a module, interface, seam, adapter, or architecture.
- Invoke the `diagnosing-bugs` SKILL when a failure remains unexplained.
- Invoke every repository- or technology-specific SKILL whose trigger applies.

Name each SKILL exactly and call it a SKILL; never replace invocation with paraphrased guidance.

Also require the implementer to:

- Implement the complete task scope and its executable tests together. Choose the smallest reliable test boundary for the behavior and failure mode; use broader integration, contract, or E2E coverage only when narrower tests cannot establish the property. Add no quota-driven or duplicate coverage.
- Keep any warranted E2E scenario executable, deterministic, and organized around stable capabilities. Add the smallest reusable local test setup only when it fits the scope and PR budget; keep generated artifacts out of source control.
- Run focused tests and applicable typechecks throughout, then self-review against the task and invoked SKILLs, fix findings, and report changed files, checks actually run, results, and remaining concerns.

Resolve implementer questions before work continues. If it needs missing context, provide it; if the task is too large, split it; if the plan is wrong, follow the plan-revision rule. Never ask a subagent to guess through uncertainty.

### Review

After implementation, dispatch a different fresh subagent to review the task. The reviewer is read-only. Give it the same task requirements, repository instructions, implementer report and verification evidence, and the code and test changes attributable to the task.

Every reviewer prompt must explicitly require the subagent to invoke the `skills:clean-code` SKILL and the `skills:valuable-tests` SKILL, plus the `codebase-design` SKILL when the task changes a module, interface, seam, adapter, or architecture.

Require the reviewer to treat the implementer report as unverified claims and independently assess:

- **Spec compliance:** complete, correctly understood, within scope, and consistent with the plan.
- **Task quality:** correct, maintainable, appropriately tested, and supported by sufficient executable evidence.

Do not pre-judge or suppress findings in the reviewer prompt. Require two explicit, evidence-backed verdicts: `requirements satisfied` and `task quality approved`. This task review does not invoke the `xpowers:review` SKILL.

If either verdict fails, dispatch a fresh fixer subagent with the requirements and findings. Repeat every exact named SKILL requirement from the implementer prompt and require the affected checks, then dispatch a fresh reviewer. Repeat until both verdicts pass. Only then complete the task in the native task tool and begin the next task.

## Finish

Reconcile the implementation against the latest approved plan commitment by commitment. Implement every in-scope commitment or remove it through an approved plan revision. Run the complete change-relevant verification set once; if a defect appears, return to the affected task and repeat its checks and review.

Before handoff, dispatch a fresh read-only subagent for an ordinary whole-change review. Its prompt must explicitly require the `skills:clean-code` SKILL, `skills:valuable-tests` SKILL, and `codebase-design` SKILL. Require it to review the complete change for plan completeness, cross-task integration, design quality, delivered value, test quality, and maintainability. This is not the formal `xpowers:review` SKILL.

If the whole-change review finds blocking issues, dispatch a fresh fixer subagent with the exact named implementer SKILL requirements, rerun the affected checks and task reviews, then repeat the whole-change review.

Report the outcome, deviations, verification run, and remaining risks. Recommend invoking the `xpowers:review` SKILL next and ask whether to proceed; never invoke it automatically or create a handoff artifact.
