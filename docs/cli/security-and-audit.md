# Security And Audit

## Tenant Isolation

Brand-scoped commands must send `X-Brand-Id` from explicit CLI context. The CLI
must not invent or fallback to a first brand. Backend routes must still enforce
brand ownership.

## Token Handling

- Prefer browser/device login for humans.
- Prefer PAT/API keys with narrow scopes for automation.
- Do not commit `.env`, config files, credentials files, or raw PAT values.
- Revoke tokens from the GUI account page or with `geo auth token revoke`.

## Raw API Writes

`geo api POST/PATCH/DELETE` requires `api:raw`. Dangerous deletes require
`--yes`; dry-run is available for preview.

## Audit Trail

CLI requests include stable metadata headers:

```text
X-Geo-CLI-Version
X-Geo-Command
X-Geo-Profile
X-Brand-Id
```

Backend audit logs should capture command name, user, token identity, method,
path, brand, status, request ID, and timing without storing raw token values.
