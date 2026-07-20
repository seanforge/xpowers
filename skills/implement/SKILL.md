---
name: implement
description: Use when an approved Xpowers plan is ready for implementation in the current repository.
---

# Implement

Deliver the approved behavior and its executable evidence as one reviewable change.

## Orient

- Identify the approved plan. Read repository instructions and inspect the relevant code, tests, configuration, and history. Without an approved plan, ask the user to run planning first.
- Translate the blueprint into the harness's native task list. Keep it current; persist no duplicate checklist or workflow state.
- Verify facts instead of trusting plan-era assumptions or model memory. Decide implementation-local details autonomously. If evidence invalidates a plan-owned outcome, product boundary, or architectural decision, pause for an approved plan revision.

## Build and prove behavior

Choose direct work or one implementation subagent at a time. All worktree writes are sequential; never allow concurrent writing agents.

**REQUIRED SUB-SKILLS:** Use `skills:clean-code`, `skills:valuable-tests`, and `tdd`.

**CONDITIONAL SUB-SKILLS:** Use `codebase-design` when creating or changing a module, interface, seam, adapter, or architecture. Use `diagnosing-bugs` when an unexpected failure or behavior remains unexplained. Load repository- or technology-specific skills when the change triggers them.

Production code and tests are one implementation responsibility. Follow `tdd` at plan-agreed seams, one vertical behavior slice at a time. Do not batch all tests before implementation or defer them to another phase.

Choose the smallest reliable test boundary for the behavior and its realistic failure mode. Use broader integration, contract, or end-to-end coverage only for properties narrower tests cannot establish.

There is no test-level quota. Do not duplicate assertions across layers or add E2E merely because a backend, Web, or Electron project changed. Run existing broader tests when they provide useful regression confidence.

When E2E is warranted, implementation owns its executable source—never a prose E2E plan. Exercise the supported local system through public interfaces, using the repository's HTTP/API harness or Playwright setup. Preserve repository conventions and organize long-lived scenarios by stable product capability or subsystem, not by change or plan. Keep setup, readiness, assertions, and cleanup deterministic.

If no suitable local harness exists, add the smallest reusable one only when it fits the approved scope and PR budget; otherwise surface it as a prerequisite. Commit test source, fixtures, and configuration, but keep generated reports, traces, videos, and screenshots in CI artifacts. Run change-relevant scenarios locally; leave the complete historical suite to merge CI.

Run focused tests and typechecks throughout. Stay within scope and PR-size guidance. Formal review remains separate.

## Finish

Inspect production code and tests together against the approved plan and repository instructions. Run all change-relevant verification. If a defect appears, return to the affected behavior slice and re-run its checks. Self-authored tests are implementation evidence, not a substitute for independent review and testing.

Report the outcome, meaningful deviations, verification actually run, and remaining risks. Ask whether to proceed to `xpowers:review`; never invoke it automatically or create a handoff artifact.
