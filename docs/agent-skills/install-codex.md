# Install Geo Agent Skills For Codex

Install the Geo App agent skills into Codex:

```bash
geo agent-skills install --agent codex
geo agent-skills doctor --agent codex
```

Default install path:

```text
~/.codex/skills
```

For tests or custom setups, override the path:

```bash
GEO_AGENT_SKILLS_CODEX_DIR=/path/to/skills geo agent-skills install --agent codex
```

After installation, ask Codex to use one of:

- `$geo-shared`
- `$geo-prompt-ops`
- `$geo-run-analysis`
- `$geo-reporting`
- `$geo-skill-execution`
- `$geo-competitor-citation-analysis`

Run `geo doctor` before real work so the agent can recover from auth, brand,
scope, and server compatibility issues.
