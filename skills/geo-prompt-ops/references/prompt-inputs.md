# Prompt Inputs And Batch Runs

Use this reference when creating prompt files, importing prompts, or selecting
prompts for batch runs.

## CSV Input

CSV is best when prompts come from a spreadsheet.

Required column:

```text
promptText
```

Recommended optional columns:

```text
keyFeature
promptType
tags
metadata
```

Example:

```csv
promptText,keyFeature,promptType,tags
"What are the best alternatives to Acme for enterprise teams?",Alternatives,comparison,"enterprise,alternatives"
"How does Acme pricing compare with competitors?",Price,comparison,"pricing"
```

Run:

```bash
geo prompts batch --from-file prompts.csv --dry-run
geo prompts batch --from-file prompts.csv
```

## JSON And JSONL Input

Use JSON for agent-generated prompts.

JSON array:

```json
[
  {
    "promptText": "What are the best Acme alternatives for enterprise teams?",
    "keyFeature": "Alternatives",
    "promptType": "comparison",
    "tags": ["enterprise", "alternatives"]
  }
]
```

JSONL:

```jsonl
{"promptText":"What are the best Acme alternatives?","keyFeature":"Alternatives"}
{"promptText":"How does Acme pricing compare?","keyFeature":"Price"}
```

Run:

```bash
geo prompts batch --from-file prompts.jsonl --dry-run
```

## Long Text And Batch Files

Use `@file` or stdin for long single-prompt text. For batch imports, write the
CSV, JSON, or JSONL content to a file and pass that file path to `--from-file`.

```bash
geo prompts create --text @prompt.txt --key-feature Price --dry-run
cat prompt.txt | geo prompts create --text - --key-feature Price --dry-run
```

Avoid shell-escaped large JSON in inline arguments when a file path or stdin
source is available for the supported command.

## Generation Quality Checks

Before applying generated prompts, remove or revise prompts that are:

- duplicates of existing prompts
- too vague to produce an actionable AI answer
- not tied to the brand, competitors, category, audience, or buying decision
- asking for private facts the model cannot know
- mixing multiple unrelated questions in one prompt

## Batch-Run Selectors

Prefer focused selectors:

```bash
geo prompts batch-run --feature Price --concurrency 5 --watch
geo prompts batch-run --type comparison --concurrency 5 --watch
geo prompts batch-run --tag enterprise --concurrency 5 --watch
geo prompts batch-run --from-file prompt-ids.txt --watch
```

Use `--all` only when the user explicitly wants all eligible prompts.

## After Batch Run

Capture run ids and hand off to run analysis:

```bash
geo runs list --since 24h --limit 50
geo runs export --since 24h --include response,citations,mentions,metrics,errors
```
