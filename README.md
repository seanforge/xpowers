# Xpowers

Xpowers is my minimal development flow for Claude Code and Codex. It gives each phase a focused skill without replacing the agent harness's native reasoning, task tracking, or code review capabilities.

The intended flow is:

```text
plan → implement → review → test → normal PR workflow
```

Each completed phase recommends the next named skill and asks before handing off. It never invokes the next phase automatically. There is no workflow runtime, persisted phase state, machine-enforced artifact schema or lifecycle, receipt system, or automatic chaining.

## Philosophy

Xpowers deliberately does not use an issue-tracker-centered or living-spec workflow. Detailed workflow artifacts drift from the implementation, duplicate what capable agents can learn from the repository, and can constrain better decisions with stale assumptions. Code and executable tests are the ground truth; skills provide lightweight intent and quality gates without replacing the agent harness's judgment.

The dated `plan.md` is not a living specification. It is a per-PR engineering blueprint produced after evidence-driven grilling: the intended outcome, meaningful boundaries and decisions, and how the change will be validated. It may be revised with the user while that change is in progress. Once the PR is complete, it remains only as a historical snapshot of the direction agreed at that time.

Planning uses `grill-with-docs` as its ongoing conversation mode, interleaving repository inspection, domain modeling, up-to-date primary-source research, user decisions, and disposable feasibility spikes as uncertainty demands. It writes a lightweight engineering blueprint only after the problem, behavioral commitments, implementation decisions, and testing seams are agreed and no material gap remains.

Implementation completes every in-scope commitment in the latest approved plan, including the executable tests needed to establish the changed behavior. It uses the external `tdd` skill where possible at plan-agreed seams, selects the smallest reliable test boundary, and runs focused checks throughout before one complete change-relevant verification pass. When E2E is warranted, backend behavior runs through the repository's local HTTP/API harness and Web or Electron journeys use its Playwright setup. Long-lived scenarios follow repository conventions and stable product capabilities; generated reports and traces belong in CI artifacts, not Git. The agent uses native task tracking and performs worktree writes sequentially, either directly or through one implementation subagent at a time.

Review gives each harness's native reviewer the same complete change and approved plan, requiring whole-change review through the non-compensating lenses of intent alignment and engineering soundness. The Subgent `interleaved-review` loop keeps the reviews independent, exchanges and reconciles their findings, and repeats after fixes until a fresh clean round passes; no review artifact is persisted.

Test formally validates the unchanged, reviewed implementation with repository-native checks and committed change-relevant scenarios against the supported local system. Backend behavior uses the repository's HTTP/API harness, while Web and Electron behavior uses Playwright. Complete historical regression and platform matrices remain merge-CI responsibilities. Any source or test fix invalidates the prior review and returns the change through `xpowers:review` before testing resumes; no test receipt is persisted.

Plans live at `docs/plans/YYYY-MM-DD-<change-name>.md`. One plan corresponds to one PR. Necessary glossary and ADR changes created during planning are committed with the plan. Typo, formatting, and purely mechanical rename changes may skip Xpowers; repository PR rules still apply.

## Prerequisites

Install these dependencies before using Xpowers:

- [Seanforge Skills](https://github.com/seanforge/skills)
- [Matt Pocock's Skills](https://github.com/mattpocock/skills)
- [Subgent](https://gitlab.com/xiang9156/subgent), including its `sub` CLI and `interleaved-review` skill

Xpowers references these tools and skills instead of vendoring them. Follow each repository's installation instructions so the skills and `sub` CLI are available to Claude Code or Codex.

## Plugin layout

The shared `skills/` tree is exposed through both `.claude-plugin/plugin.json` and `.codex-plugin/plugin.json`. The Claude marketplace manifest supports installing this repository as a local marketplace during development.

## Influences

- [Matt Pocock's skills collection](https://github.com/mattpocock/skills), especially To Spec, test-driven development, and Code Review, informed the emphasis on repository exploration, decision-relevant implementation and testing choices, public test seams, and separating intent alignment from engineering soundness. Xpowers keeps grilling, avoids exhaustive user stories or implementation-level plans, and uses its own interleaved review protocol.
