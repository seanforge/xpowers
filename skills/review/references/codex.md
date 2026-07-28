# Codex runtime

Use this reference only for Codex invocation and session reuse; keep review judgment in the core skill. Use the native Codex CLI in the repository. Consult `codex review --help` for the locally installed target and prompt options; keep the shared review contract in the reviewer request.

Start the reviewer with `codex review`. Capture the `session id:` printed in its startup output before collecting the result.

If a review runs unusually long, inspect the matching `~/.codex/sessions/**/rollout-*<session-id>.jsonl` before treating it as stalled.

Resume the reviewer for every later exchange or full re-review:

```sh
codex exec resume --json <reviewer-session-id> "<follow-up prompt>"
```

On the round's first findings, use native `spawn_agent` with `agent_type: "xpowers-auditor"` and `fork_turns: "none"` to start a different fresh session. Pass the submitted findings, their reasoning and evidence, the review contract, and the relevant code context. Capture the returned agent ID and target the active auditor ID with `followup_task` for every later exchange in the round. Replacing a failed reviewer does not replace an existing auditor.

If `xpowers-auditor` is unavailable, ask the user to invoke `xpowers:setup` and restart Codex. Do not silently substitute a generic agent.

Keep reviewer and auditor IDs distinct and serialize turns within each session. Do not proactively terminate or discard the auditor while the loop may resume it. Do not start an auditor for a valid no-findings reviewer response.
