---
name: stack
description: Automate whole-series delivery by invoking Xpowers implementation and formal review across ordered pull requests through an authorized merge decision. Use when the user wants one or more completed Xpowers plans delivered as a complete flow.
---

# Stack

Own automated delivery of the provided `xpowers:plan` Plan Series. Preserve one cohesive PR per plan and use GitHub's native stacked-PR capability when the series contains multiple PRs.

Read [references/github-stack.md](references/github-stack.md) before operating the stack. Invoke `xpowers:setup` when the required GitHub capability is unavailable or unauthorized.

## Deliver the series

Resolve the complete series and its dependency order from the provided plans. Use the active harness's native task tracking, Git, and GitHub as the only workflow state.

For each plan in dependency order:

1. Establish its intended branch and PR layer.
2. Invoke `xpowers:implement` for exactly that plan.
3. Create or update its PR, then invoke `xpowers:review` against that plan and layer.
4. Continue the implementation–review loop until the PR's current content holds a clean formal review conclusion.

After the complete series is implemented, invoke a fresh `xpowers:review` for every PR in dependency order against its final intended layer. Later fixes, rebases, or dependency changes invalidate any affected conclusion; restore clean final conclusions across the stack before proceeding.

Persist through routine implementation, review, synchronization, and recovery. Let the invoked skills own their internal methods. Use engineering judgment for task decomposition, stack mechanics, and the evidence needed to preserve cohesive boundaries.

## Preserve boundaries

- Never substitute another implementation or review workflow for `xpowers:implement` or `xpowers:review`.
- Never absorb sibling-plan scope, weaken a planned boundary, or emulate unavailable stack behavior. Return to `xpowers:plan` when evidence invalidates the series design.
- Preserve a valid repository state and independently reviewable PR at every layer.
- Treat a one-plan series as one ordinary PR with the same implementation and review guarantees; GitHub creates a remote stack only for multiple PRs.
- Do not stop for ordinary phase transitions. Ask only for a user-owned decision, missing authority, or an external blocker that remains after reasonable recovery.

## Merge

Only after every PR's final content holds a clean `xpowers:review` conclusion, present the stack status and ask whether to merge. Merge only with current user authorization, using GitHub's native stack merge for a multi-PR series and the repository's normal PR merge for a one-PR series.
