# Codex runtime

Use this reference only for Codex invocation and session reuse; keep review judgment in the core skill. Use the native Codex CLI in the repository. Consult `codex review --help` for the locally installed target and prompt options; keep the shared review contract in the reviewer request.

Start the reviewer with `codex review`. Capture the `session id:` printed in its startup output before collecting the result.

If a review runs unusually long, inspect the matching `~/.codex/sessions/**/rollout-*<session-id>.jsonl` before treating it as stalled.

Resume the reviewer for every later exchange or full re-review:

```sh
codex exec resume --json <reviewer-session-id> "<follow-up prompt>"
```

On the round's first findings, use native `spawn_agent` to start a different fresh subagent with the shared finding-auditor role prompt. Capture the returned agent ID and target the active auditor ID with `followup_task` for every later exchange in the round. Replacing a failed reviewer does not replace an existing auditor.

Keep reviewer and auditor IDs distinct and serialize turns within each session. Do not proactively terminate or discard the auditor while the loop may resume it. Do not start an auditor for a valid no-findings reviewer response.
