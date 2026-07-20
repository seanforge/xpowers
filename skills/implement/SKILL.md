---
name: implement
description: Use when an approved Xpowers plan is ready for implementation in the current repository.
---

# Implement

Turn the approved blueprint into the smallest complete, reviewable change. Follow vertical-slice TDD while leaving execution topology to the agent harness.

## Orient

- Identify the approved plan from the conversation. Read repository instructions and inspect the code, tests, configuration, and relevant history. If no approved plan is available, ask the user to run planning first.
- Translate the blueprint into the harness's native task list. Keep it current during implementation; do not persist a duplicate checklist or workflow state.
- Verify facts instead of trusting plan-era assumptions or model memory. Decide implementation-local details autonomously. If evidence invalidates an outcome, product boundary, or architectural decision owned by the plan, pause and ask the user to approve a plan revision.

## Execute

Choose direct implementation, subagent-driven work, or parallel execution according to task boundaries, coupling, risk, and harness capabilities. Do not impose one orchestration pattern or per-task review ritual.

Use test seams agreed in the plan when available. Otherwise choose the strongest existing public seam from repository prior art; ask only when that choice changes a product or architectural boundary.

For each vertical slice:

1. Write one smallest valuable test for an observable behavior. Use a characterization test before changing existing behavior. Start at the unit, integration, or E2E seam that gives the clearest useful signal.
2. Run the test and confirm it fails for the intended missing behavior or reproduces the defect.
3. Implement the simplest coherent production change that makes it pass.
4. Run the focused test, then use what the slice taught you to choose the next behavior.

Do not write every test before any implementation. Tests should exercise public interfaces, use independently known expectations, and survive internal refactors. Mock system boundaries, not internal collaborators. Keep each green slice clean; broader refactoring belongs to the review phase.

Implementation owns executable E2E test code, not a prose E2E plan. Add change-scoped scenarios directly to the repository's canonical long-lived suite:

- For backend behavior, run the supported local service and author executable HTTP or API scenarios using curl or the repository's equivalent harness.
- For Web or Electron behavior, run the local application and author Playwright scenarios.

Keep production code, unit and integration tests, fixtures, and E2E scripts maintainable. Cover the current change rather than rewriting or locally running the entire historical E2E suite.

Run focused tests and typechecks throughout the work and diagnose failures before changing code. Stay within the repository's PR-size guidance and the approved scope. Do not perform the separate review phase during implementation.

## Finish

Inspect the complete diff against the approved plan and repository instructions. Run all change-relevant verification. If verification exposes a defect, return to the TDD loop and re-run affected checks.

Report the implemented outcome, meaningful deviations, verification actually run, and remaining risks. Ask whether to proceed to `xpowers:review`; never invoke it automatically and create no handoff artifact.
