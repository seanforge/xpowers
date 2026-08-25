---
name: implement
description: Use when implementing an assigned code-change scope after its consequential decisions are settled.
---

# Implement

Complete the assigned scope, including its executable evidence, as a coherent whole.

## Ground the scope

- Resolve the intended behavior, settled design, constraints, and assigned scope from the available context. Ask when the boundary remains ambiguous.
- Treat the assigned scope as authoritative; do not exceed it.
- Require the scope to own a complete contract at its boundary, preserve a valid repository state, keep tests with the behavior they verify, and make dependencies explicit.
- Treat broader context and adjacent changes as intent and dependency context, not current implementation scope. Use the active harness's native planning/task tools as needed; persist no duplicate workflow state.
- Inspect repository instructions and the relevant code, tests, configuration, and history. Retain ownership of the complete assigned scope when delegating.
- Treat model memory as a source of search terms, never as evidence. Verify implementation-shaping external claims against current primary sources and evaluate prior art against repository constraints.
- Revalidate settled assumptions and the assigned scope against current evidence. Decide implementation-local details autonomously. Return scope- or decision-invalidating evidence to the invoker before further work.
- When cleaning the scope, remove only out-of-scope changes attributable to its implementation; preserve unrelated or user-owned work and ask when ownership is uncertain.

## Implement the scope

Invoke and follow the `skills:anti-tdd` SKILL with the assigned scope as its requested-change context; it owns the production implementation lifecycle.

Invoke and apply the `skills:clean-code` SKILL to production and test code changes.

Invoke the `skills:valuable-tests` SKILL before changing maintained tests; it owns test methodology.

Invoke the `codebase-design` SKILL for implementation-local module, interface, seam, adapter, or architecture decisions. Invoke every repository- or technology-specific SKILL whose trigger applies.

Complete the assigned scope as a coherent whole; do not defer production implementation, exercise and stabilization, maintained tests, or final verification beyond it.

Run focused tests and applicable typechecks throughout the work.

## Finish

Reconcile the result against the assigned scope and its intended behavior, commitment by commitment. Never return incomplete scope. Run the complete scope-relevant verification set once; if a defect appears, return to the affected behavior and re-run its checks.

Self-authored tests and implementation reports are evidence, not a correctness verdict. Report the completed scope and dependencies, outcome, deviations, verification actually run, and residual risks, then return control to the invoker. Create no handoff artifact.
