# Finalize a GitHub stack

Use the installed `github/gh-stack` extension from the stack's only worktree. Verify the active account with `gh auth status`, then consult `gh stack <command> --help` before relying on remembered syntax.

Create the Archive PR as the top layer, submit it, and confirm the final stack shape:

```sh
gh stack top
gh stack add <archive-branch>
mkdir -p docs/plans/archives
git mv <plan-files> docs/plans/archives/
git commit -m '<archive commit>'
gh stack submit --open
gh stack view --json
```

Target the Archive PR when atomically squash-merging every layer:

```sh
gh stack merge <archive-top-pr> --squash
gh stack sync --prune
```

Never merge Stack members individually.
