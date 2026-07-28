# Single-runtime review loop

## Problem

Xpowers currently delegates review rigor to a cross-model `interleaved-review` dependency. Running both Claude and Codex duplicates the review cost even though the active harness can supply its own native reviewer and subagents.

## Outcome

Review keeps the adversarial reconciliation while using only the active runtime. Fresh-session rounds reset accumulated context bias without narrowing review to a delta. Runtime-specific invocation and session-resume mechanics stay outside the shared reasoning model.

## Behavioral commitments

1. The active harness runs one native reviewer against the merge base, repository instructions, complete current change, and approved plan when one exists. A small hotfix may proceed without a plan when its intended behavior is clear from the user request, PR, or commits; ask the user only when the intent remains ambiguous.
2. The reviewer and finding auditor always occupy different sessions, and neither session crosses round boundaries. Any valid reviewer response reporting no findings never starts or resumes an auditor. The first findings from a new reviewer round trigger a fresh auditor for that round; further findings in the round resume that auditor. The auditor acts as a brutal staff engineer and challenges each submitted finding's validity, materiality, cause, severity, evidence, and proposed direction without running another independent review.
3. The coordinator freezes the artifact and relays each side's complete arguments, objections, and evidence to the other until both roles have answered every unresolved point and agree on each finding's validity, materiality, cause, severity, and fix direction. Their reconciled finding set is authoritative; the coordinator owns its control-flow consequences, applies any agreed root-cause fixes, and sends the complete current change back to the round's active reviewer. Resume the round's active auditor only when that review produces findings.
4. A round begins with a fresh reviewer examining the whole current scope. It may contain repeated audit, reconciliation, coordinator-fix, and full re-review loops in its active reviewer/auditor sessions, subject only to role-specific failure replacement. An empty authoritative finding set completes the round under coordinator control. For substantial code, a round that changed the artifact is followed by another fresh-session round over the whole current scope; continue until a round completes with an empty authoritative finding set and without changing the artifact. For non-code or non-substantial code, an empty authoritative finding set completes review even if the round contained fixes.
5. Claude uses Claude-native subagents with reviewer prompts beginning `/code-review high` and receives the finding auditor directly from the Xpowers plugin. Codex uses `codex review` for the reviewer and a globally installed Xpowers custom agent for the auditor. Both runtimes capture the reviewer and auditor handles, resume each role for follow-up turns, and never proactively kill sessions while the loop may resume them.
6. A failed, timed-out, empty, delegated, or otherwise invalid response never establishes clean. Retry it, recovering the same session when reliable; otherwise replace only that role with a fresh session. The replacement inherits the current round's context and state rather than creating a fresh-session round.

## Implementation decisions

The shared review skill owns scope, the review contract, role separation, reconciliation, fixes, round completion, and handoff to testing. It presents them as semantic invariants and judgment principles rather than a prescribed message sequence, so the coordinator can adapt the interaction without weakening the boundaries. The coordinator persists until the applicable clean outcome is established or a concrete user decision or genuine external blocker prevents continuation. It selects one runtime adapter and never starts the other runtime or invokes the cross-model review skill.

Claude and Codex mechanics live in separate, concise references. Each reference describes only how to start the reviewer, conditionally start the runtime-native Xpowers auditor, capture their handles, preserve them for the working loop, and resume them. The Claude plugin agent and Codex custom-agent template carry identical authored auditor-role instructions. Codex target selection remains discoverable through CLI help; the reference preserves only the non-obvious reviewer session-ID choreography.

A Codex-only setup skill copies the bundled custom-agent template exactly to `~/.codex/agents/xpowers-auditor.toml`, replacing only that Xpowers-managed file, then requires a Codex restart. Claude loads its auditor directly from the plugin and does not expose the setup skill.

Each runtime reference includes concise reviewer-prompt guidance: supply the fixed comparison point, complete review surface, repository instructions, staff-engineer expectations, and the approved plan as the primary intent source when available. For a planless hotfix, supply the explicit user, PR, or commit intent instead. Keep the shared review contract in the core skill rather than duplicating it across runtime references.

Within a round, its active reviewer and—once findings exist—auditor form the working pair. Preserve that pair through every challenge, reconciliation, fix, and full re-review unless recovery replaces only a failed role; a replacement inherits the round, and no session crosses into another round. The reviewer alone performs each full re-review; its findings return through the round's auditor and reconciliation before another coordinator fix. A fresh round receives a fresh auditor only if it reports findings. The coordinator orchestrates the exchange and changes the artifact only after the reviewer and auditor agree; it never adjudicates an unresolved disagreement or substitutes its own judgment for either role. The auditor never audits a clean verdict or searches the artifact for unrelated findings.

The round is the unit of review confidence. Freshness constrains its starting context, not the evidence developed through review and reconciliation. Review and reconciliation determine the authoritative finding set; the coordinator owns round completion and subsequent control flow. Review-driven editing and the code's risk determine whether a completed round requires another fresh round. Any review-driven edit makes the round changed even if that edit is later reverted.

Freshness resets context, not scope. Every new round reviews the whole current change without prior findings or an expected verdict. This guards substantial code against confidence inherited from sessions that participated in shaping its fixes, and the same rule reapplies after every changed round.

A code change is substantial when its behavioral reach, coupling, or failure impact cannot be understood as a local, isolated edit. The coordinator decides from the actual system and its risks, not line count or a rigid checklist. Signals include, but are not limited to, interacting components; lifecycle, concurrency, persistence, protocol, security, or recovery behavior; review fixes that materially reshape the change; failures reaching multiple sessions, users, or durable data; and a review role identifying a concrete need for fresh-session confirmation.

Root cause is part of finding resolution, not an afterthought. Judge fixes by whether they correct the causal model and strengthen coherent invariants, not merely by whether the current finding disappears. A converging loop makes the system model more coherent while reducing risk and incidental complexity; a non-converging loop produces new findings that show fixes proliferating or relocating related problems. On non-convergence, freeze further editing and reconcile a coherent direction. Ask the user only when that direction requires a redesign, public-contract change, or materially larger refactor outside the approved scope.

## Testing decisions

Validate the skill package structure and frontmatter with the skill-authoring validator. Verify all Markdown references resolve, runtime selection is unambiguous, the cross-model dependency is absent, reviewer/auditor session separation and fresh-session round semantics are stated consistently, and the Claude and Codex auditor-role instructions are identical.

Use short probes to establish that Codex exposes and resumes a review session ID, the Claude plugin agent is callable, the Codex setup installs a loadable global custom agent, and each auditor exposes a reusable handle. Do not perform a substantive review as part of this validation.

## Out of scope

Session cleanup, persisted review ledgers, a workflow runtime, automatic phase chaining, cross-model review, and changes to native reviewer implementations are excluded.
