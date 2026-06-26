# Geo CLI Command Reference

The CLI is intentionally agent-first and machine-readable. Default output is
JSON. Watch and export flows use NDJSON or raw stdout where useful.

## Command Groups

| Group | Purpose |
| --- | --- |
| `auth` | Token set/create/list/revoke, browser login, device login, status, whoami |
| `config` | Profiles and API URL defaults |
| `doctor` | Local config, backend health, and compatibility checks |
| `api` | Raw GET/POST/PATCH/DELETE fallback for long-tail endpoints |
| `schema` | Local command manifest and backend OpenAPI lookup/cache |
| `agent-skills` | Install and diagnose external agent playbooks |
| `brand` | List, select, and inspect brand context |
| `prompts` | List/get/create/batch/delete/generate/run/batch-run |
| `runs` | List/get/events/watch/cancel/export |
| `skills` | List/detail/config/execute/watch/cancel backend skills |
| `reports` | List/get/create/delete reports |
| `competitors` | List/import/export/merge/confirm/restore competitors |
| `citations` | Domains/pages/compare/emerging/top-authority/classify |
| `analytics` | Overview/visibility/competitor-analysis/refresh/generate-insights |

## Discovery

```bash
geo --help
geo schema commands
geo schema prompts batch-run
geo schema api POST /api/v1/prompts
```

## Safety Flags

Writes should support `--dry-run` where preview is meaningful. Dangerous writes
also require `--yes`.

Examples:

```bash
geo prompts batch --from-file prompts.csv --dry-run
geo competitors merge --into <primary_id> --from <duplicate_id> --dry-run
geo competitors merge --into <primary_id> --from <duplicate_id> --yes
```
