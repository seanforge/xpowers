---
name: review
description: Coordinate whole-change review after implementation and before formal testing, using the active harness's native reviewer plus an isolated finding auditor when needed.
---

# Review

Establish the correctness of the complete current change before formal test execution.

## Act only as coordinator

Never serve as the reviewer or finding auditor. Own the review contract, session orchestration, evidence relay, authorized fixes, state transitions, and stop decision without substituting your judgment for either review role.

Use only the active runtime:

- In Claude Code, read [references/claude.md](references/claude.md).
- In Codex, read [references/codex.md](references/codex.md).

Never start the other runtime or invoke a cross-model review workflow.

## Establish the review contract

Resolve the intended merge base and verify that the whole review scope is non-empty. Include committed, staged, unstaged, and applicable test changes.

Use the approved plan as the primary statement of intent when available. A small hotfix may proceed without one when its intended behavior is clear from the user request, PR, or commits; ask the user only when intent remains ambiguous.

Give every reviewer:

- the fixed comparison point and complete current change;
- applicable repository instructions and the plan or hotfix intent;
- a pragmatic staff-engineer perspective;
- intent-alignment and engineering-soundness criteria; and
- production code and executable tests as one review surface.

Ask for native review only: no edits, no coordination, no auditor creation, and no coordinating review skill.

Use `skills:valuable-tests` and `skills:clean-code` to form the review criteria. Invoke `skills:clean-code` again before applying a production-code fix. Invoke `diagnosing-bugs` when a finding's cause remains unexplained.

## Run review rounds

Treat each review round as one reviewer session with an optional finding-auditor session that exists only after that reviewer reports findings.

Start a fresh native reviewer using the selected runtime reference and preserve its session handle. Freeze the artifact during every review, audit, and reconciliation turn; change it only after reviewer and auditor agreement.

If the reviewer reports no findings, never start or resume an auditor. The round is clean.

If the reviewer reports findings:

1. Start a fresh auditor for that reviewer round, or resume its existing auditor.
2. Read [references/finding-auditor.md](references/finding-auditor.md), use it as the auditor's role prompt, and give the auditor every finding, its reason and evidence, the review contract, and access to relevant code.
3. Require the auditor to challenge only the submitted findings. It must not run an independent review or search for unrelated findings.
4. Ask for audit only: no edits, no coordination, and no additional reviewer creation.
5. Relay each side's complete arguments, objections, and evidence to the other, and require each role to answer the other's unresolved points. Continue for as many turns as needed without summarizing away disagreement. Preserve lightweight finding IDs in conversation; create no ledger.
6. Wait until reviewer and auditor agree on what is valid and material, and on the coherent fix direction. Do not adjudicate their unresolved disagreement.

If reviewer and auditor agree that no finding holds up, require the reviewer to confirm the clean result and apply the clean gate without changing the artifact.

If evidence cannot settle a product, architectural, or scope decision, ask the user. Otherwise apply each agreed root-cause fix. Do not stack compensating patches merely to make findings disappear.

After a fix, resume the same reviewer and require a full review of the complete current change, not only the latest delta. If it reports findings, resume the same auditor and continue the exchange. If it reports none, do not call the auditor.

## Apply the clean gate

Treat the initial reviewer as already fresh. If it reports no findings, stop.

When a working reviewer returns clean after reconciliation or fixes:

- Stop for non-code or non-substantial code changes.
- For substantial code changes, start a new round with a fresh reviewer and no prior findings or expected verdict.

If that fresh reviewer reports findings, start a fresh auditor for the new round; those sessions become the new working pair. Repeat until a fresh reviewer reports no findings.

Treat a code change as substantial when multiple modules interact; lifecycle, concurrency, persistence, protocol, security, or recovery behavior changes; review fixes materially grow or reshape the code diff; a defect could affect multiple sessions, users, or stored data; or either review role requests clean-room confirmation. Use judgment rather than a line-count threshold. Non-code artifacts never require the final fresh gate.

A timeout, failed or empty result, invalid delegation, fix, or elapsed time never counts as a clean reviewer result. Once an auditor exists, neither role may leave a disputed finding unresolved. Recover the same session when reliable; otherwise replace only that role with a fresh session.

## Finish

Report the clean result, important fixes made during review, and residual risk for formal testing. Recommend invoking `xpowers:test` and ask whether to proceed; never invoke it automatically.
