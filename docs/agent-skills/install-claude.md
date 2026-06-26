# Install Geo Agent Skills For Claude Code

Install the Geo App agent skills into Claude Code:

```bash
geo agent-skills install --agent claude
geo agent-skills doctor --agent claude
```

Default install path:

```text
~/.claude/skills
```

For tests or custom setups, override the path:

```bash
GEO_AGENT_SKILLS_CLAUDE_DIR=/path/to/skills geo agent-skills install --agent claude
```

The installed skills are regular `SKILL.md` directories. Use them with Claude
Code when asking for Geo App prompt operations, run analysis, reporting, server
skill execution, or competitor/citation analysis.
