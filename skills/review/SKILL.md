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

Use the approved plan as the primary statement of intent when available. For a small planless hotfix, use the explicit intent from the user request, PR, or commits; ask the user only when intent remains ambiguous.

Give every reviewer the complete scope, repository instructions, intent, and a pragmatic staff-engineer mandate to assess both intent alignment and engineering soundness. Treat production code and executable tests as one review surface. Ask for review only: no edits.

Use `skills:valuable-tests`, `skills:clean-code`, and `codebase-design` to shape every reviewer's criteria. Invoke `skills:clean-code` again before changing production code, `codebase-design` when a fix reshapes modules or their interfaces, and `diagnosing-bugs` when a finding's cause remains unexplained.

## Preserve the invariants

- A reviewer and auditor always occupy different sessions.
- A reviewer defines one round. Its auditor exists only if that reviewer reports findings, and stays paired with it throughout that round.
- Preserve both session handles and reuse the same pair while the round continues.
- An auditor challenges only the submitted findings; it does not independently review the change or search for new issues.
- Freeze the artifact during review, audit, and reconciliation. Change it only after reviewer and auditor agreement.
- A reviewer result with no findings never starts or resumes an auditor.
- A failed, timed-out, empty, delegated, or invalid result never counts as clean. Neither does a fix, elapsed time, or coordinator confidence.
- Never leave a disputed finding unresolved or create a parallel finding ledger.

After a transient call failure, resume the saved reviewer or auditor session. If that session cannot be resumed reliably, create a fresh replacement for only that role, give it the complete review contract and unresolved context, and keep the other role unchanged.

## Drive to completion

Persist until the applicable clean gate passes. Do not stop at a progress update, a pending reviewer or auditor, an unresolved finding, or an applied fix. Do not ask the user whether to continue ordinary review-and-fix rounds. Wait for active work, recover or resume reusable sessions, and keep driving reconciliation, fixes, and full re-review.

The only non-clean handoff is a concrete user decision that evidence cannot settle or a genuine external blocker that remains after reasonable recovery. Persistence means owning the outcome, not defending a failing implementation direction.

## Reconcile findings

Start with a fresh native reviewer. If it reports findings, read [references/finding-auditor.md](references/finding-auditor.md) and start a fresh auditor for that round. Give the auditor the findings, their reasoning and evidence, the review contract, and access to the relevant code.

Treat reconciliation as a direct, evidence-driven argument between reviewer and auditor. Relay each side's complete arguments, objections, and evidence to the other, and require both to answer every unresolved point. Adapt the number and shape of exchanges to the evidence; do not summarize away disagreement or adjudicate it yourself.

Guide both roles toward case-by-case engineering judgment. A finding may be technically true yet immaterial. What matters is whether it represents a grounded, material risk under credible real-world conditions for this system.

Reconciliation ends only when both roles agree on each finding's validity, materiality, cause, severity, and coherent fix direction:

- If no finding holds up, require the reviewer to accept the pushback and explicitly confirm no findings.
- If findings hold up, you—the coordinator—apply only the agreed root-cause fixes. Ask the user when evidence cannot settle a product, architectural, or scope decision.

## Fix root causes, not symptoms

Treat the causal explanation as part of finding resolution. Do not accept a fix direction merely because it suppresses the reported symptom; require a coherent account of the violated invariant and why the fix addresses it.

Persist through a converging fix loop; interrupt a diverging patch loop. Continue coherent root-cause fixes and full re-reviews while risk and complexity converge toward clean. If fixes instead keep surfacing related findings, adding special cases, moving the defect, or materially growing the diff, freeze further editing. Treat the pattern as evidence—not automatic proof—that the design, architecture, or assumed root cause may be wrong. Use `diagnosing-bugs`, return the pattern to the same reviewer and auditor, and reconcile a simpler coherent direction before changing code again. Ask the user if that direction requires a product decision, architectural change, or materially larger scope.

After a coherent fix, resume the same reviewer and ask for a full review of the complete current change. Resume its auditor only if that review produces findings.

## Require independent clean confirmation

The initial reviewer is already fresh. If it initially reports no findings, stop.

When a working reviewer confirms clean after reconciliation or fixes, stop for non-code or non-substantial code changes. For substantial code changes, always require a final fresh-reviewer round with no prior findings or expected verdict. This gate counters anchoring and overconfidence in the working sessions; same-session confidence is not independent confirmation.

If the fresh reviewer reports findings, create its fresh auditor and treat them as the new working pair. Continue until a fresh reviewer reports no findings.

Judge substantial code by risk and interaction, not line count. Signals include changes across modules; lifecycle, concurrency, persistence, protocol, security, or recovery behavior; review fixes that materially reshape the diff; defects that could affect multiple sessions, users, or stored data; or a review role requesting clean-room confirmation. Non-code artifacts never require the final fresh gate.

## Finish

Report the clean result, important fixes made during review, and residual risk for formal testing. Recommend invoking `xpowers:test` and ask whether to proceed; never invoke it automatically.
