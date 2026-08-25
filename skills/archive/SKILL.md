---
name: archive
description: Use when the user explicitly wants to archive a completed Xpowers plan and atomically merge its reviewed GitHub PR stack after every implementation PR has a current clean xpowers:review conclusion.
---

# Archive

Finish the reviewed change with one mechanical top-layer PR. The user's explicit invocation authorizes creating that PR and atomically merging the complete stack.

Read [references/github-stack.md](references/github-stack.md) before operating the stack.

1. Resolve the current clean PR stack and the exact plan it delivers.
2. Add a top Archive PR that moves only that plan file, unchanged, from `docs/plans/` to `docs/plans/archives/`; verify rename-only and do not review.
3. Before any merge mutation, confirm the base permits direct atomic stack merge. A required merge queue does not; report the blocker and stop. Otherwise atomically merge through the Archive PR, then synchronize and prune the local stack.

Never archive an incomplete change, edit an archived plan, merge stack members individually, or include unrelated changes in the Archive PR.
