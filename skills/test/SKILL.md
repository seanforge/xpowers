---
name: test
description: Use when an Xpowers review has passed and the unchanged current implementation is ready for formal local validation.
---

# Test

Validate the reviewed change with executable evidence before the normal repository PR workflow.

## Establish scope

- Confirm `xpowers:review` passed for the current implementation. If tracked code, tests, or executable configuration changed afterward, return to review first.
- Read the latest approved plan, repository instructions, current diff, review outcome, and residual risks. Derive the validation scope from their observable commitments and the repository's supported commands; create no parallel checklist or test-plan artifact.
- Validate the current change locally. Merge CI owns the complete historical regression suite, browser or OS matrices, and full E2E unless repository rules require them earlier.

## Execute

Run every repository-required check applicable to the change, such as focused and broader unit, integration, or contract tests; typechecks; lint; builds; packaging; or migration checks.

For system behavior, start the supported local system under test, wait for deterministic readiness, isolate test data, execute the committed change-relevant scenarios through public interfaces, and clean up reliably:

- Backend behavior uses the repository's HTTP, API, or curl harness.
- Web and Electron behavior uses the repository's Playwright setup.

Do not mask nondeterminism with fixed sleeps, retries, test order, or widened timeouts. Keep generated reports, traces, videos, screenshots, coverage, and logs out of Git; expose them as runtime or CI artifacts.

## Handle failures

**CONDITIONAL SUB-SKILL:** Use `diagnosing-bugs` when a failure's cause is unclear.

Never mark a failed, blocked, or unrun required check as passing. Diagnose it first.

- If failure evidence invalidates an approved outcome, product boundary, or architectural decision, pause for user direction instead of treating it as an ordinary test fix.
- If the environment alone is wrong, correct it and rerun with evidence.
- If production code, tests, or executable configuration must change, stop validation and make the smallest applicable fix. Then recommend `xpowers:review`; after review passes again, recommend rerunning the complete applicable `xpowers:test` validation set.
- If an external blocker prevents a required check, report the blocker and stop short of success.

This phase does not first author missing tests. Missing or inadequate coverage is implementation work and follows the same fix, review, and test loop.

## Finish

Pass only when every required check succeeds on the unchanged reviewed implementation. Report the exact checks, results, and remaining limits conversationally; create no test receipt or workflow artifact.

State that the Xpowers flow is complete, then recommend the repository's normal PR workflow and ask whether to proceed. Never start it automatically.
