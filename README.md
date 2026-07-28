# Xpowers

Xpowers is my personal development-flow plugin for Claude Code and Codex. It collects a small set of skills I use regularly to shape and deliver repository changes.

The `X` stands for **cross-reference**. Rather than reimplement every capability, Xpowers references and composes strong skills and practices from the wider engineering and agent ecosystem, adapting them only where needed to form a coherent personal flow.

Xpowers stays minimal. Its skills remain composable, while each agent harness keeps its native reasoning, task tracking, subagents, and review capabilities. Code and executable tests remain the ground truth; Xpowers adds no workflow engine, persisted phase state, duplicate task tracker, or automatic phase chaining.

The exact skills, dependencies, triggers, and handoffs live in their own `SKILL.md` files. This README intentionally does not duplicate that changing surface.

## Setup

Install Xpowers through the plugin flow supported by the active agent harness, and make referenced external skills available from their own sources.

After installing or updating Xpowers in Codex, invoke `xpowers:setup` once and restart Codex so its native finding auditor is available. Claude Code receives the same auditor directly from the plugin.
