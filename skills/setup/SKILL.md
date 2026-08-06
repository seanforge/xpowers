---
name: setup
description: Prepare Xpowers repository guidance and delivery dependencies for Claude Code or Codex. Use after installing or updating Xpowers, before xpowers:stack, or when GitHub stacked PRs or the Codex Xpowers auditor are unavailable.
---

# Set up Xpowers

## Prepare repository guidance

Establish this repository-level policy for Codex and Claude:

> `docs/plans/archives/` contains immutable historical decision snapshots. Ignore it unless explicitly researching history; never edit archived plans or treat them as current requirements.

In a new repository, create a minimal root `AGENTS.md` and symlink a missing `CLAUDE.md` to it. In an existing repository, preserve its instruction structure and minimally add the policy to each independent root instruction source the runtimes use. Reuse shared files and symlinks without duplication.

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
