# Install Geo Agent Skills For Codex

Geo agent skills teach Codex how to operate Geo App through the `geo` CLI.
Install them when you want Codex to generate prompts, run prompt batches,
analyze run exports, create reports, inspect competitors/citations, or execute
server-side Geo skills.

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

If auth is missing, run:

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
geo agent-skills install --agent codex
geo agent-skills doctor --agent codex
```

Default install path:

```text
~/.codex/skills
```

For tests or custom setups:

```bash
GEO_AGENT_SKILLS_CODEX_DIR=/path/to/skills geo agent-skills install --agent codex
```

## Verify

```bash
geo agent-skills list
geo agent-skills doctor --agent codex
```

Then ask Codex to use one of:

- `$geo-shared`
- `$geo-prompt-ops`
- `$geo-run-analysis`
- `$geo-reporting`
- `$geo-skill-execution`
- `$geo-competitor-citation-analysis`

## First Useful Prompt

```text
Use the Geo skills. Check geo doctor/auth/brand, export the last 7 days of
prompt runs with responses, citations, mentions, metrics, and errors, then
summarize the top findings and save a report in Geo App.
```

## Recovery

| Symptom | Fix |
| --- | --- |
| Codex does not see the skills | Re-run `geo agent-skills doctor --agent codex` and restart Codex. |
| `geo` is not found | Run `npm install -g geoapp-cli@beta`, then ensure npm's global bin directory is on `PATH`. |
| Auth required | Run `geo auth login` or set a scoped PAT with `geo auth token set`. |
| Missing brand | Run `geo brand list`, then `geo brand use <brand_id>`. |
| Permission denied | Check PAT scopes and brand allowlist. |
