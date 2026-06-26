---
name: geo-prompt-ops
description: Generate, create, batch import, run, and batch-run Geo App prompts through the geo CLI.
min_cli_version: 0.1.0
tested_cli_version: 0.1.0
required_scopes:
  - prompts:read
  - prompts:write
  - prompts:run
  - runs:read
---

# Geo Prompt Ops

Use this skill when the user wants prompt generation, prompt import, prompt
execution, or batch prompt runs.

## Required Shared Context

Run `geo-shared` setup first: auth, brand, doctor, and schema checks.

## Commands

List prompts:

```bash
geo prompts list
```

Generate candidates without persisting:

```bash
geo prompts generate --keywords "brand comparison,pricing" --count 20
```

Persist generated prompts only after review:

```bash
geo prompts generate --keywords "brand comparison,pricing" --count 20 --apply --dry-run
geo prompts generate --keywords "brand comparison,pricing" --count 20 --apply
```

Create one prompt from a file:

```bash
geo prompts create --text @prompt.txt --key-feature Price --dry-run
geo prompts create --text @prompt.txt --key-feature Price
```

Batch import prompts:

```bash
geo prompts batch --from-file prompts.csv --dry-run
geo prompts batch --from-file prompts.csv
```

Run one prompt:

```bash
geo prompts run <prompt_id> --watch
```

Batch-run selected prompts:

```bash
geo prompts batch-run --feature Price --concurrency 5 --watch
```

## Expected Output

`geo prompts generate` returns generated candidates and `apply: false` unless
`--apply` is used.

`geo prompts run` returns `run`, `status_url`, `stream_url`, and `events_url`.

`--watch` emits NDJSON start events; parse one JSON object per line.

## Safe Defaults

- Preview generated prompts before `--apply`.
- Use `--dry-run` for all broad creates.
- Use `--concurrency 5` unless the user requested a different load profile.
- Prefer selectors over `--all` when a user asks for focused analysis.

## Recovery

- `missing_prompt_selector`: pass `--all`, `--feature`, `--type`, `--tag`, `--status`, or `--from-file`.
- `invalid_prompts_file`: use CSV with `promptText` or JSON/JSONL with `promptText`.
- `permission_denied`: token needs `prompts:write` or `prompts:run`.
