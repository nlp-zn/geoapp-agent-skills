---
name: geo-prompt-ops
version: 0.2.0
description: Generate, create, import, run, and batch-run Geo App prompts through the `geo` CLI. Use this skill whenever the user asks to create prompts, customize prompt sets, batch run prompts, test prompt coverage, operate prompt generation from keywords, or turn GUI prompt workflows into agent-run CLI workflows.
min_cli_version: 0.1.0
tested_cli_version: 0.1.0
required_scopes:
  - prompts:read
  - prompts:write
  - prompts:run
  - runs:read
---

# Geo Prompt Ops

Use this skill for prompt lifecycle work: generate, create, import, delete,
run, and batch-run prompts.

## Required Shared Context

Run `geo-shared` first. Confirm:

```bash
geo doctor
geo auth status
geo brand current
geo schema prompts generate
geo schema prompts batch-run
```

## When To Use

- The user wants to turn keywords, positioning, features, competitors, or
  product questions into prompts.
- The user wants to import prompts from CSV, JSON, or JSONL files, or create
  single prompts from stdin or `@file` text.
- The user wants to run one prompt or batch-run selected prompts.
- The user wants an agent to replace GUI prompt generation or prompt execution.

## Do Not Use

- For interpreting completed run data; use `geo-run-analysis`.
- For saved report creation; use `geo-reporting`.
- For backend/server-side skill execution; use `geo-skill-execution`.
- For raw long-tail endpoints unless the manifest lacks a prompt command.

## Task Router

| User Goal | Command Path | Safety Default |
| --- | --- | --- |
| See existing prompts | `geo prompts list --limit 100` | Keep output JSON. |
| Inspect one prompt | `geo prompts get <prompt_id>` | Use real IDs from CLI output. |
| Generate candidates | `geo prompts generate --keywords "<terms>" --count <n>` | Preview first. |
| Persist generated prompts | `geo prompts generate ... --apply --dry-run`, then `--apply` | Dry-run first. |
| Create one prompt | `geo prompts create --text @prompt.txt --key-feature "<feature>" --dry-run` | Dry-run first. |
| Import many prompts | `geo prompts batch --from-file prompts.csv --dry-run` | Validate headers first. |
| Run one prompt | `geo prompts run <prompt_id> --watch` | Capture run id. |
| Batch-run selected prompts | `geo prompts batch-run --feature "<feature>" --concurrency 5 --watch` | Prefer selectors over `--all`. |
| Delete prompts | `geo prompts delete <prompt_id> --dry-run`, then `--yes` | Ask user before `--yes`. |

## Generation Workflow

1. Ask for or infer the objective: feature area, competitor set, target
   audience, geography, or decision the prompts should test.
2. Preview candidates:

```bash
geo prompts generate --keywords "pricing,alternatives,best vendor" --count 20
```

3. Review for duplicates, vague prompts, and prompts that cannot be answered by
   AI search behavior.
4. Persist only after preview:

```bash
geo prompts generate --keywords "pricing,alternatives,best vendor" --count 20 --apply --dry-run
geo prompts generate --keywords "pricing,alternatives,best vendor" --count 20 --apply
```

5. Run a selected batch:

```bash
geo prompts batch-run --feature Price --concurrency 5 --watch
```

6. Hand off to `geo-run-analysis` for the export and interpretation.

## Import Workflow

Use CSV for human-edited sheets and JSON/JSONL for agent-generated payloads.
Read `references/prompt-inputs.md` before converting files or diagnosing input
errors.

CSV should include a `promptText` column. Optional columns may include
`keyFeature`, `promptType`, `tags`, and `metadata`.

```bash
geo prompts batch --from-file prompts.csv --dry-run
geo prompts batch --from-file prompts.csv
```

For generated JSON:

```bash
geo prompts batch --from-file prompts.jsonl --dry-run
```

## Batch-Run Selection Rules

- Prefer targeted selectors: `--feature`, `--type`, `--tag`, `--status`, or
  `--from-file`.
- Use `--all` only when the user explicitly wants every eligible prompt.
- Start with `--concurrency 5` unless the user requests a different load
  profile or the platform indicates a lower limit.
- Capture the returned run ids, status URLs, and event URLs for follow-up.

## Expected Outputs

`geo prompts generate` returns generated candidates and `apply: false` unless
`--apply` is used.

`geo prompts run` and `geo prompts batch-run` return run metadata plus
`status_url`, `stream_url`, and `events_url`.

`--watch` emits NDJSON. Parse one object per line.

## Recovery

| Error | Recovery |
| --- | --- |
| `missing_prompt_selector` | Pass `--all`, `--feature`, `--type`, `--tag`, `--status`, or `--from-file`. |
| `invalid_prompts_file` | Read `references/prompt-inputs.md`; check `promptText` and JSON object shape. |
| `missing_brand` | Return to `geo-shared` brand recovery. |
| `permission_denied` | Token needs `prompts:write` or `prompts:run`; check brand allowlist. |
| Empty batch selection | List prompts, verify feature/type/tag spelling, then narrow or widen selector. |
| Watch interrupted | Use returned run id with `geo runs watch <run_id>` or `geo runs events <run_id>`. |

## References

- Read `references/prompt-inputs.md` for CSV, JSON, JSONL, `@file`, stdin
  text, and batch-run selector details.
