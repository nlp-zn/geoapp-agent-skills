# Geo CLI Auth

The first launch supports both PAT/API key auth and browser/device login. Tokens
represent the user. Service accounts, bot identity, and `--as user/bot` are out
of scope for this phase.

## PAT/API Key

Create from the GUI account page or from CLI:

```bash
geo auth token create --name "Local Agent" --scope prompts:read --scope prompts:write --scope prompts:run --store
geo auth status
```

Use explicit tokens in CI or agent sandboxes:

```bash
export GEO_ACCESS_TOKEN=<pat>
```

PATs support expiration, revocation, last-used metadata, command-domain scopes,
and brand allowlists. Raw writes through `geo api POST/PATCH/DELETE` require
`api:raw`.

## Browser Login

```bash
geo auth login
geo auth whoami
```

The CLI stores an access token and refresh token locally. Expiring access tokens
are refreshed before commands when the API URL is known.

## Device Code

For terminal-only or remote agent sessions:

```bash
geo auth login --device --no-wait
geo auth login --device-code <device_code>
```

The user approves the code from the account page.

## Minimum Scopes By Workflow

```text
Prompt generation and batch run: prompts:read prompts:write prompts:run
Run analysis and reporting:      runs:read reports:write
Server skill execution:          skills:read skills:execute
Competitor/citation analysis:    competitors:read citations:read analytics:read
Raw API writes:                  api:raw
Analytics refresh/insights:      analytics:write
```
