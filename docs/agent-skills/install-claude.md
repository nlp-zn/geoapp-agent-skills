# Install Geo Agent Skills For Claude Code

Geo agent skills teach Claude Code how to operate Geo App through the `geo`
CLI. Install them when you want Claude to perform prompt operations, run
analysis, reporting, competitor/citation analysis, or server-side skill
execution.

## Prerequisites

If `geo` is not found, install the published beta CLI first:

```bash
npm install -g geoapp-cli@beta
geo --version
```

The npm package is `geoapp-cli`, not `@geoapp/cli`; the executable is `geo`.
For hosted Geo App, the CLI defaults to `https://api.prompt-insights.com`.

```bash
geo doctor
geo auth status
geo brand current
```

If auth is missing:

```bash
geo auth login
```

If brand context is missing:

```bash
geo brand list
geo brand use <brand_id>
```

## Current Beta Skills Source

`geoapp-cli@beta` does not yet bundle the agent skill files in the npm tarball.
For beta testing, point the CLI at the public skills checkout before running
`geo agent-skills install`:

```bash
git clone https://github.com/nlp-zn/geoapp-agent-skills.git ~/geoapp-agent-skills
export GEO_AGENT_SKILLS_SOURCE_DIR=~/geoapp-agent-skills/skills
```

If the checkout already exists:

```bash
git -C ~/geoapp-agent-skills pull --ff-only origin main
export GEO_AGENT_SKILLS_SOURCE_DIR=~/geoapp-agent-skills/skills
```

## Install

```bash
geo agent-skills install --agent claude
geo agent-skills doctor --agent claude
```

Default install path:

```text
~/.claude/skills
```

For tests or custom setups:

```bash
GEO_AGENT_SKILLS_CLAUDE_DIR=/path/to/skills geo agent-skills install --agent claude
```

## Verify

```bash
geo agent-skills list
geo agent-skills doctor --agent claude
```

Restart Claude Code after installation so it reloads the skill list.

## First Useful Prompt

```text
Use the Geo skills. Check setup first, generate pricing and alternatives
prompts, preview before applying them, batch-run the Price prompts after I
approve, then analyze the results.
```

## Recovery

| Symptom | Fix |
| --- | --- |
| Claude does not see the skills | Re-run `geo agent-skills doctor --agent claude` and restart Claude Code. |
| `geo` is not found | Run `npm install -g geoapp-cli@beta`, then ensure npm's global bin directory is on `PATH`. |
| Auth required | Run `geo auth login` or set a scoped PAT with `geo auth token set`. |
| Missing brand | Run `geo brand list`, then `geo brand use <brand_id>`. |
| Permission denied | Check PAT scopes and brand allowlist. |
