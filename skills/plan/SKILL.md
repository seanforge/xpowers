---
name: plan
description: Use when a non-mechanical repository change needs an agreed, research-backed direction before implementation.
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

What becomes true when this change is complete, including its meaningful boundary.

## Blueprint

The research-informed implementation shape: relevant current behavior, boundaries or data flows that change, likely modules or representative paths, and only the tradeoffs, risks, or constraints that affect the direction.

## Validation

The observable behaviors to prove and the appropriate test layers, using the highest useful existing test seam. Include commands only when they are already known and useful.

## Future

Optional. Deferred work, or the boundaries, order, and dependencies of a multi-PR sequence.
```

Keep the plan rough, solved, bounded, and verifiable. Preserve only research that affects a decision. Likely paths orient the implementer; they are not binding. Omit task checklists, pseudocode, code snippets, signatures, exact edits, and commit sequences.

## Hand off

Ask the user to review the written plan. Revise it until approved. After approval, commit the plan by itself using the repository's commit conventions, then ask whether to begin implementation. Never start the next phase automatically.
