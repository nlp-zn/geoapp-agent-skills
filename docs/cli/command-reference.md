# Geo CLI Command Reference

The CLI is intentionally agent-first and machine-readable. Default output is
JSON. Watch and export flows use NDJSON or raw stdout where useful.

## Command Groups

This table is checked against `COMMAND_GROUPS` in
`packages/cli/src/schema/manifest.ts` by `pnpm release:cli:docs`.

| Group | Purpose |
| --- | --- |
| `auth` | Manage CLI authentication |
| `config` | Manage profiles and API URLs |
| `doctor` | Diagnose CLI, auth, brand, and schema setup |
| `api` | Call Geo App APIs directly |
| `schema` | Inspect command and API schemas |
| `agent-skills` | Install and diagnose agent skills |
| `brand` | Select and inspect brand context |
| `prompts` | Manage, generate, and run prompts |
| `runs` | Inspect, watch, cancel, and export prompt runs |
| `skills` | Execute server-side Geo App skills |
| `reports` | Save and retrieve reports |
| `competitors` | Inspect and manage competitors |
| `citations` | Inspect citation domains and pages |
| `analytics` | Retrieve overview and visibility analytics |
| `completion` | Generate shell completion scripts |

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
