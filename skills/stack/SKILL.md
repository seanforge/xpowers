---
name: stack
description: Automate whole-series implementation and formal review across ordered pull requests. Use when the user wants one or more completed Xpowers plans delivered to a clean stack ready for archival.
---

# Stack

Own automated delivery of the provided `xpowers:plan` Plan Series as one Plan Series PR followed by one implementation PR per plan.

Read [references/github-stack.md](references/github-stack.md) before operating the stack. Invoke `xpowers:setup` when the required GitHub capability is unavailable or unauthorized.

## Deliver the series

Resolve the complete series and its dependency order from the provided plans. Use the active harness's native task tracking, Git, and GitHub as the only workflow state.

Use the committed Plan Series and its planning artifacts as the implementation-free bottom PR against trunk; do not invoke `xpowers:review` for it.

For each plan in dependency order:

1. Establish its intended branch and PR layer.
2. Invoke `xpowers:implement` for exactly that plan.
3. Create or update its PR, then invoke `xpowers:review` against that plan and layer until the PR's current content holds a clean formal review conclusion.

After the complete series is implemented, invoke a fresh `xpowers:review` for every implementation PR in dependency order against its final intended layer. Later fixes, rebases, or dependency changes invalidate any affected conclusion; restore clean final conclusions across the stack before proceeding.

Let the invoked skills own their internal methods. Use engineering judgment for task decomposition, stack mechanics, and the evidence needed to preserve cohesive boundaries.

## Preserve boundaries

- Never substitute another implementation or review workflow for `xpowers:implement` or `xpowers:review`.
- Never absorb sibling-plan scope, weaken a planned boundary, or emulate unavailable stack behavior.
- Actively monitor whether delivery is converging. If it is not, stop adding fixes and return to `xpowers:plan` to revisit the root cause, decomposition, or architecture before continuing.
- Preserve a valid repository state and independently reviewable PR at every layer.
- Produce exactly one Plan Series PR plus one implementation PR per plan; `xpowers:archive` later adds the mechanical top layer.
- Do not stop for ordinary phase transitions. Ask only for a user-owned decision, missing authority, or an external blocker that remains after reasonable recovery.

## Finish

Only after the Plan Series PR still matches the reviewed planning output and every implementation PR holds a clean final `xpowers:review` conclusion, present the stack status and stop. Recommend invoking `xpowers:archive` to archive the Plan Series and merge the stack; never archive or merge from this skill.
