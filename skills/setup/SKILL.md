---
name: setup
description: Prepare Xpowers repository guidance and delivery dependencies for Claude Code or Codex. Use after installing or updating Xpowers, before xpowers:stack, or when GitHub stacked PRs or the Codex Xpowers auditor are unavailable.
---

# Set up Xpowers

## Prepare repository guidance

Ensure the repository-root instructions visible to Codex and Claude contain this policy exactly once:

> `docs/plans/archives/` contains immutable historical decision snapshots. Ignore it unless explicitly researching history; never edit archived plans or treat them as current requirements.

Create `AGENTS.md` when missing. Create a missing `CLAUDE.md` as a symlink to `AGENTS.md`; when both are independent files, preserve them and add the policy to each. Reuse shared files or symlinks without duplicating the rule.

## Prepare GitHub stacked PRs

1. Check the current official requirements and verify a supported `gh` CLI. Ask before installing or upgrading system software.
2. Verify that the active `gh` account is authenticated and can push branches and create PRs in the target repository. Never switch accounts or weaken repository permissions silently.
3. Install `github/gh-stack` when missing, or upgrade the installed extension:

   ```sh
   gh extension install github/gh-stack
   gh extension upgrade gh-stack
   ```

4. Verify `gh stack --help` and prove that the target repository exposes the GitHub Stack API with a read-only check. Treat missing rollout as an external blocker; do not create a test branch, PR, or stack merely to probe availability.

## Prepare the Codex auditor

When running in Codex, read [references/codex.md](references/codex.md) and install or refresh its auditor. Claude Code receives the auditor from the plugin and needs no auditor installation.

Do not configure model, reasoning effort, tools, MCP servers, skills, or sandbox policy.
