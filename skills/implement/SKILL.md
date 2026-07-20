---
name: implement
description: Use when an approved Xpowers plan is ready for implementation in the current repository.
---

# Implement

Turn the approved blueprint into the smallest reviewable change through test-driven development.

## Orient

- Identify the approved plan from the conversation. Read repository instructions and inspect the code, tests, configuration, and relevant history. If no approved plan is available, ask the user to run planning first.
- Translate the blueprint into the harness's native task list. Keep it current during implementation; do not persist a duplicate checklist or workflow state.
- Verify facts instead of trusting plan-era assumptions or model memory. Decide implementation-local details autonomously. If evidence invalidates an outcome, product boundary, or architectural decision owned by the plan, pause and ask the user to approve a plan revision.

## Execute

Choose direct work or one implementation subagent at a time. All worktree writes are sequential: finish and verify each change before starting the next. Never allow concurrent writing agents.

**REQUIRED SUB-SKILL:** Use `skills:valuable-tests` to choose test boundaries, cases, doubles, and suite shape.

At test seams agreed in the plan, follow test-driven development: red before green; one test, one minimal implementation, and one vertical slice at a time. Do not batch all tests before implementation or anticipate later slices. When the plan leaves a seam open, use repository prior art and `skills:valuable-tests`; ask only if the choice changes a product or architectural decision.

Run focused tests and typechecks throughout the work. Keep each passing slice clean; broader refactoring and formal review belong to the review phase.

## E2E suite

Implementation owns executable E2E test code, not a prose E2E plan. Add change-scoped scenarios directly to the repository's canonical long-lived suite:

- For backend behavior, run the supported local service and author executable HTTP or API scenarios using curl or the repository's equivalent harness.
- For Web or Electron behavior, run the local application and author Playwright scenarios.

Preserve an existing E2E layout. If none exists, use this default at the nearest deployable product root:

```text
e2e/
  <domain-or-subsystem>/
    <behavior>.api.sh
    <behavior>.web.spec.ts
    <behavior>.electron.spec.ts
  support/
playwright.config.ts
```

Organize by stable product behavior, not change name, plan, date, or PR. Create only applicable files. Configure Playwright's `testDir` and projects around this tree. Keep domain fixtures beside their scenarios; reserve `e2e/support/` for genuinely shared harnesses. Tests must be isolated and own setup, readiness, and cleanup.

Commit E2E source, fixtures, and configuration. Keep reports, traces, videos, screenshots, and `test-results` out of Git and publish them as CI artifacts. Run current-change scenarios locally; leave the complete historical suite to merge CI.

Stay within repository PR-size guidance and approved scope. Do not perform the separate review phase during implementation.

## Finish

Inspect the complete diff against the approved plan and repository instructions. Run all change-relevant verification. If verification exposes a defect, return to test-driven development and re-run affected checks.

Report the implemented outcome, meaningful deviations, verification actually run, and remaining risks. Ask whether to proceed to `xpowers:review`; never invoke it automatically and create no handoff artifact.
