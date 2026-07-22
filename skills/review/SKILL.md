---
name: review
description: Use when an Xpowers implementation is complete and ready for whole-change review before test execution.
---

# Review

Establish the correctness of the complete current change before formal test execution.

## Establish scope

- Locate the user-approved plan for the current change in `docs/plans/` and read repository instructions. If the plan is unavailable or its approval is unclear, ask the user rather than guessing.
- Determine the intended merge base from the PR target, tracking branch, or repository default. Verify that it resolves and that the combined review scope is non-empty. Review the whole current change, including committed, staged, and unstaged work.
- Treat production code and all applicable unit, integration, contract, and end-to-end tests as one review surface.

## Set the review contract

Give both native reviewers the same approved plan, merge base, complete current change, and repository instructions. Require each to review the whole surface through two non-compensating lenses:

- **Intent alignment:** find missing or partial commitments, behavior that does not satisfy the plan, unapproved deviations, and scope creep.
- **Engineering soundness:** find correctness, security, reliability, maintainability, or repository-standard problems, including inadequate or misleading executable tests.

The lenses are shared coverage requirements, not separate reviewer roles or required report sections. A clean result under one lens cannot offset a valid finding under the other.

## Run the review loop

**REQUIRED SKILL INVOCATIONS:** Invoke the `interleaved-review` SKILL for the adversarial review loop and the `skills:valuable-tests` SKILL for test quality.

**CONDITIONAL SKILL INVOCATIONS:** Invoke the `skills:clean-code` SKILL before applying a production-code fix. Invoke the `diagnosing-bugs` SKILL when a finding's cause remains unexplained. Invoke every repository- or technology-specific SKILL when its trigger applies.

Follow the invoked `interleaved-review` SKILL rather than recreating its cross-agent protocol. Include the review contract in every independent and clean-round prompt while leaving each harness's native review judgment intact.

Keep the same whole-change scope and merge base across rounds. Resolve every valid finding with the smallest in-scope change. Re-run the review loop after every fix. If a finding invalidates an approved outcome, product boundary, or architectural decision, pause for user direction instead of silently changing the plan.

The review passes only when no valid finding remains under either lens and the invoked `interleaved-review` SKILL reaches its required fresh clean round. Review findings and resolutions remain conversational; create no review artifact or receipt.

## Finish

Report the clean review result, important fixes made during review, and any residual risk that belongs in test execution. Recommend invoking the `xpowers:test` SKILL and ask whether to proceed; never invoke it automatically.
