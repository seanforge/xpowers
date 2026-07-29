# Codex runtime

Use this reference only for Codex invocation and session reuse; keep review judgment in the core skill. Use the native Codex CLI in the repository.

`codex review` accepts exactly one target: `--base`, `--commit`, `--uncommitted`, or a positional custom prompt. The CLI rejects a target flag combined with a custom prompt. Resolve the comparison point first and pass the core review contract in a raw custom prompt:

```sh
codex review '<review range, such as changes against a branch or commit, one commit, uncommitted changes, or the complete mixed current scope> <review contract from the core skill>'
```

Express the exact range and contract needed for the current review. Do not add `--base`, `--commit`, or `--uncommitted` to a custom-prompt invocation. If the installed CLI rejects this invocation, consult `codex review --help`, adapt and verify a form that preserves the exact range and core review contract, then retry. Capture the `session id:` printed in its startup output before collecting the result.

If a review runs unusually long, inspect the matching `~/.codex/sessions/**/rollout-*<session-id>.jsonl` before treating it as stalled.

Resume the reviewer for every later exchange or full re-review:

```sh
codex exec resume --json <reviewer-session-id> '<follow-up prompt>'
```

Use that same reviewer session ID both for reviewer-auditor reconciliation exchanges and for complete re-review after fixes within the round.

On the round's first findings, use native `spawn_agent` with `agent_type: "xpowers-auditor"` and `fork_turns: "none"` to start a different fresh session. Pass the submitted findings, their reasoning and evidence, the review contract, and the relevant code context. Capture the returned agent ID and target the active auditor ID with `followup_task` for every later exchange in the round. Replacing a failed reviewer does not replace an existing auditor.

If `xpowers-auditor` is unavailable, ask the user to invoke `xpowers:setup` and restart Codex. Do not silently substitute a generic agent.

Keep reviewer and auditor IDs distinct and serialize turns within each session. Do not proactively terminate or discard the auditor while the loop may resume it. Do not start an auditor for a valid no-findings reviewer response.
