---
name: review
description: Use when an assigned code-change scope is authorized for independent review, finding adjudication, and root-cause fixes through Implement until clean.
---

# Review

Establish a trustworthy conclusion about the assigned review scope. Use this skill as a reasoning framework, not a rigid workflow: preserve its semantic boundaries while adapting the interaction to the scope, evidence, and active agent runtime.

## Coordinate; do not review

Act only as coordinator. Own the review contract, session orchestration, evidence relay, fix delegation, and stop decision. Never review or edit the artifact, or substitute your judgment for the reviewer or finding auditor.

Identify the agent harness currently running you, then read exactly its matching reference:

- If you are running in Claude Code, read [references/claude.md](references/claude.md).
- If you are running in Codex, read [references/codex.md](references/codex.md).

Do not ask the user to choose, read the other runtime reference, start the other runtime, or invoke a cross-model review workflow.

## Establish the review contract

Resolve the assigned scope, intent, comparison point, and declared dependencies from the available context. Treat the scope as authoritative and verify that it is non-empty. Review its complete current artifact, including committed, staged, and unstaged production and test changes. An immutable target remains review-only until the invoker provides a mutable branch or worktree for fix delegation. Ask the invoker only when the boundary or intent remains ambiguous.

Require the reviewer to assess whether the assigned scope is cohesive, reviewable under the applicable guidance, aligned with its intent, and consistent with its dependencies.

When delegating scope cleanup, authorize removal only of changes attributable to the reviewed implementation; preserve unrelated or user-owned work and ask when ownership is uncertain.

Brief every reviewer on the user's intent, complete scope, result to establish, repository instructions, and material focus or evidence. Give it a pragmatic staff-engineer mandate to assess both intent alignment and engineering soundness. Treat production code and executable tests as one review surface. The reviewer chooses its reasoning tools, additional evidence, and exploration depth. Ask for review only: no edits.

Require the review to establish intent alignment, correctness, maintainability, whether the change has the valuable executable tests and other sufficient evidence warranted by its behavior and risks, and architectural soundness where applicable.

Require the reviewer to read and apply `skills:clean-code`, `skills:valuable-tests`, `codebase-design`, and every applicable repository- or technology-specific SKILL as review criteria, not authoring workflows.

## Preserve the invariants

Keep orchestration context out of delegated prompts. Make each prompt self-contained around its assigned role, scope, and evidence, without revealing or assigning responsibility for the broader workflow.

Fresh always means a new isolated session created through the active runtime's context-isolation mechanism, with no inherited coordinator, role, or prior-round conversation context. Give each fresh role deliberately selected starting context; it chooses any additional evidence and exploration depth.

- Every round starts with a fresh reviewer session and reviews the complete current artifact within the assigned scope, never only the latest delta.
- A reviewer and auditor always occupy different sessions.
- An auditor belongs only to the round in which it is created and exists only after that round's reviewer reports findings.
- Reuse the active reviewer and auditor sessions throughout their round and never across rounds.
- An auditor challenges only the submitted findings; it does not independently review the change or search for new issues.
- Freeze the artifact during review, audit, and reconciliation. Change it only after reviewer and auditor agreement through `xpowers:implement`.
- A valid reviewer result that reports no findings never starts or resumes an auditor.
- A failed, timed-out, empty, delegated, or otherwise invalid response never counts as clean. Neither does a fix, elapsed time, or coordinator confidence.
- Never leave a disputed finding unresolved or create a parallel finding ledger.

Retry every failed, timed-out, empty, delegated, or otherwise invalid review or audit response. Prefer retrying or resuming the saved session. If that session cannot be resumed reliably, create a fresh replacement for only that role, give it the complete review contract and unresolved context, and keep the other role unchanged. The replacement inherits the current round; it does not start a fresh-session round or reset that round's state.

## Drive to completion

Persist until the applicable clean outcome is established. Do not stop at a progress update, a pending reviewer or auditor, an unresolved finding, or an applied fix. Do not ask the user whether to continue ordinary review-and-fix rounds. Wait for active work, recover or resume reusable sessions, and keep driving reconciliation, fixes, and full re-review.

A non-clean return is limited to reconciled scope- or decision-invalidating evidence, or a genuine external blocker that remains after reasonable recovery. Return it only to the invoker. Persistence means owning the outcome, not defending a failing implementation direction.

## Reconcile findings

Start with a fresh native reviewer. On the round's first findings, start the active runtime's Xpowers auditor as a fresh session. Give it the findings, their reasoning and evidence, the review contract, and access to the relevant code.

Treat reconciliation as a direct, evidence-driven argument between reviewer and auditor. Exchange evidence or reasoning when it could change the other role's position, and require both to answer every unresolved point. Explicit acceptance of an unchanged position completes reconciliation without another turn. Adapt the number and shape of exchanges to the evidence; do not summarize away disagreement or adjudicate it yourself.

Guide both roles toward case-by-case engineering judgment. A finding may be technically true yet immaterial. What matters is whether it represents a grounded, material risk under credible real-world conditions for this system.

Reconciliation ends only when both roles agree on each finding's validity, materiality, cause, severity, and coherent fix direction. Treat the reconciled finding set as authoritative. Delegate only the agreed root-cause fix set to an isolated `xpowers:implement` session within the assigned scope; return unresolved product, architectural, or scope decisions to the invoker.

## Fix root causes, not symptoms

Treat the causal explanation as part of finding resolution. Judge a fix by whether it corrects the causal model and strengthens a coherent invariant, not merely by whether the current finding disappears.

A converging fix loop makes the system model more coherent while reducing risk and incidental complexity; persist through it to clean. A non-converging loop is revealed when new reviewer findings show fixes proliferating or relocating related problems. Treat that pattern as evidence—not automatic proof—that the causal model, design, or architecture may be wrong.

On non-convergence, stop fix delegation. Return the current finding and pattern to the round's active reviewer and auditor, and reconcile a simpler coherent direction before changing code again. Return it to the invoker only when that direction requires a product decision, architectural change, or materially larger scope.

After Implement returns a coherent fix and verification evidence, resume the round's active reviewer for a full review of the complete current artifact within the assigned scope. Feed any findings back through the round's active auditor and reconciliation before another fix delegation, repeating this loop within the round.

## Complete rounds and reset context

Use the round as the unit of review confidence. Review and reconciliation determine the authoritative finding set; the coordinator owns round completion and all subsequent control flow. An empty authoritative finding set completes the round.

Freshness constrains a round's starting context, not the evidence it develops. Reconciliation tests findings without consuming freshness; changing the artifact entangles the sessions with work they helped shape. A round is changed after any review-driven edit, even if that edit is later reverted.

For substantial code, every changed round must be followed by another fresh-session round over the complete current scope, with no prior findings or expected verdict. Apply the same rule to every subsequent round until one completes with an empty authoritative finding set and without changing the artifact. Freshness resets accumulated context and its bias; it never narrows the review to a delta.

For non-code or non-substantial code, a round that completes with an empty authoritative finding set is sufficient even when it contained fixes.

## Judge substantial code

The coordinator decides from the actual system and its risks, never from line count or a rigid checklist. A change is substantial when its behavioral reach, coupling, or failure impact cannot be understood as a local, isolated edit.

Signals include, but are not limited to:

- behavior spanning interacting components or modules;
- changes to lifecycle, concurrency, persistence, protocols, security, or recovery;
- review fixes materially growing or reshaping the original change;
- failures reaching multiple sessions, users, or durable data; or
- a reviewer or auditor identifying a concrete need for fresh-session confirmation.

## Finish

Report the clean result, important fixes, verification performed, and residual risk, then return control to the invoker. Never create or merge a PR from this skill.
