# Single-runtime review loop

## Problem

Xpowers currently delegates review rigor to a cross-model `interleaved-review` dependency. Running both Claude and Codex duplicates the review cost even though the active harness can supply its own native reviewer and subagents.

## Outcome

Review keeps the adversarial interleaving and clean-room gates while using only the active runtime. Runtime-specific invocation and session-resume mechanics stay outside the shared workflow.

## Behavioral commitments

1. The active harness runs one native reviewer against the merge base, repository instructions, complete current change, and approved plan when one exists. A small hotfix may proceed without a plan when its intended behavior is clear from the user request, PR, or commits; ask the user only when the intent remains ambiguous.
2. The reviewer and finding auditor always occupy different sessions. Any reviewer result with no findings never starts or resumes an auditor. The first findings from a new reviewer round trigger a fresh auditor for that round; further findings from the same reviewer session resume the same auditor. The auditor acts as a brutal staff engineer and challenges each submitted finding's validity, materiality, cause, severity, evidence, and proposed direction without running another independent review.
3. The coordinator freezes the artifact and relays complete evidence between the reviewer and auditor for as many rounds as needed until they reconcile each finding's validity, materiality, cause, severity, and fix direction with each other. Only then does the coordinator apply the agreed root-cause fixes and send the complete current change back to the same reviewer. Resume the same auditor only when that review produces findings.
4. Any fresh reviewer that finds nothing passes without starting an auditor. After a working pair reaches clean, only a substantial code change requires another fresh reviewer. If it reports findings, start a fresh auditor; those two sessions become the new working pair and remain in use through every resulting challenge, reconciliation, fix, and full re-review turn. Once they reach clean, repeat the fresh gate until a fresh reviewer finds nothing. Non-code changes never require this final fresh gate.
5. Claude uses Claude-native subagents with reviewer prompts beginning `/code-review high`, retains their session handles, and never proactively kills reviewer or auditor sessions while the loop may resume them. Codex uses `codex review` for the reviewer and a native subagent for the auditor, captures both session handles, and resumes each role for follow-up turns.
6. A failed, timed-out, empty, or invalid response never opens a gate. Recover the same session when reliable; otherwise replace only that role with a fresh session.

## Implementation decisions

The shared review skill owns scope, the review contract, role separation, reconciliation, fixes, clean gates, and handoff to testing. It selects one runtime adapter and never starts the other runtime or invokes the cross-model review skill.

Claude and Codex mechanics live in separate, concise references. Each reference describes only how to start the reviewer, conditionally start the auditor, capture their handles, preserve them for the working loop, and resume them. A shared prompt defines the auditor's narrow finding-challenge role. Codex target selection remains discoverable through CLI help; the reference preserves only the non-obvious reviewer session-ID choreography.

Each runtime reference includes concise reviewer-prompt guidance: supply the fixed comparison point, complete review surface, repository instructions, staff-engineer expectations, and the approved plan as the primary intent source when available. For a planless hotfix, supply the explicit user, PR, or commit intent instead. Keep the shared review contract in the core skill rather than duplicating it across runtime references.

A reviewer and its finding-auditor session form a working pair after that reviewer first reports findings. The pair remains stable through multi-round challenge, reconciliation, fixes, and full re-review. The reviewer alone performs each full re-review; further findings resume its paired auditor. A fresh reviewer round receives a fresh auditor only if it reports findings. The coordinator orchestrates the exchange and changes the artifact only after the reviewer and auditor agree; it never adjudicates an unresolved disagreement or substitutes its own judgment for either role. The auditor never audits a clean verdict or searches the artifact for unrelated findings.

```text
FRESH REVIEWER
    ├─ no findings ─────────────────────────────────────► STOP
    └─ findings → FRESH AUDITOR → WORKING PAIR
                                      │
                                      ▼
       audit ↔ push back → REVIEWER + AUDITOR reconcile → COORDINATOR fixes
                                      │
                                      ▼
                         SAME REVIEWER reviews the FULL change
                                      │
                                      ├─ findings ──────► SAME AUDITOR → continue loop
                                      └─ no findings
                                           ├─ non-code or non-substantial code ─► STOP
                                           └─ substantial code ─────────────────► FRESH REVIEWER
```

The initial reviewer is already fresh, so an initially clean review always stops. The final fresh gate applies only after a working pair has fixed findings and reached a clean same-session re-review.

After any reviewer returns no findings, never run the auditor. Then decide whether another fresh-session round is required: start a fresh reviewer only when a substantial code change has just reached clean in its working sessions; otherwise stop. The fresh reviewer starts a fresh auditor only if it reports findings, completing the new working pair.

Treat a code change as substantial when multiple modules interact; lifecycle, concurrency, persistence, protocol, security, or recovery behavior changes; review fixes materially grow or reshape the code diff; a defect could affect multiple sessions, users, or stored data; or either review role requests clean-room confirmation. These are judgment signals, not a checklist or line-count threshold, and they do not apply to non-code artifacts.

Do not stack compensating patches to satisfy findings. If the coherent fix requires a redesign, public-contract change, or materially larger refactor outside the approved scope, freeze the diff and ask the user.

## Testing decisions

Validate the skill package structure and frontmatter with the skill-authoring validator. Verify all Markdown references resolve, runtime selection is unambiguous, the cross-model dependency is absent, and reviewer/auditor session separation and fresh-reviewer gates are stated consistently.

Use short probes to establish that Codex exposes and resumes a review session ID, and that a native auditor subagent exposes a reusable handle. Do not perform a substantive review as part of this validation.

## Out of scope

Session cleanup, persisted review ledgers, a workflow runtime, automatic phase chaining, cross-model review, and changes to native reviewer implementations are excluded.
