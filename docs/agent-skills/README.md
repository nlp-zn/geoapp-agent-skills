# Agent Skills Docs

Open-source agent skills live in `packages/agent-skills` and are distributed separately from the private CLI source. These docs cover installation, public sync, templates, and manual validation.

## Start Here

- Public repository: <https://github.com/nlp-zn/geoapp-agent-skills>
- CLI install: `npm install -g geoapp-cli@beta` (`geoapp-cli`, not
  `@geoapp/cli`; executable is `geo`)
- Hosted API default: `https://api.prompt-insights.com`
- Codex install: `install-codex.md`
- Claude Code install: `install-claude.md`
- Cursor-style template: `cursor-template.md`
- Public mirror sync: `public-sync.md`
- Manual test plan: `manual-test-plan.md`
- Playbooks: `playbooks/`

## Product Boundary

- `geo agent-skills` installs and checks Codex/Claude-style playbooks for user-owned agents.
- `geo skills` controls backend/server skills through existing backend APIs.
- Do not mix server skill execution docs into public agent-skill playbooks unless the playbook intentionally calls the backend through the CLI.

## Public Sync

Use the repo root command when preparing public skill updates:

```bash
pnpm sync:agent-skills:public
pnpm check:agent-skills-content
```

Public sync rules and expected outputs are documented in `public-sync.md`.
