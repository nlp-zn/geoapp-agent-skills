# Example: Triage A Failed Run

## User Prompt

```text
This run looks wrong. Figure out whether it failed because of provider errors,
bad prompt data, cancellation, or a backend issue. Do not cancel or rerun
anything unless I approve.
```

## Skills That Should Trigger

- `geo-shared`
- `geo-run-analysis`

## Expected Agent Behavior

1. Run setup checks:

```bash
geo doctor
geo auth status
geo brand current
```

2. Inspect the run:

```bash
geo runs get <run_id>
geo runs events <run_id> --limit 200
geo runs export --run-id <run_id> --include response,citations,mentions,metrics,errors --format jsonl
```

3. Classify the failure:

- provider timeout or rate limit
- auth or permission issue
- malformed input
- cancelled run
- backend persistence or retry issue
- empty provider response

4. Report whether the run can be trusted for analysis.

## Quality Bar

- Do not rerun, retry, or cancel without approval.
- Include event timestamps or error codes when available.
- Separate root cause from impact on analysis.
