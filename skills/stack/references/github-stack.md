# GitHub stacked PRs

Use the installed `github/gh-stack` extension. Check `gh stack --help` and [GitHub's current official documentation](https://docs.github.com/en/pull-requests/reference/stacked-prs-cli-commands) before relying on remembered syntax.

## Model

- A local stack is an ordered branch chain from trunk to the highest layer.
- Each PR targets the branch immediately below it; the bottom PR targets trunk.
- GitHub creates a remote stack object only when at least two PRs exist.
- Operate branch and PR arguments from bottom to top.

## Common operations

```sh
gh stack init --base <trunk> <bottom-branch> [<next-branch>...]
gh stack add <next-branch>
gh stack view --json
gh stack submit
gh stack sync
gh stack link <bottom-pr-or-branch> <next-pr-or-branch> [...]
gh stack merge
```

`submit` pushes branches, creates or updates PRs, fixes their bases, and creates or updates the remote stack. Its interactive mode controls titles, descriptions, and draft state; inspect current help before choosing non-interactive flags.

`sync` fetches, cascade-rebases, force-pushes with lease, and reconciles remote stack state. Inspect the working tree and current help before running it.

`merge` is an external state change. Run it only after the core skill's final review gate and explicit user authorization.
