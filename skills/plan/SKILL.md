---
name: plan
description: Use when a non-mechanical repository change needs an agreed, research-backed direction before implementation.
---

# Plan

Turn an idea into a feasible, user-approved engineering blueprint. Research before writing; leave implementation detail to the implementing agent.

## Shape the change

1. Read the repository instructions. Inspect the relevant code, tests, documentation, and history before proposing a direction. Research current external facts from primary sources when they affect the decision.
2. Use the `grilling` skill to clarify the desired outcome, observable behavior, scope boundaries, constraints, and user-owned tradeoffs. Ask one decision at a time.
3. Resolve uncertainties that could invalidate the direction. For a new runtime, framework, database, external service, or architectural dependency, investigate feasibility and consequences and get the user's agreement. Run a disposable spike when reading cannot answer the question; do not turn spike code into production code by accident.
4. Continue until the direction is feasible and the remaining uncertainty is local implementation judgment. Then ask whether to write the plan.

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
