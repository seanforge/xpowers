---
name: implement
description: Use when an approved Xpowers plan is ready for implementation in the current repository.
---

# Implement

Implement every in-scope commitment in the latest approved plan, including its executable evidence, as a reviewable change.

## Orient

- Identify the latest approved plan and repository instructions; inspect relevant code, tests, configuration, and history. If the plan is unavailable, ask the user to run planning first.
- Translate every in-scope plan commitment into the harness's native task list. Keep it current; persist no duplicate checklist or workflow state.
- Verify facts instead of trusting plan-era assumptions or model memory. Decide implementation-local details autonomously. If evidence invalidates a plan-owned outcome, product boundary, or architectural decision, pause, present the evidence, obtain user approval, revise the existing plan in place, then reconcile the task list and continue. Never silently deviate from the plan.

## Build and prove behavior

Choose direct work or one implementation subagent at a time. All worktree writes are sequential; never allow concurrent writing agents.

**REQUIRED SUB-SKILLS:** Use `skills:clean-code`, `skills:valuable-tests`, and `tdd`.

**CONDITIONAL SUB-SKILLS:** Use `codebase-design` when changing a module, interface, seam, adapter, or architecture. Use `diagnosing-bugs` when a failure remains unexplained. Load repository- or technology-specific skills when triggered.

Production code and tests are one implementation responsibility. Follow `tdd` at plan-agreed seams, one vertical behavior slice at a time. Do not batch all tests before implementation or defer them to another phase.

Choose the smallest reliable test boundary for the behavior and its realistic failure mode. Use broader integration, contract, or end-to-end coverage only for properties narrower tests cannot establish.

There is no test-level quota. Do not duplicate assertions across layers or add E2E merely because a backend, Web, or Electron project changed. Run existing broader tests when they provide useful regression confidence.

When E2E is warranted, own its executable source—never a prose plan. Exercise the local system through public interfaces with the repository's HTTP/API harness or Playwright setup. Preserve conventions, organize scenarios by stable capability or subsystem, and keep setup, readiness, assertions, and cleanup deterministic.

If no local harness exists, add the smallest reusable one only within scope and PR budget; otherwise surface a prerequisite. Commit test source, fixtures, and configuration; keep generated reports, traces, videos, and screenshots in CI artifacts. Run change-relevant scenarios locally and the historical suite in merge CI.

Run focused tests and typechecks throughout. Stay within scope and PR-size guidance. Formal review remains separate.

## Finish

Reconcile the implementation against the latest approved plan, commitment by commitment. Every in-scope commitment must be implemented or removed through an approved plan revision; never hand incomplete planned work to review. Run change-relevant verification. If a defect appears, return to the affected behavior slice and re-run its checks.

This is a completeness gate, not a correctness judgment. Self-authored tests are implementation evidence; `xpowers:review` determines correctness and `xpowers:test` validates it.

Report the outcome, deviations, verification actually run, and remaining risks. Ask whether to proceed to `xpowers:review`; never invoke it automatically or create a handoff artifact.
