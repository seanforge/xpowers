---
name: archive
description: Archive a completed Xpowers Plan Series in a final pull request and atomically squash-merge its GitHub PR stack. Use only when the user explicitly invokes xpowers:archive after every implementation PR has a current clean xpowers:review conclusion.
---

# Archive

Finish the reviewed Plan Series with one mechanical top-layer PR. The user's explicit invocation authorizes creating that PR and atomically squash-merging the complete stack.

Read [references/github-stack.md](references/github-stack.md) before operating the stack.

1. Resolve the complete Plan Series and its clean stack, and verify that every implementation PR still holds its final clean `xpowers:review` conclusion.
2. Create one top stack layer and move every plan in the series from `docs/plans/` to `docs/plans/archives/`, preserving filenames and contents.
3. Verify that the PR contains only those renames, then create or update the Archive PR. Do not invoke `xpowers:review`.
4. Atomically squash-merge the complete stack through the Archive PR, then synchronize and prune the local stack.

Never archive only part of a series, edit an archived plan, merge stack members individually, or include unrelated changes in the Archive PR.
