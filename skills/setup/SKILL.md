---
name: setup
description: Install or refresh the Xpowers native finding auditor for Codex. Use after installing or updating the Xpowers plugin in Codex, or when Codex cannot find the Xpowers auditor.
---

# Set up Xpowers for Codex

Install the bundled [auditor template](assets/xpowers-auditor.toml) globally:

1. Resolve the user's home directory and ensure `.codex/agents/` exists beneath it.
2. Replace only `.codex/agents/xpowers-auditor.toml` with the exact template contents. Do not merge, preserve, or modify any other agent file.
3. Verify that the installed file is byte-for-byte identical to the template and parses as TOML.
4. Tell the user to restart Codex before invoking `xpowers:review`; the current session cannot load a newly installed custom agent.

Do not configure model, reasoning effort, tools, MCP servers, skills, or sandbox policy. The auditor inherits Codex's available capabilities; its role prompt prohibits edits and unrelated review work.
