# Prompt Generation Playbook

Use this playbook when an agent needs to create or refine prompt coverage.

## Skills

- `geo-shared`
- `geo-prompt-ops`
- `geo-run-analysis` after prompts are run
- `geo-reporting` if the user wants a saved report

## Setup

```bash
geo doctor
geo auth status
geo brand current
geo schema prompts generate
geo schema prompts batch
```

## Workflow

1. Clarify the target coverage: feature, competitor, audience, region, or buyer
   decision.
2. Preview generated prompts:

```bash
geo prompts generate --keywords "brand comparison,pricing" --count 20
```

3. Remove duplicates and vague prompts.
4. Persist only after review:

```bash
geo prompts generate --keywords "brand comparison,pricing" --count 20 --apply --dry-run
geo prompts generate --keywords "brand comparison,pricing" --count 20 --apply
```

5. Optionally batch-run a focused set:

```bash
geo prompts batch-run --feature Price --concurrency 5 --watch
```

6. Export results for analysis:

```bash
geo runs export --since 24h --include response,citations,mentions,metrics,errors
```

## Quality Bar

- Preview before apply.
- Use `--dry-run` before persistence.
- Prefer selectors over `--all`.
- Preserve run ids for analysis.
- Save a report only after citing evidence.
