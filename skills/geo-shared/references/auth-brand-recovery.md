# Auth And Brand Recovery

Use this reference when a Geo CLI task cannot proceed because the agent lacks
auth, a brand, scopes, or server compatibility.

## Recovery Order

1. Check local CLI and server compatibility:

```bash
geo doctor
geo --version
```

2. Check auth:

```bash
geo auth status
geo auth whoami
```

3. Check brand context:

```bash
geo brand current
```

4. Check command contract:

```bash
geo schema commands
geo schema <group> <command>
```

## Device Login Split Flow

Use this when the user is present but the agent should not block forever.

Current turn:

```bash
geo auth login --device --no-wait
```

Return the verification URL to the user and stop. Do not start polling before
the user has seen the URL.

After the user confirms approval:

```bash
geo auth login --device-code <device_code>
geo auth status
```

Do not reuse stale device codes. If the code expired, start a new login.

## PAT/API Key Flow

Use PATs for automation, CI, or remote agents.

```bash
geo auth token set <token>
geo auth status
```

Prefer narrow scopes:

| Workflow | Minimum Scopes |
| --- | --- |
| Prompt generation and run | `prompts:read`, `prompts:write`, `prompts:run`, `runs:read` |
| Run analysis and reporting | `runs:read`, `reports:write` |
| Server skill execution | `skills:read`, `skills:execute` |
| Competitor and citation analysis | `competitors:read`, `citations:read`, `analytics:read` |
| Competitor writes | `competitors:write` |
| Analytics refresh/insights | `analytics:write` |
| Raw API writes | `api:raw` |

Do not print the raw token after setting it. Use status and masked metadata.

## Brand Selection

If no brand is selected:

```bash
geo brand list
```

If the result is unambiguous:

```bash
geo brand use <brand_id>
```

If multiple brands could match, ask the user to choose. Do not default to the
first brand, because that can mix tenant data and invalidate analysis.

## Permission Denied Triage

When a command fails with `permission_denied`, check in this order:

1. Does the token have the command-domain scope?
2. Is the selected brand inside the PAT brand allowlist?
3. Is the command a write that needs a write scope?
4. Is the command a raw API write that needs `api:raw`?
5. Is the backend version compatible with the CLI?

Report the missing capability without exposing token material.

## Confirmation Required

Some dangerous writes require explicit confirmation.

1. Run or preserve the dry-run preview.
2. Tell the user which resource and action will be affected.
3. Wait for explicit approval.
4. Retry the same command with `--yes` appended.

Do not silently add `--yes`.
