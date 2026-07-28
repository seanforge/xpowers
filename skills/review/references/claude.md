# Claude Code runtime

Use this reference only for Claude Code invocation and session reuse; keep review judgment in the core skill. Use Claude Code's native subagent tools and keep reviewer and auditor in different sessions.

Start every reviewer in a fresh subagent with a prompt beginning exactly:

```text
/code-review high

<review contract>
```

Capture the returned session handle. On the round's first findings, start the plugin-native `xpowers:auditor` in a different fresh session and capture its handle. Pass the submitted findings, their reasoning and evidence, the review contract, and the relevant code context. Resume the active reviewer and auditor handles for every later exchange and re-review in the round. Replacing a failed reviewer does not replace an existing auditor.

Do not proactively kill, terminate, or discard reviewer or auditor sessions while the review loop may resume them. Do not start an auditor for a valid no-findings reviewer response.
