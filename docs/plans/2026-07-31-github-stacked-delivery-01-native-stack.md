# Deliver plan series as GitHub stacked pull requests

## Delivery

Series: github-stacked-delivery
Plan: 1 of 1
Depends on: none
Estimated reviewable implementation change: 200–500 changed lines
Size guidance: AGENTS.md

## Problem

Xpowers distinguishes standalone plans from multi-plan series even though both represent the same domain shape: a planning output containing one or more PR plans. It also has no owner for delivery after planning, so the user must manually invoke implementation and formal review for each plan and manage their PR relationship.

GitHub now supports stacked pull requests in public preview through the `github/gh-stack` CLI extension. Xpowers needs a minimal, runtime-neutral way to prepare that capability and use it without duplicating Implement or Review, inventing persistent workflow state, or relying on stale model knowledge of the new CLI.

## Outcome

A completed `xpowers:plan` Plan Series of one or more plans can be handed to `xpowers:stack`. Stack owns the delivery outcome and invokes `xpowers:implement` and `xpowers:review` while preserving one cohesive, independently reviewable PR per plan.

Setup prepares GitHub's native stack capability for both Claude Code and Codex while preserving Codex's auditor installation. The agent retains discretion over how to organize the work, recover from change, and use the available native tooling.

## Behavioral commitments

1. `xpowers:plan` remains the human-in-the-loop design phase. Every planning output is a Plan Series containing one or more cohesive plans, each defining one PR and their meaningful dependency order.
2. `xpowers:stack` delivers the series in dependency order. For each plan, it invokes `xpowers:implement`, then `xpowers:review`, and continues their fix-and-review loop until that PR's current content holds a clean formal review conclusion.
3. After the complete series is implemented, Stack invokes a fresh `xpowers:review` for every PR in dependency order against its final intended layer. Relevant later changes invalidate affected conclusions. Only when the complete stack holds clean final conclusions may Stack ask the user whether to merge it.
4. The active harness's native task tracking plus Git and GitHub remain the only workflow state. Xpowers adds no state file, workflow runtime, or duplicate checklist.
5. Setup and Stack are available in both Claude Code and Codex. Setup establishes a supported GitHub CLI, an active account with adequate repository access, and the native `github/gh-stack` extension; in Codex it also maintains the Xpowers auditor.
6. Stack operations follow current `gh stack --help` and GitHub's official documentation. A concise reference teaches the stable model and common capabilities without replacing those primary sources.
7. Surface plan invalidation, unavailable delivery capabilities, genuine blockers, and merge choices to the user when their judgment or authority is required. Do not silently weaken the planned PR boundaries, emulate unsupported stack behavior, or merge without current user authorization.

## Implementation outline

- Update `skills/plan/SKILL.md` so every planning output is a Plan Series containing one or more PR plans, all plan files use the same series-indexed naming convention, and every completed series hands delivery to `xpowers:stack`.
- Add a concise `skills/stack/` that states the delivery order, role boundaries, final fresh-review gate, and user-authority invariants while leaving task decomposition, recovery, and tool use to the agent. Keep GitHub CLI mechanics in a focused reference.
- Update `skills/implement/SKILL.md` and `skills/review/SKILL.md` only enough to cooperate with Stack without duplicating Stack's responsibility or prescribing a rigid lifecycle.
- Expand `skills/setup/` into a cross-runtime setup skill. Keep the existing Codex auditor template behavior and add conservative GitHub CLI version, authentication, extension installation, refresh, and verification.
- Let Claude Code and Codex discover the complete standard `skills/` directory without a duplicated skill-path list. Update UI metadata to match the revised skills.
- Update `README.md` so Setup is no longer described as Codex-only and the minimality statement permits explicitly invoked skill composition without implying that Xpowers can never orchestrate phases.
- Bump the plugin patch version when the feature is ready for release.

## Testing decisions

- Validate every changed or added skill with the skill-creator validator.
- Validate both plugin manifests and the Claude marketplace JSON, then run the plugin-creator validator.
- Check that Claude Code and Codex discover Plan, Implement, Review, Setup, and Stack from the standard skills directory.
- Check that Plan contains no standalone-plan representation or direct post-plan handoff to Implement.
- Verify the setup decision paths against the locally installed `gh`: supported version, authenticated active account, target-repository write access, missing extension, installed extension, and help/version detection. Check that missing, outdated, unauthorized, and unauthenticated prerequisites request remediation rather than silently falling back. Do not mutate a real repository stack during validation.
- Forward-test `xpowers:stack` in fresh isolated agent sessions using representative one-plan and multi-plan series. Judge whether agents preserve the responsibilities and confidence boundaries while adapting their execution intelligently, without leaking an expected workflow.

## Out of scope

- Merging without current user authorization.
- Replacing GitHub's stack metadata, CLI, rebasing, checks, or merge behavior.
- Supporting third-party stack managers in this change.
- Retrofactively splitting an arbitrary existing large PR.
- Changing the internal implementation or review standards owned by the existing skills.
