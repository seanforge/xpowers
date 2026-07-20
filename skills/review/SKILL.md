---
name: review
description: Use when an Xpowers implementation is complete and ready for whole-change review before test execution.
---

# Review

Establish the correctness of the complete current change before formal test execution.

## Establish scope

- Identify the approved plan from the conversation and read repository instructions. If the plan is unavailable, ask the user to provide it rather than guessing.
- Determine the intended merge base from the PR target, tracking branch, or repository default. Review the whole current change, including committed, staged, and unstaged work.
- Treat production code and all applicable unit, integration, contract, and end-to-end tests as one review surface.

## Run the review loop

**REQUIRED SUB-SKILLS:** Use `interleaved-review` for the adversarial review loop and `skills:valuable-tests` for test quality.

Follow `interleaved-review` rather than recreating its cross-agent protocol. Add this focus to every review prompt:

> Additionally verify that the implementation matches the approved plan. Review production code and tests together. Assess whether the tests provide sufficient behavioral confidence at appropriate boundaries; flag missing or weak cases, tautological or implementation-coupled assertions, misleading doubles, and nondeterminism or flakiness risks.

Keep the same whole-change scope and merge base across rounds. Resolve every valid finding with the smallest in-scope change, using applicable implementation skills. Re-run the review loop after every fix. If a finding invalidates an approved outcome or architectural decision, pause for user direction instead of silently changing the plan.

The review passes only when `interleaved-review` reaches its required fresh clean round. Review findings and resolutions remain conversational; create no review artifact or receipt.

## Finish

Report the clean review result, important fixes made during review, and any residual risk that belongs in test execution. Recommend `xpowers:test` and ask whether to proceed; never invoke it automatically.
