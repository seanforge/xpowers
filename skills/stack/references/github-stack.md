# GitHub stacked PRs

Use the installed `github/gh-stack` extension. Verify the active account with `gh auth status`, then consult `gh stack <command> --help` and [GitHub's current documentation](https://docs.github.com/en/pull-requests/reference/stacked-prs-cli-commands) for operations and flags.

A stack is an ordered branch chain from trunk upward. Each PR targets the layer below it; the bottom PR targets trunk. Operate the stack from one worktree because branches checked out elsewhere block synchronization.

After trunk or a lower layer changes, run:

```sh
gh stack sync --prune
```

Run this before the final fresh-review gate. If synchronization changes a reviewed layer, its review conclusion is stale.

After every final review is clean, stop for explicit user authorization, then squash-merge the stack atomically from its top. This preserves one reviewed Plan/PR as one mainline commit.

```sh
gh stack merge <stack-or-top-pr> --squash
```

Never merge Stack members individually. After merge, run `gh stack sync --prune` to update trunk and prune merged branches.
