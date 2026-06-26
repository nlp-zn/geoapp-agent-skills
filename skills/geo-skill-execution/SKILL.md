---
name: geo-skill-execution
description: Execute and monitor backend/server-side Geo App skills through geo skills commands.
min_cli_version: 0.1.0
tested_cli_version: 0.1.0
required_scopes:
  - skills:read
  - skills:execute
---

# Geo Skill Execution

Use this skill when the user asks to run a backend/server-side Geo App skill.
Do not confuse this with open-source agent skills. `geo skills execute` runs
server skills exposed by the Geo App backend.

## Commands

List server-side skills:

```bash
geo skills list
```

Inspect a server-side skill:

```bash
geo skills detail <slug>
geo skills config <slug>
```

Update config only when requested:

```bash
geo skills config <slug> --disable --dry-run
geo skills config <slug> --disable
```

Execute a skill:

```bash
geo skills execute <slug> --input @input.json
```

Watch execution events:

```bash
geo skills watch <execution_id>
```

Cancel execution only after approval:

```bash
geo skills cancel <execution_id> --dry-run
geo skills cancel <execution_id> --yes
```

## Input Shape

`--input` must be a JSON object. Use the server skill detail/config to determine
required fields.

## Expected Output

`geo skills execute` returns an execution object with id, task id, status URL,
and stream URL. `geo skills watch` emits NDJSON converted from backend SSE
events.

## Recovery

- `invalid_json`: pass a JSON object, not an array or string.
- `not_found`: check `geo skills list` for the server skill slug.
- `permission_denied`: token needs `skills:read` and `skills:execute`.
