# Competitor Citation Analysis Playbook

Use this playbook when an agent needs to explain why competitors appear more
often than the brand, which domains influence AI answers, and what to do next.

## Skills

- `geo-shared`
- `geo-competitor-citation-analysis`
- `geo-run-analysis`
- `geo-reporting` when saving the recommendation

## Setup

```bash
geo doctor
geo auth status
geo brand current
geo schema citations compare
```

## Commands

```bash
geo analytics overview --section visibility --section kpis
geo competitors list --status pending --limit 100
geo citations compare --days 30 --limit 50
geo citations emerging --days 7 --order-by trend
geo citations top-authority --limit-per-category 10
```

Pull prompt-level evidence before making a strong recommendation:

```bash
geo runs export --since 7d --include response,citations,mentions,metrics,errors
```

## Decision Steps

1. Identify current visibility and KPI movement.
2. Clean obvious competitor hygiene issues.
3. Compare brand-led, competitor-led, and contested citation domains.
4. Inspect high-authority domains and emerging domains.
5. Connect citation gaps to prompt-level responses.
6. Recommend prompt, content, citation, competitor, or analytics actions.

## Safety

- Use `--dry-run` for imports, merges, and restores.
- Confirm intent before running `geo analytics refresh`; it has no dry-run mode.
- Confirm only obvious competitors.
- Do not merge competitors unless the user approves the primary and source ids.
- Say when data is sparse or stale.
