# Codex runtime

Use the native Codex CLI in the repository. Consult `codex review --help` for the locally installed target and prompt options; keep the shared review contract in the reviewer request.

Start the reviewer with `codex review`. Capture the `session id:` printed in its startup output before collecting the result.

Start the finding auditor only when that reviewer first reports findings:

```sh
codex exec --json "<finding-auditor prompt>"
```

Capture `thread_id` from the `thread.started` JSON event.

Resume either session for every later exchange or re-review:

```sh
codex exec resume --json <session-id> "<follow-up prompt>"
```

Keep reviewer and auditor IDs distinct and serialize turns within each session. Do not start an auditor when the reviewer reports no findings.
