---
name: setup
description: Prepare Xpowers delivery dependencies for Claude Code or Codex. Use after installing or updating Xpowers, before xpowers:stack, or when GitHub stacked PRs or the Codex Xpowers auditor are unavailable.
---

# Set up Xpowers

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

When running in Codex, install the bundled [auditor template](assets/xpowers-auditor.toml) globally:

1. Resolve the user's home directory and ensure `.codex/agents/` exists beneath it.
2. Replace only `.codex/agents/xpowers-auditor.toml` with the exact template contents. Do not modify any other agent file.
3. Verify that the installed file is byte-for-byte identical to the template and parses as TOML.
4. Tell the user to restart Codex when the auditor was created or changed.

Claude Code receives its auditor from the plugin and needs no auditor installation.

Do not configure model, reasoning effort, tools, MCP servers, skills, or sandbox policy.
