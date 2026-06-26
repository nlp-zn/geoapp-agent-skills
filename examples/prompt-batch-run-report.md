# Example: Generate Prompts, Batch Run, And Save A Report

## User Prompt

```text
Generate 20 prompts for our pricing and alternatives coverage, preview them
first, then after I approve, apply them, batch-run the Price prompts, analyze
the answers, and save a report in Geo App.
```

## Skills That Should Trigger

- `geo-shared`
- `geo-prompt-ops`
- `geo-run-analysis`
- `geo-reporting`

## Expected Agent Behavior

1. Run setup checks:

```bash
geo doctor
geo auth status
geo brand current
geo schema prompts generate
```

2. Preview generated prompts:

```bash
geo prompts generate --keywords "pricing,alternatives" --count 20
```

3. Ask the user to approve persistence.

4. Dry-run and apply:

```bash
geo prompts generate --keywords "pricing,alternatives" --count 20 --apply --dry-run
geo prompts generate --keywords "pricing,alternatives" --count 20 --apply
```

5. Batch-run selected prompts:

```bash
geo prompts batch-run --feature Price --concurrency 5 --watch
```

6. Export results:

```bash
geo runs export --since 24h --include response,citations,mentions,metrics,errors --format jsonl > runs.jsonl
```

7. Draft and save report:

```bash
cat report.md | geo reports create --title "Pricing And Alternatives GEO Analysis" --content -
```

## Quality Bar

- Do not apply generated prompts before preview.
- Do not use `--all` unless the user asks for all prompts.
- Include run ids or export window in the report.
- Include next actions, not just observations.
