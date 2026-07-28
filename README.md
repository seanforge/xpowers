# Xpowers

The `X` means cross. Xpowers is my minimal personal engineering workflow for Claude Code and Codex, built by combining strong skills from different engineering ecosystems.

Xpowers mostly does not reinvent those skills. It references them directly where they already fit and borrows their strongest ideas where adaptation produces a more coherent personal workflow.

## Prerequisites

Xpowers intentionally depends on these external skills and projects instead of copying their capabilities:

- [Matt Pocock's Skills](https://github.com/mattpocock/skills) is the primary foundation for planning and implementation. Xpowers directly uses its skills and adapts ideas from To Spec and Code Review.
- Claude Code's official `/code-review` skill supplies the Claude-side native reviewer.
- My [Seanforge Skills](https://github.com/seanforge/skills) repository provides the required `clean-code` and `valuable-tests` skills.

Make each skill and tool available to Claude Code or Codex, following its source's installation instructions where applicable.

## What Xpowers combines

- **Plan:** `grill-with-docs` plus an issue-tracker-free adaptation of Matt Pocock's To Spec approach.
- **Implement:** Matt Pocock's `tdd` at agreed seams, alongside Seanforge's `clean-code` and `valuable-tests` skills.
- **Review:** the active harness's native reviewer plus a separate same-runtime finding auditor, coordinated by Xpowers.
- **Test:** repository-native checks and committed scenarios, kept separate from implementation and review.

Xpowers owns the cross-skill contract, not the underlying capabilities. External skills remain independent dependencies and each agent harness keeps its native reasoning, task tracking, and review judgment.

The intended flow is:

```text
plan → implement → review → test → normal PR workflow
```

Each phase recommends the next named skill and asks before handing off. It never invokes the next phase automatically.

There is no workflow runtime, persisted phase state, machine-enforced artifact lifecycle, receipt system, duplicate task tracker, or automatic chaining.

## Philosophy

Code and executable tests are the ground truth. Xpowers uses a lightweight plan and explicit quality gates without turning the workflow into a living-spec or issue-tracker-centered system.

A dated plan is a per-PR engineering blueprint. It captures the agreed problem, outcome, behavioral commitments, meaningful implementation decisions, testing seams, and scope boundaries.

The plan may change with user approval while its PR is in progress. After the PR is complete, the plan remains a historical snapshot rather than an artifact that must stay synchronized forever.

Planning uses `grill-with-docs` as its conversation mode. It combines repository evidence, domain modeling, current primary sources, user decisions, and disposable feasibility spikes.

Implementation completes every in-scope commitment and its executable evidence. It uses `tdd` where possible at plan-agreed seams and runs focused checks throughout before one complete change-relevant verification pass.

Review keeps the active agent as coordinator. A native reviewer inspects the complete change, and a separate same-runtime auditor brutally challenges its findings before the coordinator applies agreed fixes.

Test formally validates the unchanged, reviewed implementation with repository-native checks. Any source or test fix invalidates the review result and sends the change back through `xpowers:review`.

Plans live at `docs/plans/YYYY-MM-DD-<change-name>.md`. One plan corresponds to one PR, while `Future` may preserve boundaries, ordering, and dependencies for larger follow-up PRs.

Necessary glossary and ADR changes created during planning are committed with the plan. Typo, formatting, and purely mechanical rename changes may skip Xpowers; repository PR rules still apply.

## Plugin layout

The shared `skills/` tree is exposed through both `.claude-plugin/plugin.json` and `.codex-plugin/plugin.json`.

The Claude marketplace manifest supports installing this repository as a local marketplace during development.
