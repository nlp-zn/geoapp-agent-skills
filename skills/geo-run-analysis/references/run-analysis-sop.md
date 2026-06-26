# Run Analysis SOP

Use this reference when turning prompt-run exports into decisions.

## Inputs To Capture

Record the analysis boundary:

- brand id or brand name
- export window, run id, or prompt id
- provider set
- prompt selector or feature
- include fields used in export

Recommended export:

```bash
geo runs export --since 7d --include response,citations,mentions,metrics,errors --format jsonl > runs.jsonl
```

## Triage Before Analysis

Before drawing conclusions:

1. Count runs and executions.
2. Separate successful, failed, cancelled, and running states.
3. Identify provider-level failures.
4. Check whether retry executions or hidden rows are excluded by the API
   contract.
5. Confirm sample size is large enough for the requested conclusion.

## Analysis Dimensions

| Dimension | What To Look For |
| --- | --- |
| Prompt | Which prompt texts create strong or weak brand visibility? |
| Provider | Which models or platforms diverge? |
| Mentions | Is the brand mentioned, compared, recommended, or omitted? |
| Competitors | Which competitors appear repeatedly and in what context? |
| Citations | Which domains/pages support answers? Are they authoritative? |
| Metrics | Visibility, position, citation share, and prompt-level movement. |
| Errors | Provider errors, timeouts, rate limits, invalid responses, cancelled runs. |

## Failure Diagnosis

Use events for one run:

```bash
geo runs events <run_id> --limit 200
```

Classify failures:

- auth or permission failure
- provider timeout or rate limit
- cancelled run
- validation or malformed input
- backend persistence or retry issue
- empty response or no citations

For each failure, state whether it affects analysis completeness.

## Decision Quality

Good decisions include:

- the evidence boundary
- what changed
- the likely cause
- confidence level
- operational action
- follow-up command if more data is required

Avoid claiming a trend from a single run unless the user asked for an example.

## Handoff To Reporting

When saving a report, pass a concise evidence set:

- export command
- run ids or window
- top findings
- notable prompt ids
- cited domains/pages
- risks and unknowns
