---
name: archive
description: Archive a completed Xpowers Plan Series in a final pull request and atomically squash-merge its GitHub PR stack. Use only when the user explicitly invokes xpowers:archive after every implementation PR has a current clean xpowers:review conclusion.
---

# Archive

Finish the reviewed Plan Series with one mechanical top-layer PR. The user's explicit invocation authorizes creating that PR and atomically squash-merging the complete stack.

Read [references/github-stack.md](references/github-stack.md) before operating the stack.

1. Resolve the current clean PR Stack and the exact Plan Series it delivers.
2. Add a top Archive PR that moves only that series's plan files, unchanged, from `docs/plans/` to `docs/plans/archives/`; verify rename-only and do not review.
3. Atomically squash-merge through the Archive PR, then synchronize and prune the local stack.

Never archive only part of a series, edit an archived plan, merge stack members individually, or include unrelated changes in the Archive PR.
