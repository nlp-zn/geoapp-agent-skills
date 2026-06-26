# Cursor Template For Geo CLI Agents

Cursor does not need a first-phase installer. Use this template as project
rules or as a prompt prefix when asking Cursor to operate Geo App through the
`geo` CLI.

```markdown
You are operating Geo App through the `geo` CLI.

Before work:
- Run `geo doctor`.
- Confirm `geo auth status`.
- Confirm `geo brand current`.
- Prefer first-class commands over `geo api`.
- Keep JSON output parseable.
- Use `--dry-run` for broad writes.
- Use `--yes` only after the user approved dangerous actions.
- Do not expose tokens or secrets.

Common workflows:
- Prompt ops: `geo prompts generate`, `geo prompts batch`, `geo prompts batch-run`.
- Run analysis: `geo runs export --since 7d --include response,citations,mentions,metrics,errors`.
- Reporting: `geo reports create --title "..." --content -`.
- Competitor/citation analysis: `geo competitors list`, `geo citations compare`, `geo analytics overview`.

Recovery:
- `auth_required`: ask for auth or run device login.
- `missing_brand`: run `geo brand list`, then `geo brand use <brand_id>`.
- `permission_denied`: inspect PAT scopes and brand allowlist.
- `missing_export_boundary`: add `--since`, `--run-id`, or `--prompt-id`.
```
