# GitHub stacked PRs

Use the installed `github/gh-stack` extension. Verify the active account with `gh auth status`, then consult `gh stack <command> --help` and [GitHub's current documentation](https://docs.github.com/en/pull-requests/reference/stacked-prs-cli-commands) for operations and flags.

A stack is an ordered branch chain from trunk upward. Each PR targets the layer below it; the bottom PR targets trunk. Operate the stack from one worktree because branches checked out elsewhere block synchronization.

Create and submit layers from bottom to top:

```sh
gh stack init --base <trunk> <plan-branch>
gh stack add <implementation-branch>
gh stack submit --open
gh stack view --json
```

After trunk or a lower layer changes, run:

```sh
gh stack sync --prune
```

This fetches, cascade-rebases, pushes, updates remote stack state, and prunes merged local branches.
