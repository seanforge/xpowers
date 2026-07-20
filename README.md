# Xpowers

Xpowers is my minimal development flow for Claude Code and Codex. It gives each phase a focused skill without replacing the agent harness's native reasoning, task tracking, or code review capabilities.

The intended flow is:

```text
plan → implement → review → test → normal PR workflow
```

Each phase asks before handing off to the next one. There is no workflow runtime, persisted phase state, artifact schema, receipt system, or automatic chaining.

## Status

The MVP currently contains `xpowers:plan` and `xpowers:implement`.

Planning uses grilling as its ongoing conversation mode, interleaving repository inspection, up-to-date primary-source research, user decisions, and disposable feasibility spikes as uncertainty demands. It writes a lightweight engineering blueprint only after evidence supports the direction and no material gap remains.

Implementation follows test-driven development and owns production code plus executable automated tests. Backend changes receive change-scoped local API scenarios; Web and Electron changes receive Playwright scenarios. Existing E2E layouts win; otherwise tests live under the nearest product root's `e2e/<domain-or-subsystem>/`, with runtime-specific file suffixes and shared harnesses in `e2e/support/`. Generated reports and traces belong in CI artifacts, not Git. The agent uses native task tracking and performs worktree writes sequentially, either directly or through one implementation subagent at a time.

The future review phase will review production code, unit and integration tests, and E2E scripts together, checking that the approved behavior has comprehensive coverage before the separate test phase runs it.

Plans live at `.xpowers/plans/YYYY-MM-DD-<change-name>.md`. One plan corresponds to one PR. Typo, formatting, and purely mechanical rename changes may skip Xpowers; repository PR rules still apply.

## Dependencies

- The `grilling` skill for the planning conversation.
- The `skills:valuable-tests` skill for test boundaries, cases, doubles, and suite quality during implementation and review.
- The Subgent CLI and its `interleaved-review` skill for the future review phase.

## Plugin layout

The shared `skills/` tree is exposed through both `.claude-plugin/plugin.json` and `.codex-plugin/plugin.json`. The Claude marketplace manifest supports installing this repository as a local marketplace during development.

## Influences

- [Superpowers](https://github.com/obra/superpowers) informed the shared Claude/Codex plugin layout and skill-oriented workflow.
- [Matt Pocock's skills collection](https://github.com/mattpocock/skills), especially To Spec and test-driven development, informed the emphasis on repository exploration, decision-relevant implementation and testing choices, public test seams, and vertical slices. Xpowers deliberately keeps grilling and avoids exhaustive user stories or implementation-level plans.
