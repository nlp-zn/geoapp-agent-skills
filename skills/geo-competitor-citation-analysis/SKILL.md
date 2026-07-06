---
name: geo-competitor-citation-analysis
version: 0.2.0
description: Investigate Geo competitor, citation, and analytics signals to explain brand visibility and recommend actions. Use this skill whenever the user asks why a brand is underperforming, which competitors matter, what citations influence AI answers, which domains are emerging, how to improve authority, or how analytics should drive decisions.
min_cli_version: 0.1.0
tested_cli_version: 0.1.1
required_scopes:
  - competitors:read
  - competitors:write
  - citations:read
  - analytics:read
  - analytics:write
---

# Geo Competitor Citation Analysis

Use this skill to explain GEO performance through competitors, citations, and
analytics. For prompt-level evidence, combine it with `geo-run-analysis`.

## Required Shared Context

Run `geo-shared` first:

```bash
geo doctor
geo auth status
geo brand current
geo schema analytics overview
geo schema citations compare
```

## When To Use

- The user asks why a brand is visible or invisible in AI answers.
- The user asks which competitors matter or which pending competitors to
  confirm.
- The user asks which citation domains influence answers.
- The user asks for a data-backed content, PR, SEO, or GEO recommendation.
- The user asks for analytics refresh or generated insights.

## Do Not Use

- For prompt creation; use `geo-prompt-ops`.
- For final report persistence; use `geo-reporting`.
- For row-level prompt-response evidence alone; use `geo-run-analysis`.
- For broad analytics refreshes unless the user wants current data.

## Task Router

| User Goal | Command Path | Notes |
| --- | --- | --- |
| Understand visibility | `geo analytics overview --section visibility --section kpis` | First stop for current state. |
| Refresh analytics | `geo analytics refresh` | No dry-run support; confirm intent before running. |
| Generate insights | `geo analytics generate-insights --async` | Track task status. |
| Review pending competitors | `geo competitors list --status pending --limit 100` | Confirm obvious true competitors. |
| Import/export competitors | `geo competitors export`, `geo competitors import --from-file competitors.csv --dry-run` | Dry-run imports. |
| Merge duplicates | `geo competitors merge --from <source_id> --into <primary_id> --dry-run` | Ask before `--yes`. |
| Compare citation domains | `geo citations compare --days 30 --limit 50` | Use for contested domains. |
| Find emerging domains | `geo citations emerging --days 7 --order-by trend` | Good for recent movement. |
| Inspect authority | `geo citations top-authority --limit-per-category 10` | Good for content/PR planning. |
| Classify a domain | `geo citations classify example.com` | Use for one-off domain understanding. |

## Decision Workflow

Read `references/decision-sop.md` for the full method when the user asks for a
recommendation.

Short version:

1. Check overview visibility and KPIs.
2. Review pending competitors and confirm obvious matches.
3. Compare citation domains for brand-led, competitor-led, and contested areas.
4. Inspect emerging and high-authority domains.
5. Pull prompt-level run evidence with `geo-run-analysis` when making claims.
6. Translate findings into actions: prompt coverage, content gaps, citation
   targets, competitor hygiene, or analytics refresh.
7. Save the recommendation with `geo-reporting` when the user wants a durable
   artifact.

## Safe Defaults

- Use `--dry-run` for competitor import, merge, and restore.
- Analytics refresh has no dry-run support; confirm intent before running
  `geo analytics refresh`.
- Use `--yes` for merge only after the user approves the exact source and
  destination competitors.
- Widen `--days` from 30 to 90 only when current data is too sparse.
- Avoid long-tail admin endpoints unless the manifest lacks a first-class
  command.

## Recovery

| Error Or Symptom | Recovery |
| --- | --- |
| `permission_denied` | Check `competitors:*`, `citations:read`, or `analytics:*` scopes. |
| Empty citation lists | Widen `--days`, verify brand, then inspect run exports. |
| Too many pending competitors | Sort/filter, confirm only obvious matches, leave ambiguous items pending. |
| Merge target unclear | Stop and ask the user; do not guess primary competitor. |
| Analytics refresh queued | Poll task status through the returned status URL or raw API if required. |
| Recommendation lacks evidence | Pull run export through `geo-run-analysis` before finalizing. |

## References

- Read `references/decision-sop.md` for evidence quality, competitor hygiene,
  citation interpretation, and recommendation structure.
