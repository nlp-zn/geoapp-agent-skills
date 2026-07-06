---
name: geo-run-analysis
version: 0.2.0
description: Export Geo prompt runs, inspect run events, diagnose failures, and turn model responses, mentions, citations, metrics, and errors into evidence-backed decisions. Use this skill whenever the user asks what happened in a run, why GEO visibility changed, whether prompt results are trustworthy, or how to analyze batch prompt output.
min_cli_version: 0.1.0
tested_cli_version: 0.1.1
required_scopes:
  - runs:read
  - runs:cancel
  - reports:write
---

# Geo Run Analysis

Use this skill to inspect prompt runs and produce evidence-backed conclusions.

## Required Shared Context

Run `geo-shared` first. Confirm:

```bash
geo doctor
geo auth status
geo brand current
geo schema runs export
```

## When To Use

- The user asks what happened in prompt runs or why a batch changed.
- The user asks for response, mention, citation, provider, or error analysis.
- The user needs a decision from actual prompt-run data.
- The user wants to cancel or monitor a run.

## Do Not Use

- For creating prompts; use `geo-prompt-ops`.
- For saving the final report body; use `geo-reporting`.
- For competitor/citation discovery without prompt-level evidence; start with
  `geo-competitor-citation-analysis` and return here when run evidence is
  needed.

## Task Router

| User Goal | Command Path | Notes |
| --- | --- | --- |
| List recent runs | `geo runs list --since 24h --limit 50` | Good first orientation. |
| Inspect one run | `geo runs get <run_id>` | Use run id from CLI output. |
| Export a time window | `geo runs export --since 7d --include response,citations,mentions,metrics,errors` | Required for analysis. |
| Export one run | `geo runs export --run-id <run_id> --include response,citations,mentions,metrics,errors --format jsonl` | Best for debugging. |
| Inspect events | `geo runs events <run_id> --limit 200` | Use for errors and timing. |
| Watch a run | `geo runs watch <run_id> --limit 200` | NDJSON stream. |
| Cancel a run | `geo runs cancel <run_id> --dry-run`, then `--yes` | Ask user before `--yes`. |

## Export Rules

- Always include a boundary: `--since`, `--until`, `--run-id`, or
  `--prompt-id`.
- Use this include set for analysis unless the user asks for a smaller export:

```bash
geo runs export --since 7d --include response,citations,mentions,metrics,errors
```

- Prefer JSONL for large exports:

```bash
geo runs export --since 30d --include response,citations,mentions,metrics,errors --format jsonl > runs.jsonl
```

- In JSON output, full model answers are nested at
  `runs[].executions[].responseText`.
- In JSONL output, each line is one run, so read
  `executions[].responseText`, `executions[].mentions`, and
  `executions[].citations` from each line.
- Do not concatenate `raw_events_sample` from `geo runs events`; those are
  diagnostic samples and may be intentionally short.

Quick full-answer inspection:

```bash
jq -r '.executions[] | [.provider, .responseText] | @tsv' runs.jsonl
```

- Do not rely on raw database fields or internal `extra` payloads. Use the
  documented export contract.

## Analysis Workflow

Read `references/run-analysis-sop.md` when the user asks for a decision, a
trend explanation, or a failure diagnosis.

Short version:

1. Identify export scope: brand, time window, prompt selector, providers.
2. Group by prompt, provider, status, run date, and feature.
3. Separate missing data, failed providers, stale retries, and successful
   responses.
4. Extract answer themes from `responseText`.
5. Compare brand mentions and competitor mentions.
6. Summarize citation domains, high-authority sources, and missing source types.
7. Turn observations into decisions, risks, and next actions.
8. Hand off to `geo-reporting` if the user wants a durable report.

## Evidence Requirements

Every conclusion should cite at least one of:

- run id or export window
- prompt id or prompt text
- provider and status
- citation domain/page
- mention counts or metrics
- error code or event timestamp

If sample size is small, say so. If data is stale or incomplete, say what to run
next before making a strong decision.

## Recovery

| Error Or Symptom | Recovery |
| --- | --- |
| `missing_export_boundary` | Add `--since 7d`, `--run-id`, or `--prompt-id`. |
| Empty export | Widen `--since`, check brand, list prompts, then retry. |
| `responseText` is null | Confirm `--include response`, inspect `executions[]` rather than a top-level `responses` field, then run `geo runs events <run_id>` only to diagnose persistence/capture failures. |
| Only `raw_events_sample` has text | Re-run `geo runs export --run-id <run_id> --include response,citations,mentions,metrics,errors`; event samples are not the full answer contract. |
| `permission_denied` | Token needs `runs:read` or `runs:cancel`. |
| Watch disconnected | Use `geo runs events <run_id>` to recover event history. |
| Too much output | Re-run with tighter `--since`, `--prompt-id`, or JSONL output. |
| User asks to cancel | Dry-run first, explain target run, then use `--yes` only after approval. |

## References

- Read `references/run-analysis-sop.md` for the full analysis and failure
  triage method.
