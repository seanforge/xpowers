---
name: implement
description: Use when an approved Xpowers plan is ready for implementation in the current repository.
---

# Implement

Implement every in-scope commitment in the latest approved plan, including its executable evidence, as a reviewable change.

## Orient

- Identify the latest approved plan for the current change in `docs/plans/` and read repository instructions; inspect relevant code, tests, configuration, and history. If the plan is unavailable, ask the user to invoke the `xpowers:plan` SKILL first.
- Act as implementation coordinator. Use your native task tool to split the plan into ordered tasks and keep it current; create no duplicate progress or workflow state. Apply engineering judgment: each task must have cohesive behavior, explicit scope, a clean boundary, known dependencies, and enough focus for a fresh implementer and reviewer. Avoid monoliths, mechanical micro-tasks, and fixed size thresholds.
- Verify facts instead of trusting plan-era assumptions or model memory. Decide implementation-local details autonomously. If evidence invalidates a plan-owned outcome, product boundary, or architectural decision, pause, present the evidence, obtain user approval, revise the existing plan in place, then reconcile the task list and continue. Never silently deviate from the plan.

## Build and prove behavior

Do not implement production code or tests yourself. Run one task at a time:

1. When a task begins, dispatch a fresh implementer subagent. Give it the plan path, exact task scope and commitments, dependencies and established interfaces, repository instructions, and only the context needed for that task. Require it to implement code and tests, run focused checks, self-review, fix its findings, and report results and concerns.
2. After implementation, dispatch a different fresh, read-only reviewer subagent. Give it the same task requirements, repository instructions, implementer report and verification evidence, and the code and test changes attributable to the task.
3. Require the reviewer to treat the implementer report as unverified claims and return two evidence-backed verdicts: `requirements satisfied` and `task quality approved`. Do not pre-judge or suppress findings in its prompt.
4. If either verdict fails, dispatch a fresh fixer subagent with the requirements and findings, rerun affected checks, then dispatch a fresh reviewer. Repeat until both verdicts pass.
5. Only then complete the task in the native task tool and begin the next task. Never allow multiple agents to modify files concurrently.

Every implementer and fixer prompt must explicitly require:

- Invoke the `skills:clean-code` SKILL before changing production code or tests.
- Invoke the `skills:valuable-tests` SKILL before changing tests.
- Invoke the `tdd` SKILL where possible, only at seams agreed in the plan.
- Invoke the `codebase-design` SKILL when changing a module, interface, seam, adapter, or architecture.
- Invoke the `diagnosing-bugs` SKILL when a failure remains unexplained.
- Invoke every repository- or technology-specific SKILL whose trigger applies.

Every reviewer prompt must explicitly require the `skills:clean-code` SKILL and the `skills:valuable-tests` SKILL, plus the `codebase-design` SKILL when the task changes a module, interface, seam, adapter, or architecture. This task review does not invoke the formal `xpowers:review` SKILL.

Build production code and its executable tests together; do not defer planned coverage to another phase.

Choose the smallest reliable test boundary for the behavior and its realistic failure mode. Use broader integration, contract, or end-to-end coverage only for properties narrower tests cannot establish.

There is no test-level quota. Do not duplicate assertions across layers or add E2E merely because a backend, Web, or Electron project changed. Run existing broader tests when they provide useful regression confidence.

When E2E is warranted, own its executable source—never a prose plan. Exercise the local system through public interfaces with the repository's HTTP/API harness or Playwright setup. Preserve conventions, organize scenarios by stable capability or subsystem, and keep setup, readiness, assertions, and cleanup deterministic.

If no local harness exists, add the smallest reusable one only within scope and PR budget; otherwise surface a prerequisite. Commit test source, fixtures, and configuration; keep generated reports, traces, videos, and screenshots in CI artifacts. Run change-relevant scenarios locally and the historical suite in merge CI.

Run the smallest relevant test targets and applicable typechecks throughout. Stay within scope and PR-size guidance. Formal review remains separate.

## Finish

Reconcile the implementation against the latest approved plan, commitment by commitment. Every in-scope commitment must be implemented or removed through an approved plan revision; never hand incomplete planned work to review. Run the complete change-relevant verification set once. If a defect appears, return to the affected behavior and re-run its checks.

This is a completeness gate, not a correctness judgment. Self-authored tests are implementation evidence; the `xpowers:review` SKILL determines correctness and the `xpowers:test` SKILL validates it.

Report the outcome, deviations, verification actually run, and remaining risks. Recommend invoking the `xpowers:review` SKILL and ask whether to proceed; never invoke it automatically or create a handoff artifact.
