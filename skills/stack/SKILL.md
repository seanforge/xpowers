---
name: stack
description: Use when the user explicitly wants an approved Xpowers plan delivered as an adaptively shaped, formally reviewed pull-request stack.
---

# Stack

Own automated delivery of one approved `xpowers:plan` plan as one implementation-free Plan PR followed by as many implementation PRs as the actual change warrants.

Read [references/github-stack.md](references/github-stack.md) before operating the stack. Invoke `xpowers:setup` when the required GitHub capability is unavailable or unauthorized.

## Shape and deliver the stack

Resolve the complete plan. Track scopes, dependencies, assignments, integration, and review with the active harness's native planning/task tools; persist no duplicate workflow artifact. Use the committed Plan PR as the implementation-free bottom layer against trunk; do not invoke `xpowers:review` for it.

Derive the fewest PR scopes that satisfy applicable reviewability guidance, using the plan, completed layers, and actual repository surface. Split only at meaningful behavioral or architectural seams. Each scope must form a coherent requested change with a complete contract at that layer, preserve a valid repository state, and keep tests with the behavior they verify. Treat size guidance as a constraint, not a fragmentation target; use no fixed session or PR count. Fix a boundary only when assigning its scope. Assign acceptance tests to the scope that completes their path.

Follow repository PR-size guidance. When none exists, use roughly 500 changed lines of production implementation as the upper review budget per PR. Exclude tests, generated code, and planning artifacts from that estimate while treating them as review surface.

Coordinate; never implement. Assign a scope to an isolated implementer session once its dependencies are implemented, not necessarily reviewed. Reuse a session for sequential scopes when continuity helps and isolation remains valid; run independent scopes concurrently only in isolated sessions with non-overlapping write surfaces, shaped to minimize conflicts. Choose the schedule dynamically. Implementers commit only on their scope branch; the coordinator alone performs stack operations and serializes integration.

For each assigned PR scope:

1. Establish its explicit boundary, dependencies, intended parent layer, and execution isolation.
2. In the implementer's prompt, require loading `xpowers:implement` and every nested skill it requires before acting, with the complete plan and assigned scope.
3. Integrate the completed scope as one stack layer and cascade-rebase it onto its recorded parent. Resolve mechanical merge conflicts; delegate semantic conflicts and verification failures to an implementer. Run scope-relevant verification against that parent, create or update its PR with a link to the plan and a concise scope contract and dependencies, then invoke the `xpowers:review` SKILL until the layer holds a clean conclusion.

Re-evaluate delivery as implementation evidence changes the actual surface. Adjust unassigned PR boundaries, concurrency, and layer order autonomously while the plan remains valid. Reconcile concrete packaging evidence before changing an assigned boundary. If delivery requires revising the plan, or no cohesive PR scope satisfies the repository's reviewability guidance without doing so, stop accumulating scope, present the evidence, and ask whether the user wants to invoke `xpowers:plan`. Resume only from the newly approved plan.

Implementation may run concurrently, but integrate and formally review one layer at a time in stack order. Do not adopt the next layer until the current layer holds a clean conclusion.

Before finishing, reconcile the stack directly in the coordinator against the complete plan—omit or defer no settled commitment—and confirm the top layer's current head passes the acceptance tests (run them only if unverified on that head; delegate failures to an implementer). Behavioral fixes, dependency changes, or layer changes that alter a layer's diff or parent behavior invalidate affected conclusions; cascade-rebase and verify affected descendants, then restore clean conclusions in stack order. Non-behavioral updates (including, but not limited to, PR metadata, docs, verified test-only additions, or clean `git range-diff` rebases) require verification only.

## Preserve boundaries

- Never substitute another implementation or review workflow for `xpowers:implement` or `xpowers:review`.
- Every required skill—and every nested skill it requires—MUST be loaded before acting, never applied from memory. Pass this mandate in every subagent prompt, require subagents to list the skills they loaded, and resume any subagent that skipped one to load and redo its pass.
- Never expand beyond the plan or emulate unavailable stack behavior.
- Preserve a valid repository state and independently reviewable PR at every layer.
- Actively monitor convergence. When fixes proliferate or the execution graph stops becoming clearer, return to planning or diagnosis instead of adding patches.
- Produce exactly one Plan PR, one or more implementation PRs, and no other workflow artifacts; `xpowers:archive` later adds the mechanical top layer.
- Do not stop for ordinary phase transitions. Ask only for a user-owned decision, missing authority, or an external blocker that remains after reasonable recovery.

## Finish

Only after the Plan PR still matches the approved plan and every implementation PR holds a clean final `xpowers:review` conclusion, present the stack status and stop. Recommend invoking `xpowers:archive` to archive the plan and merge the stack; never archive or merge from this skill.
