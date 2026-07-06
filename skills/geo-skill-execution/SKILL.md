---
name: geo-skill-execution
version: 0.2.0
description: Execute and monitor backend/server-side Geo App skills through `geo skills` commands. Use this skill when the user asks to run a platform skill, inspect available server skills, configure or disable a server skill, watch a skill execution, cancel an execution, or distinguish backend skills from open-source agent skills.
min_cli_version: 0.1.0
tested_cli_version: 0.1.1
required_scopes:
  - skills:read
  - skills:execute
---

# Geo Skill Execution

Use this skill for server-side Geo App skills exposed by the backend. Do not
confuse these with open-source agent skills installed through
`geo agent-skills`.

## Required Shared Context

Run `geo-shared` first:

```bash
geo doctor
geo auth status
geo brand current
geo schema skills execute
```

## When To Use

- The user asks to run a backend/server-side Geo App skill.
- The user asks what platform skills are available for a brand.
- The user wants to inspect, configure, watch, or cancel a server skill
  execution.
- The user is confused about `geo skills` versus `geo agent-skills`.

## Do Not Use

- For installing Codex/Claude playbooks; use `geo agent-skills`.
- For authoring open-source agent skills; use the public skills source.
- For prompt generation or run exports unless a server skill specifically wraps
  that workflow.

## Mental Model

| Surface | Command | Runs Where | Purpose |
| --- | --- | --- | --- |
| Agent skills | `geo agent-skills install` | User's agent environment | Teaches agents how to use the CLI. |
| Server skills | `geo skills execute` | Geo App backend | Runs platform-managed jobs with backend permissions. |

## Task Router

| User Goal | Command Path | Notes |
| --- | --- | --- |
| List server skills | `geo skills list` | Start here when slug is unknown. |
| Inspect a skill | `geo skills detail <slug>` | Read required input schema. |
| Inspect config | `geo skills config <slug>` | Use before changing config. |
| Disable/enable config | `geo skills config <slug> --disable --dry-run`, then write | Dry-run first. |
| Execute a skill | `geo skills execute <slug> --input @input.json` | Input must be a JSON object. |
| Watch execution | `geo skills watch <execution_id>` | NDJSON converted from backend events. |
| Cancel execution | `geo skills cancel <execution_id> --dry-run`, then `--yes` | Ask user before `--yes`. |

## Execution Workflow

1. List or resolve the skill slug.
2. Run `geo skills detail <slug>` and `geo skills config <slug>`.
3. Build a JSON object that matches the returned input contract.
4. Execute:

```bash
geo skills execute <slug> --input @input.json
```

5. Capture `execution_id`, `task_id`, `status_url`, and `stream_url`.
6. Watch events:

```bash
geo skills watch <execution_id>
```

7. If the execution produces analysis, hand off to `geo-reporting`.

Read `references/server-skill-inputs.md` before constructing complex inputs or
recovering validation errors.

## Recovery

| Error Or Symptom | Recovery |
| --- | --- |
| `invalid_json` | Pass a JSON object, not an array or string. |
| `not_found` | Run `geo skills list` and verify the server skill slug. |
| `permission_denied` | Token needs `skills:read` and `skills:execute`; check brand allowlist. |
| Missing required input | Re-read `geo skills detail <slug>` and build `--input @file`. |
| Watch interrupted | Re-run `geo skills watch <execution_id>` or inspect status URL. |
| User asks to cancel | Dry-run first, explain target execution, then use `--yes` only after approval. |

## References

- Read `references/server-skill-inputs.md` for input construction, execution
  monitoring, and cancellation handling.
