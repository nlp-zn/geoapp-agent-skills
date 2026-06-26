# Example: Explain Competitor And Citation Gaps

## User Prompt

```text
Tell me why competitors are showing up more often than us in AI answers. Look
at competitors, citations, and recent prompt runs, then recommend what we
should do next.
```

## Skills That Should Trigger

- `geo-shared`
- `geo-competitor-citation-analysis`
- `geo-run-analysis`
- `geo-reporting`

## Expected Agent Behavior

1. Run setup checks:

```bash
geo doctor
geo auth status
geo brand current
```

2. Get current state:

```bash
geo analytics overview --section visibility --section kpis
geo competitors list --status pending --limit 100
geo citations compare --days 30 --limit 50
geo citations emerging --days 7 --order-by trend
geo citations top-authority --limit-per-category 10
```

3. Confirm only obvious pending competitors. Leave ambiguous items untouched.

4. Pull prompt-level evidence:

```bash
geo runs export --since 7d --include response,citations,mentions,metrics,errors
```

5. Save a recommendation report if requested:

```bash
cat report.md | geo reports create --title "Competitor Citation Gap Analysis" --content -
```

## Quality Bar

- Distinguish citation gaps from prompt coverage gaps.
- Tie recommendations to domains, prompts, or analytics sections.
- Use `--dry-run` before competitor import, merge, or restore.
- Confirm intent before running `geo analytics refresh`; it has no dry-run
  mode.
- Do not merge competitors without explicit user approval.
