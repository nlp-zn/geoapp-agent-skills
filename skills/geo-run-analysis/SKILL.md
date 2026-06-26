---
name: geo-run-analysis
description: Export prompt runs, inspect events, and analyze responses, mentions, citations, metrics, and errors.
min_cli_version: 0.1.0
tested_cli_version: 0.1.0
required_scopes:
  - runs:read
  - runs:cancel
  - reports:write
---

# Geo Run Analysis

Use this skill when the user asks what happened in prompt runs, why performance
changed, or what operational decision to make from run data.

## Commands

List recent runs:

```bash
geo runs list --since 24h --limit 50
```

Export analysis-ready runs:

```bash
geo runs export --since 7d --include response,citations,mentions,metrics,errors
```

Export one run as JSONL:

```bash
geo runs export --run-id <run_id> --include response,citations,mentions,metrics,errors --format jsonl
```

Inspect events:

```bash
geo runs events <run_id> --limit 200
geo runs watch <run_id> --limit 200
```

Cancel a run only when approved:

```bash
geo runs cancel <run_id> --dry-run
geo runs cancel <run_id> --yes
```

## Analysis Checklist

- Group by prompt, provider, status, and run date.
- Identify failed providers and repeated error codes.
- Extract answer themes from `responseText`.
- Compare brand mentions and competitor mentions.
- Summarize high-authority citations and missing citation categories.
- Separate data-backed conclusions from hypotheses.

## Expected Output

`geo runs export` returns stable run objects with:

- run id, status, created/completed timestamps
- prompt id and prompt text
- provider states
- executions with response text, mentions, citations, metrics, and errors

Do not depend on raw database fields or internal `extra` payloads.

## Recovery

- `missing_export_boundary`: add `--since 7d`, `--run-id`, or `--prompt-id`.
- `permission_denied`: token needs `runs:read` or `runs:cancel`.
- Empty export: widen `--since`, check brand context, then inspect `geo prompts list`.
