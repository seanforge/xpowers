# Claude Code runtime

Use Claude Code's native subagent tools. Keep reviewer and auditor in different sessions.

Start every reviewer in a fresh subagent with a prompt beginning exactly:

```text
/code-review high

<review contract>
```

Capture the returned session handle. When that reviewer first reports findings, start a different fresh native subagent with the shared finding-auditor role prompt and capture its handle. Resume those same sessions for every later exchange and re-review in the round.

Do not proactively kill, terminate, or discard reviewer or auditor sessions while the review loop may resume them. Do not start an auditor when the reviewer reports no findings.
