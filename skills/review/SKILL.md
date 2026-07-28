---
name: review
description: Coordinate whole-change review after implementation and before formal testing, using the active harness's native reviewer plus an isolated finding auditor when needed.
---

# Review

Establish a trustworthy conclusion about the complete current change before formal testing. Use this skill as a reasoning framework, not a rigid workflow: preserve its semantic boundaries while adapting the interaction to the change, evidence, and active agent runtime.

## Coordinate; do not review

Act only as coordinator. Own the review contract, session orchestration, evidence relay, authorized fixes, and stop decision. Never substitute your judgment for the reviewer or finding auditor.

Use only the active runtime and read its reference:

- Claude Code: [references/claude.md](references/claude.md)
- Codex: [references/codex.md](references/codex.md)

Never start the other runtime or invoke a cross-model review workflow.

## Establish the review contract

Fix the intended comparison point and include the complete change: committed, staged, unstaged, production, and applicable test code.

Use the approved plan as the primary statement of intent when available. For a small planless hotfix, use the explicit intent from the user request, PR, or commits; ask the user only when intent remains ambiguous.

Give the reviewer the complete scope, repository instructions, intent, and a pragmatic staff-engineer mandate to assess both intent alignment and engineering soundness. Treat production code and executable tests as one review surface. Ask for native review only: no edits, coordination, auditor creation, or coordinating review skill.

Use `skills:valuable-tests` and `skills:clean-code` to shape review criteria. Invoke `skills:clean-code` again before changing production code, and `diagnosing-bugs` when a finding's cause remains unexplained.

## Preserve the invariants

- A reviewer and auditor always occupy different sessions.
- A reviewer defines one round. Its auditor exists only if that reviewer reports findings, and stays paired with it throughout that round.
- Preserve both session handles and reuse the same pair while the round continues.
- Freeze the artifact during review, audit, and reconciliation. Change it only after reviewer and auditor agreement.
- A reviewer result with no findings never starts or resumes an auditor.
- A failed, timed-out, empty, or invalid result never counts as clean.
- Never leave a disputed finding unresolved or create a parallel finding ledger.

Recover a reusable session when reliable. If recovery is impossible, replace only that role and preserve the separation.

## Reconcile findings

Start with a fresh native reviewer. If it reports findings, read [references/finding-auditor.md](references/finding-auditor.md) and start a fresh auditor for that round. Give the auditor the findings, their reasoning and evidence, the review contract, and access to the relevant code.

Treat reconciliation as a direct, evidence-driven argument between reviewer and auditor. Relay each side's complete arguments, objections, and evidence to the other, and require both to answer every unresolved point. Adapt the number and shape of exchanges to the evidence; do not summarize away disagreement or adjudicate it yourself.

Guide both roles toward case-by-case engineering judgment. A finding may be technically true yet immaterial. What matters is whether it represents a grounded, material risk under credible real-world conditions for this system.

Reconciliation ends only when both roles agree on each finding's validity, materiality, cause, severity, and coherent fix direction:

- If no finding holds up, require the reviewer to accept the pushback and explicitly confirm no findings.
- If findings hold up, apply only the agreed root-cause fixes. Ask the user when evidence cannot settle a product, architectural, or scope decision.

Do not stack compensating patches merely to silence findings. After a fix, resume the same reviewer and ask for a full review of the complete current change. Resume its auditor only if that review produces findings.

## Require independent clean confirmation

The initial reviewer is already fresh. If it initially reports no findings, stop.

When a working reviewer confirms clean after reconciliation or fixes, stop for non-code or non-substantial code changes. For substantial code changes, always require a final fresh-reviewer round with no prior findings or expected verdict. This gate counters anchoring and overconfidence in the working sessions; same-session confidence is not independent confirmation.

If the fresh reviewer reports findings, create its fresh auditor and treat them as the new working pair. Continue until a fresh reviewer reports no findings.

Judge substantial code by risk and interaction, not line count. Signals include changes across modules; lifecycle, concurrency, persistence, protocol, security, or recovery behavior; review fixes that materially reshape the diff; defects that could affect multiple sessions, users, or stored data; or a review role requesting clean-room confirmation. Non-code artifacts never require the final fresh gate.

## Finish

Report the clean result, important fixes made during review, and residual risk for formal testing. Recommend invoking `xpowers:test` and ask whether to proceed; never invoke it automatically.
