---
name: plan
description: Use when a repository change requires design decisions, clarification, or evidence-backed research before implementation.
---

# Plan

Explore the codebase and close consequential design gaps with the user, then capture the requested change as a feasible, user-confirmed implementation handoff. Establish the behavioral contract and consequential decisions; leave execution packaging and local implementation detail to later judgment.

## Shape the change

Run a `grilling` session throughout shaping.

Compose the applicable skills. Let each own its decisions and preserve its material outputs in the owning artifacts:

- Invoke `domain-modeling` when domain terminology, relationships, or context boundaries need resolution, or when a settled decision may warrant an ADR.
- Invoke `skills:solution-design` when no decision-complete implementation direction exists.
- Invoke `codebase-design` when module, interface, seam, adapter, or architecture design is consequential.
- Invoke `skills:valuable-tests` to select the maintained automated-test portfolio.

Ignore `docs/plans/archives/` unless historical decisions are explicitly relevant. Never treat archived plans as current requirements or editable documentation.

Continue until the intended behavior, user-owned decisions, decision-complete implementation outline, and applicable testing decisions are settled. Then ask whether to write the plan.

## Write the plan

Write the plan as a resumable implementation handoff: concise for an engineer who knows the context, yet explicit enough for a fresh agent to recover the necessary context from the repository, assess completeness, and implement without rediscovering settled decisions.

Create one plan file under `docs/plans/`, using the current local date and `YYYY-MM-DD-<change>.md` with a concise kebab-case name.

```markdown
# <Change title>

## Problem

The problem and relevant current behavior, with the minimum product and technical context needed for implementation.

## Outcome

What becomes true when the change is complete, from the affected user's or operator's perspective when applicable, including its meaningful boundary.

## Behavioral commitments

A numbered list of observable success, failure, and boundary behaviors the implementation must satisfy. Use actor, intent, and benefit when they clarify a user-facing behavior; do not force user-story syntax onto internal engineering work.

## Implementation outline

Serialize the implementation direction under the `skills:solution-design` Outline contract, including applicable `codebase-design` decisions. Preserve every consequential decision; do not reduce it to a summary or task list.

## Testing decisions

Serialize the minimum sufficient maintained-test portfolio selected under `skills:valuable-tests`, plus known useful commands.

## Out of scope

Explicit exclusions when the boundary could otherwise be misread.

## Future

Optional. Plausible follow-up work explicitly outside the planned outcome. Preserve only enough context to make the current boundary clear; do not turn it into a committed roadmap.
```

Treat the template as coverage prompts, not a closed schema. Organize settled decisions by meaning, not by source; add a section only when no existing section can preserve a material decision without distortion. Keep the plan decision-complete, implementation-light, bounded, and verifiable. Make the behavioral commitments complete enough to prevent omitted behavior, not an exhaustive inventory of hypothetical stories. Preserve only research that affects a decision.

When a disposable prototype expresses a decision more precisely than prose, include only its smallest decision-rich excerpt, such as a state machine, reducer, schema, or type shape, and identify it as prototype evidence.

The writer remains responsible for checking the complete planning output against the settled user decisions, repository evidence, and this SKILL. Resolve omissions, contradictions, duplicated context, and assumptions that exist only in the conversation before treating it as ready.

## Review the plan

Treat the plan and any domain documentation or ADRs created or substantively revised with it as one peer-review unit. Review it through one read-only reviewer session at a time. It must start through the active harness's native isolation mechanism with no inherited writer conversation context. Preserve and resume the existing reviewer while it remains available. If it cannot be recovered, start a fresh isolated replacement with the complete unit and authoritative context; never waive review or persist workflow state merely to preserve a session handle.

Brief the reviewer on the user's current intent, settled user-owned decisions, the result the review must establish, and material focus or evidence. Provide an authoritative, accessible source for that intent and those decisions, directly or through discoverable references such as the original request or repository instructions. Exclude superseded, repetitive, or irrelevant discussion. Do not prescribe how the reviewer explores, reasons, or reaches its judgment.

The reviewer reads the plan from the repository, judges it against the planning contract in this SKILL without executing its workflow, and independently inspects whatever repository evidence it considers necessary. Require it to read and apply the applicable `skills:solution-design`, `skills:valuable-tests`, `domain-modeling`, and `codebase-design` contracts to their owned outputs without executing their authoring workflows. Reject a handoff whose required information exists only in audit context rather than the plan or its discoverable references.

The writer owns fixes; the reviewer remains read-only and neither edits the plan, settles unresolved user-owned decisions, nor invokes the formal `xpowers:review` SKILL. Require an explicit approve-or-reject verdict based on whether the complete unit is a trustworthy handoff from which a capable implementation agent can begin without rediscovering consequential design decisions. Resume the reviewer as needed; ask the user when a finding exposes an unresolved user-owned decision.

## Hand off

Present the current written peer-review unit to the user and revise it until they are satisfied. User review and peer review may occur in either order and iterate as needed. Do not commit until the same current unit has both user satisfaction and reviewer approval. Any substantive revision invalidates both gates and requires renewal; purely editorial changes that cannot alter meaning invalidate neither.

Then commit the plan and any glossary or ADR changes produced during planning together using the repository's commit conventions; when no domain documentation changed, commit only the plan. Recommend `xpowers:implement` for direct execution or `xpowers:stack` for managed delivery. Ask whether to begin, and never invoke either automatically.
