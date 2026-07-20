# Xpowers

Xpowers is my minimal development flow for Claude Code and Codex. It gives each phase a focused skill without replacing the agent harness's native reasoning, task tracking, or code review capabilities.

The intended flow is:

```text
plan → implement → review → test → normal PR workflow
```

Each phase asks before handing off to the next one. There is no workflow runtime, persisted phase state, artifact schema, receipt system, or automatic chaining.

## Status

The first MVP contains `xpowers:plan`. It researches the repository and relevant external facts, grills unresolved decisions, permits disposable feasibility spikes, and writes a lightweight engineering blueprint only after the direction has no material gap.

Plans live at `.xpowers/plans/YYYY-MM-DD-<change-name>.md`. One plan corresponds to one PR. Typo, formatting, and purely mechanical rename changes may skip Xpowers; repository PR rules still apply.

## Dependencies

- The `grilling` skill for the planning conversation.
- The Subgent CLI and its `interleaved-review` skill for the future review phase.

## Plugin layout

The shared `skills/` tree is exposed through both `.claude-plugin/plugin.json` and `.codex-plugin/plugin.json`. The Claude marketplace manifest supports installing this repository as a local marketplace during development.

## Influences

- [Superpowers](https://github.com/obra/superpowers) informed the shared Claude/Codex plugin layout and skill-oriented workflow.
- [To Spec](https://github.com/mattpocock/skills/blob/main/skills/engineering/to-spec/SKILL.md) informed the emphasis on repository exploration, decision-relevant findings, and choosing a useful test seam. Xpowers deliberately keeps grilling and avoids exhaustive user stories or implementation-level plans.
