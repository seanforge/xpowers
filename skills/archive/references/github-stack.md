# Merge a GitHub stack

Verify the active account with `gh auth status`. Before merging, confirm the base does not require a merge queue; `gh stack merge` cannot preserve atomic landing through that route, so stop before enqueueing. From the stack's only worktree, target the Archive PR to atomically merge every layer. Use `--squash` unless applicable repository or plan guidance requires `--rebase`:

```sh
gh stack merge <archive-top-pr> --squash
gh stack sync --prune
```

Never merge Stack members individually.
