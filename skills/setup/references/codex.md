# Codex setup

Install the bundled [auditor template](../assets/xpowers-auditor.toml) globally:

1. Resolve the user's home directory and ensure `.codex/agents/` exists beneath it.
2. Replace only `.codex/agents/xpowers-auditor.toml` with the exact template contents. Do not modify any other agent file.
3. Verify that the installed file is byte-for-byte identical to the template and parses as TOML.
4. Tell the user to restart Codex when the auditor was created or changed.
