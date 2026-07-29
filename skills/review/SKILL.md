---
name: review
description: Coordinate whole-change review after implementation and before formal testing, using the active harness's native reviewer plus an isolated finding auditor when needed.
---

# Review

Establish a trustworthy conclusion about the complete current change before formal testing. Use this skill as a reasoning framework, not a rigid workflow: preserve its semantic boundaries while adapting the interaction to the change, evidence, and active agent runtime.

## Coordinate; do not review

Act only as coordinator. Own the review contract, session orchestration, evidence relay, authorized fixes, and stop decision. Never substitute your judgment for the reviewer or finding auditor.

Identify the agent harness currently running you, then read exactly its matching reference:

- If you are running in Claude Code, read [references/claude.md](references/claude.md).
- If you are running in Codex, read [references/codex.md](references/codex.md).

Do not ask the user to choose, read the other runtime reference, start the other runtime, or invoke a cross-model review workflow.

## Establish the review contract

Fix the intended comparison point, verify that the scope is non-empty, and include the complete change: committed, staged, unstaged, production, and applicable test code.

Use the provided plan as the primary statement of intent when available. For a small planless hotfix, use the explicit intent from the user request, PR, or commits; ask the user only when intent remains ambiguous.

Give every reviewer the complete scope, repository instructions, intent, and a pragmatic staff-engineer mandate to assess both intent alignment and engineering soundness. Treat production code and executable tests as one review surface. Ask for review only: no edits.

Require every reviewer to invoke the `skills:valuable-tests` and `skills:clean-code` SKILLs, plus the `codebase-design` SKILL when the review surface changes a module, interface, seam, adapter, or architecture. Before applying a fix, invoke `skills:clean-code` when changing production code or tests, `skills:valuable-tests` when changing tests, `codebase-design` when reshaping modules or their interfaces, and `diagnosing-bugs` when a finding's cause remains unexplained.

## Preserve the invariants

Fresh always means a new isolated session created through the active runtime's context-isolation mechanism, with no inherited coordinator, role, or prior-round conversation context. Select explicit context, repository exploration, and exchanges in proportion to the change and its risks. Freshness constrains starting context; it does not prevent resuming that session within its review round.

- Every round starts with a fresh reviewer session and reviews the complete current scope, never only the latest delta.
- A reviewer and auditor always occupy different sessions.
- An auditor belongs only to the round in which it is created and exists only after that round's reviewer reports findings.
- Reuse the active reviewer and auditor sessions throughout their round and never across rounds.
- An auditor challenges only the submitted findings; it does not independently review the change or search for new issues.
- Freeze the artifact during review, audit, and reconciliation. Change it only after reviewer and auditor agreement.
- A valid reviewer result that reports no findings never starts or resumes an auditor.
- A failed, timed-out, empty, delegated, or otherwise invalid response never counts as clean. Neither does a fix, elapsed time, or coordinator confidence.
- Never leave a disputed finding unresolved or create a parallel finding ledger.

Retry every failed, timed-out, empty, delegated, or otherwise invalid review or audit response. Prefer retrying or resuming the saved session. If that session cannot be resumed reliably, create a fresh replacement for only that role, give it the complete review contract and unresolved context, and keep the other role unchanged. The replacement inherits the current round; it does not start a fresh-session round or reset that round's state.

## Drive to completion

Persist until the applicable clean outcome is established. Do not stop at a progress update, a pending reviewer or auditor, an unresolved finding, or an applied fix. Do not ask the user whether to continue ordinary review-and-fix rounds. Wait for active work, recover or resume reusable sessions, and keep driving reconciliation, fixes, and full re-review.

The only non-clean handoff is a concrete user decision that evidence cannot settle or a genuine external blocker that remains after reasonable recovery. Persistence means owning the outcome, not defending a failing implementation direction.

## Reconcile findings

Start with a fresh native reviewer. On the round's first findings, start the active runtime's Xpowers auditor as a fresh session. Give it the findings, their reasoning and evidence, the review contract, and access to the relevant code.

Treat reconciliation as a direct, evidence-driven argument between reviewer and auditor. Relay each side's complete arguments, objections, and evidence to the other, and require both to answer every unresolved point. Adapt the number and shape of exchanges to the evidence; do not summarize away disagreement or adjudicate it yourself.

Guide both roles toward case-by-case engineering judgment. A finding may be technically true yet immaterial. What matters is whether it represents a grounded, material risk under credible real-world conditions for this system.

Reconciliation ends only when both roles agree on each finding's validity, materiality, cause, severity, and coherent fix direction. Treat the reconciled finding set as authoritative. Apply only agreed root-cause fixes, and ask the user when evidence cannot settle a product, architectural, or scope decision.

## Fix root causes, not symptoms

Treat the causal explanation as part of finding resolution. Judge a fix by whether it corrects the causal model and strengthens a coherent invariant, not merely by whether the current finding disappears.

A converging fix loop makes the system model more coherent while reducing risk and incidental complexity; persist through it to clean. A non-converging loop is revealed when new reviewer findings show fixes proliferating or relocating related problems. Treat that pattern as evidence—not automatic proof—that the causal model, design, or architecture may be wrong.

On non-convergence, freeze editing before another fix. Use `diagnosing-bugs`, return the current finding and pattern to the round's active reviewer and auditor, and reconcile a simpler coherent direction before changing code again. Ask the user only when that direction requires a product decision, architectural change, or materially larger scope.

After a coherent fix, resume the round's active reviewer for a full review of the complete current change. Feed any findings back through the round's active auditor and reconciliation before another coordinator fix, repeating this loop within the round.

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

Report the clean result, important fixes made during review, and residual risk for formal testing. Recommend invoking `xpowers:test` and ask whether to proceed; never invoke it automatically.
