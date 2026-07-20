---
name: plan
description: Use when a non-mechanical repository change needs an agreed, evidence-backed direction before implementation.
---

# Plan

Turn an idea into a feasible, user-approved engineering blueprint. Build the direction from current evidence; leave implementation detail to the implementing agent.

## Shape the change

Planning is an evidence loop, not a fixed sequence. Use the `grilling` skill as the conversation mode. For each important uncertainty, use whichever action produces the strongest evidence:

- Inspect repository instructions, code, tests, documentation, configuration, and history for the codebase's actual behavior and constraints.
- Search current primary sources for external facts that shape the direction. Always verify libraries, APIs, tools, platforms, standards, security guidance, and other time-sensitive knowledge, even when the answer feels familiar. Prefer official documentation, release notes, specifications, and source code current at the time of planning.
- Run a disposable spike when reading cannot establish feasibility. Keep it out of production code.
- Ask the user one decision at a time when evidence cannot settle a product boundary or meaningful tradeoff.

Treat model memory as a source of search terms, never as evidence. Do not assume that code exists, an API still behaves the same way, a remembered version is current, or an old recommendation remains sound. Separate verified facts from inference.

Repeat until evidence supports a feasible direction, the user-owned decisions are settled, and the remaining uncertainty is local implementation judgment. Then ask whether to write the plan.

Do not invent architecture to fill an information gap. Explore or ask instead.

## Write the plan

Create `.xpowers/plans/YYYY-MM-DD-<change-name>.md`, using the current local date and a concise kebab-case change name.

```markdown
# <Change title>

## Outcome

What becomes true when this change is complete, including its meaningful boundary. State relevant exclusions when scope could be misread.

## Blueprint

Confirmed decisions that orient the work: current behavior; affected modules, boundaries, and data flows; interface, API, or schema contracts; cross-boundary interactions; and only architectural choices, user clarifications, risks, or constraints that shape the direction.

## Validation

Observable external behaviors, surfaces under test, the highest useful existing seams, relevant repository prior art, and change-specific coverage boundaries. Include known, useful commands.

## Future

Optional. Deferred work, or the boundaries, order, and dependencies of a multi-PR sequence.
```

Keep the plan rough, solved, bounded, and verifiable. Preserve only research that affects a decision. Describe decisions without specific file paths, task checklists, pseudocode, ordinary code snippets, exact edits, or commit sequences.

When a disposable prototype expresses a decision more precisely than prose, include only its smallest decision-rich excerpt, such as a state machine, reducer, schema, or type shape, and identify it as prototype evidence.

## Hand off

Ask the user to review the written plan. Revise it until approved. After approval, commit the plan by itself using the repository's commit conventions, then recommend `xpowers:implement` and ask whether to begin it. Never invoke it automatically.
