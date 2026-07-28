# Codex runtime

Use this reference only for Codex invocation and session reuse; keep review judgment in the core skill. Use the native Codex CLI in the repository. Consult `codex review --help` for the locally installed target and prompt options; keep the shared review contract in the reviewer request.

Start the reviewer with `codex review`. Capture the `session id:` printed in its startup output before collecting the result.

Resume the reviewer for every later exchange or full re-review:

```sh
codex exec resume --json <reviewer-session-id> "<follow-up prompt>"
```

When that reviewer first reports findings, use native `spawn_agent` to start a different fresh subagent with the shared finding-auditor role prompt. Capture the returned agent ID and target that same ID with `followup_task` for every later exchange in the round.

Keep reviewer and auditor IDs distinct and serialize turns within each session. Do not proactively terminate or discard the auditor while the loop may resume it. Do not start an auditor when the reviewer reports no findings.
