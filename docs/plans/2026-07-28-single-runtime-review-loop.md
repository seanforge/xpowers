# Single-runtime review loop

## Problem

Xpowers currently delegates review rigor to a cross-model `interleaved-review` dependency. Running both Claude and Codex duplicates the review cost even though the active harness can supply its own native reviewer and subagents.

## Outcome

Review keeps the adversarial reconciliation while using only the active runtime. Fresh-session rounds reset accumulated context bias without narrowing review to a delta. Runtime-specific invocation and session-resume mechanics stay outside the shared reasoning model.

## Behavioral commitments

1. The active harness runs one native reviewer against the merge base, repository instructions, complete current change, and approved plan when one exists. A small hotfix may proceed without a plan when its intended behavior is clear from the user request, PR, or commits; ask the user only when the intent remains ambiguous.
2. The reviewer and finding auditor always occupy different sessions. Any reviewer result with no findings never starts or resumes an auditor. The first findings from a new reviewer round trigger a fresh auditor for that round; further findings from the same reviewer session resume the same auditor. The auditor acts as a brutal staff engineer and challenges each submitted finding's validity, materiality, cause, severity, evidence, and proposed direction without running another independent review.
3. The coordinator freezes the artifact and relays each side's complete arguments, objections, and evidence to the other until both roles have answered every unresolved point and agree on each finding's validity, materiality, cause, severity, and fix direction. Only then is the finding resolved; the coordinator applies any agreed root-cause fixes and sends the complete current change back to the same reviewer. Resume the same auditor only when that review produces findings.
4. A round begins with a fresh reviewer examining the whole current scope. It may contain repeated audit, reconciliation, coordinator-fix, and full re-review loops in the same reviewer/auditor sessions. It ends when its reviewer explicitly confirms no findings, including after accepting auditor pushback. For substantial code, a round that changed the artifact is followed by another fresh-session round over the whole current scope; continue until a round ends with no findings and without changing the artifact. A no-finding round is sufficient for other work even if it contained fixes.
5. Claude uses Claude-native subagents with reviewer prompts beginning `/code-review high`, retains their session handles, and never proactively kills reviewer or auditor sessions while the loop may resume them. Codex uses `codex review` for the reviewer and a native subagent for the auditor, captures both session handles, and resumes each role for follow-up turns.
6. A failed, timed-out, empty, delegated, or invalid response never establishes clean. Retry it, recovering the same session when reliable; otherwise replace only that role with a fresh session.

## Implementation decisions

The shared review skill owns scope, the review contract, role separation, reconciliation, fixes, round completion, and handoff to testing. It presents them as semantic invariants and judgment principles rather than a prescribed message sequence, so the coordinator can adapt the interaction without weakening the boundaries. The coordinator persists until the applicable clean outcome is established or a concrete user decision or genuine external blocker prevents continuation. It selects one runtime adapter and never starts the other runtime or invokes the cross-model review skill.

Claude and Codex mechanics live in separate, concise references. Each reference describes only how to start the reviewer, conditionally start the auditor, capture their handles, preserve them for the working loop, and resume them. A shared prompt defines the auditor's narrow finding-challenge role. Codex target selection remains discoverable through CLI help; the reference preserves only the non-obvious reviewer session-ID choreography.

Each runtime reference includes concise reviewer-prompt guidance: supply the fixed comparison point, complete review surface, repository instructions, staff-engineer expectations, and the approved plan as the primary intent source when available. For a planless hotfix, supply the explicit user, PR, or commit intent instead. Keep the shared review contract in the core skill rather than duplicating it across runtime references.

A reviewer and its finding-auditor session form a working pair after that reviewer first reports findings. The pair remains stable through every challenge, reconciliation, fix, and full re-review within that round. The reviewer alone performs each full re-review; further findings resume its paired auditor. A fresh round receives a fresh auditor only if its reviewer reports findings. The coordinator orchestrates the exchange and changes the artifact only after the reviewer and auditor agree; it never adjudicates an unresolved disagreement or substitutes its own judgment for either role. The auditor never audits a clean verdict or searches the artifact for unrelated findings.

```text
FRESH-SESSION ROUND — REVIEW THE WHOLE CURRENT SCOPE
    reviewer findings → fresh auditor → reconcile
                              ├─ upheld → coordinator fixes → same reviewer fully re-reviews
                              └─ rejected → reviewer accepts pushback
    reviewer no findings → ROUND COMPLETE

ROUND COMPLETE
    substantial code + artifact changed → ANOTHER FRESH-SESSION ROUND
    otherwise                           → STOP
```

The round is the unit of review confidence. Freshness constrains its starting context, not the evidence developed through review and reconciliation. Round completion depends on the reviewer reaching no findings; whether another round is required depends on review-driven editing and the code's risk. An auditor-rejected finding can therefore complete an unchanged round without forcing another one. Any review-driven edit makes the round changed even if that edit is later reverted.

Freshness resets context, not scope. Every new round reviews the whole current change without prior findings or an expected verdict. This guards substantial code against confidence inherited from sessions that participated in shaping its fixes, and the same rule reapplies after every changed round.

A code change is substantial when its behavioral reach, coupling, or failure impact cannot be understood as a local, isolated edit. Judge the actual system and its risks, not line count, a checklist, or enumerated cases.

Root cause is part of finding resolution, not an afterthought. Judge fixes by whether they correct the causal model and strengthen coherent invariants, not merely by whether the current finding disappears. A converging loop makes the system model more coherent while reducing risk and incidental complexity; a non-converging loop produces new findings that show fixes proliferating or relocating related problems. On non-convergence, freeze further editing and reconcile a coherent direction. Ask the user only when that direction requires a redesign, public-contract change, or materially larger refactor outside the approved scope.

## Testing decisions

Validate the skill package structure and frontmatter with the skill-authoring validator. Verify all Markdown references resolve, runtime selection is unambiguous, the cross-model dependency is absent, and reviewer/auditor session separation and fresh-session round semantics are stated consistently.

Use short probes to establish that Codex exposes and resumes a review session ID, and that a native auditor subagent exposes a reusable handle. Do not perform a substantive review as part of this validation.

## Out of scope

Session cleanup, persisted review ledgers, a workflow runtime, automatic phase chaining, cross-model review, and changes to native reviewer implementations are excluded.
