# Batch Run Analysis Playbook

Use this playbook when an agent needs to interpret prompt-run output and make a
decision from actual responses, mentions, citations, metrics, and errors.

## Skills

- `geo-shared`
- `geo-run-analysis`
- `geo-reporting` when saving the result

## Setup

```bash
geo doctor
geo auth status
geo brand current
geo schema runs export
```

## Export

Use a clear boundary:

```bash
geo runs export --since 7d --include response,citations,mentions,metrics,errors --format jsonl > runs.jsonl
```

For one run:

```bash
geo runs export --run-id <run_id> --include response,citations,mentions,metrics,errors --format jsonl
geo runs events <run_id> --limit 200
```

## Analysis Steps

1. Count runs and provider executions.
2. Separate success, failure, cancellation, and running states.
3. Group by prompt, provider, feature, and date.
4. Extract answer themes from responses.
5. Compare brand mentions and competitor mentions.
6. Summarize citation domains and high-authority pages.
7. Identify repeated provider or backend errors.
8. State confidence and data gaps.

## Output

Return:

- export boundary
- top findings
- evidence table
- risks and unknowns
- next actions

When the user wants persistence:

```bash
cat report.md | geo reports create --title "Batch Run Analysis" --content -
```
