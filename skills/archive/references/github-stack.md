# Merge a GitHub stack

Verify the active account with `gh auth status`. From the stack's only worktree, target the Archive PR to atomically squash-merge every layer:

```sh
gh stack merge <archive-top-pr> --squash
gh stack sync --prune
```

Never merge Stack members individually.
