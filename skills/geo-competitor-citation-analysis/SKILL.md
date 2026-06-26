---
name: geo-competitor-citation-analysis
description: Investigate competitors, citations, and analytics signals to explain GEO performance and recommend actions.
min_cli_version: 0.1.0
tested_cli_version: 0.1.0
required_scopes:
  - competitors:read
  - competitors:write
  - citations:read
  - analytics:read
  - analytics:write
---

# Geo Competitor Citation Analysis

Use this skill when the user asks why a brand is underperforming, which
competitors matter, which citations influence answers, or what decision to make
from analytics.

## Competitor Commands

```bash
geo competitors list --status pending --limit 100
geo competitors export > competitors.csv
geo competitors import --from-file competitors.csv --dry-run
geo competitors confirm <competitor_id>
geo competitors merge --from <source_id> --into <primary_id> --dry-run
geo competitors merge --from <source_id> --into <primary_id> --yes
geo competitors restore <competitor_id>
```

## Citation Commands

```bash
geo citations domains --days 30 --order-by share --limit 50
geo citations pages --search pricing --platform openai
geo citations compare --days 30 --limit 50
geo citations emerging --days 7 --order-by trend
geo citations top-authority --limit-per-category 10
geo citations classify example.com
```

## Analytics Commands

```bash
geo analytics overview --section visibility --section kpis
geo analytics visibility --refresh
geo analytics competitor-analysis --feature Price --refresh
geo analytics refresh
geo analytics generate-insights --async
```

## Decision Workflow

1. Check overview visibility and KPIs.
2. Export or list competitors; confirm obvious pending competitors.
3. Compare citation domains for brand-led, competitor-led, and contested areas.
4. Inspect emerging and top authority domains.
5. Use run export from `geo-run-analysis` for prompt-level evidence.
6. Save the recommendation with `geo-reporting`.

## Safe Defaults

- Use `--dry-run` for import and merge.
- Use `--yes` for merge only after approval.
- Keep long-tail admin endpoints under `geo api`.
- Avoid broad analytics refreshes unless the user needs current data.

## Recovery

- `permission_denied`: check `competitors:*`, `citations:read`, or `analytics:*` scopes.
- Empty citation lists: widen `--days` up to 90 and verify brand context.
- Analytics refresh queued: poll task status through `geo api GET /api/v1/analytics/overview-complete/refresh-status/<task_id>`.
