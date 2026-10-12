# GitHub stacked PRs

Use the installed `github/gh-stack` extension. Verify the active account with `gh auth status`, then consult `gh stack <command> --help` and [GitHub's current documentation](https://docs.github.com/en/pull-requests/reference/stacked-prs-cli-commands) for operations and flags.

A stack is an ordered branch chain from trunk upward. Each PR targets the layer below it; the bottom PR targets trunk. Give each scope one branch from its dependency base; review fixes continue on it. Run stack operations from one worktree. Implementers may use temporary worktrees, but release a worktree before any stack operation touches its branch: git cannot rebase a branch checked out elsewhere.

Create and submit layers from bottom to top:

```sh
gh stack init --base <trunk> <plan-branch>
gh stack add <implementation-branch>
gh stack rebase
gh stack view --json
gh stack submit --open
```

After adopting scope branches, require every layer to contain its recorded parent before submission, verification, or review. One cascading rebase may linearize multiple adoptions.

After trunk or a lower layer changes, run:

```sh
gh stack sync --prune
```

This fetches, cascade-rebases, pushes, updates remote stack state, and prunes merged local branches.
