---
name: plan
description: Use when a repository change requires design decisions, clarification, or evidence-backed research before implementation.
---

# Plan

Explore the codebase and close consequential design gaps with the user, then capture a feasible, user-approved direction for implementation. Establish the behavioral contract and decisions that matter; leave local implementation detail to the implementing agent.

## Shape the change

Run a `/grilling` session, using the `/domain-modeling` skill.

**CONDITIONAL SKILL INVOCATION:** Invoke the `codebase-design` SKILL when planning a module, interface, seam, adapter, architectural change, or refactor.

Planning is an evidence loop, not a fixed sequence. For each important uncertainty, use whichever action produces the strongest evidence:

- Inspect repository instructions, domain glossaries, ADRs, code, tests, documentation, configuration, and history for the codebase's actual behavior and constraints.
- Search current primary sources for external facts that shape the direction. Always verify libraries, APIs, tools, platforms, standards, security guidance, and other time-sensitive knowledge, even when the answer feels familiar. Prefer official documentation, release notes, specifications, and source code current at the time of planning.
- Run a disposable spike when reading cannot establish feasibility. Keep it out of production code.
- Ask the user one decision at a time when evidence cannot settle a product boundary or meaningful tradeoff.

Treat model memory as a source of search terms, never as evidence. Do not assume that code exists, an API still behaves the same way, a remembered version is current, or an old recommendation remains sound. Separate verified facts from inference.

Treat the current architecture as evidence, not a constraint that must be preserved. Assess whether its responsibilities and seams support the desired behavior cleanly. When they would force broken boundaries or workaround-on-workaround changes, recommend the smallest root-cause refactor and explain whether it is a prerequisite or a deferrable follow-up. Do not expand scope for unrelated cleanup.

Before writing, sketch the public seams that can establish the planned behavior. Prefer the highest useful existing seam that preserves the relevant failure model, and prefer fewer seams over duplicated coverage. Confirm the seams with the user.

Repeat until evidence supports a feasible direction, the user-owned decisions are settled, and the remaining uncertainty is local implementation judgment. Then ask whether to write the plan.

Do not invent architecture to fill an information gap. Explore or ask instead.

## Shape delivery

After the change is understood, estimate the implementation surface and shape it into reviewable PRs. Do not force PR boundaries before the behavioral, architectural, and testing decisions are clear enough to support them.

Follow explicit repository PR-size guidance from files such as `AGENTS.md`, `CLAUDE.md`, or `CONTRIBUTING.md`. When the repository provides none, target 200–500 changed lines and treat 500–1,000 as large but acceptable only when the change remains highly cohesive; split work expected to exceed roughly 1,000 lines.

Count production code, tests, configuration, schemas, migrations, and delivery-required product documentation. Exclude plan documents under `docs/plans/` and generated artifacts from the estimate.

Each plan owns one PR. When the complete change exceeds one reviewable PR, split it along behavioral or architectural boundaries into an ordered plan series. Keep tests with the behavior they verify, make dependencies explicit, and preserve a valid repository state after every PR.

A single planning session may produce multiple plans when evidence supports their boundaries and decisions. Later plans inherit the decisions settled during that session; implementation revalidates them against the merged repository state and reopens only decisions contradicted by new evidence.

Treat each plan as a dated decision snapshot for its PR, not a living source of truth. Revise it until the PR merges, then preserve it as history; capture later decisions in a new plan while code, tests, domain documentation, and ADRs remain the current truth.

## Write the plan

Create one plan file per PR under `docs/plans/`, using the current local date and concise kebab-case names. Use `YYYY-MM-DD-<change-name>.md` for a standalone plan and `YYYY-MM-DD-<series>-NN-<slice>.md` for an ordered series.

```markdown
# <Change title>

## Delivery

Series: <series name or standalone>
Plan: <position of total>
Depends on: <earlier plan paths or none>
Estimated reviewable implementation change: <rough changed-line range>
Size guidance: <repository instruction source or Xpowers fallback>

## Problem

The problem and relevant current behavior, from the affected user's or operator's perspective.

## Outcome

What becomes true from that perspective when the change is complete, including its meaningful boundary.

## Behavioral commitments

A numbered list of observable success, failure, and boundary behaviors the implementation must satisfy. Use actor, intent, and benefit when they clarify a user-facing behavior; do not force user-story syntax onto internal engineering work.

## Implementation decisions

Confirmed direction-level decisions that orient the work: affected modules, interfaces, data flows, API or schema contracts, cross-boundary interactions, architectural choices, material alternatives and trade-offs, user clarifications, risks, and constraints. Include compatibility, migration, rollout or rollback, and observability only when they materially affect the change.

## Testing decisions

The agreed public seams and modules or surfaces under test; the observable behavior established at each seam, never implementation details; relevant repository prior art; and change-specific coverage boundaries. Include known, useful commands.

## Out of scope

Explicit exclusions when the boundary could otherwise be misread.

## Future

Optional. Plausible follow-up work explicitly outside the approved outcome and plan series. Preserve only enough context to make the current boundary clear; do not turn it into a committed roadmap.
```

Treat the template as coverage prompts, not a demand for exhaustive detail. Keep each plan rough, solved, bounded, and verifiable. Make the behavioral commitments complete enough to prevent omitted behavior, not an exhaustive inventory of hypothetical stories. Preserve only research that affects a decision. Describe decisions without implementation file paths, task checklists, pseudocode, ordinary code snippets, exact edits, or commit sequences.

When a disposable prototype expresses a decision more precisely than prose, include only its smallest decision-rich excerpt, such as a state machine, reducer, schema, or type shape, and identify it as prototype evidence.

## Hand off

Ask the user to review the written plan or plan series. Revise it until approved. After approval, commit the plans and any glossary or ADR changes produced during planning together using the repository's commit conventions; when no domain documentation changed, commit only the plans. Then recommend invoking the `xpowers:implement` SKILL to implement the first approved plan and ask whether to begin it. Never invoke it automatically.
