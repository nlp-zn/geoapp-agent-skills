# Competitor And Citation Decision SOP

Use this reference when converting competitor, citation, and analytics data into
recommendations.

## Start With Current State

```bash
geo analytics overview --section visibility --section kpis
```

Capture:

- brand visibility
- movement direction
- prompt/category breakdown
- competitor share
- citation coverage
- freshness of data

## Competitor Hygiene

List pending competitors:

```bash
geo competitors list --status pending --limit 100
```

Confirm only obvious competitors:

```bash
geo competitors confirm <competitor_id>
```

For duplicates:

```bash
geo competitors merge --from <source_id> --into <primary_id> --dry-run
```

Ask the user before adding `--yes`. If the primary competitor is unclear, stop
and ask.

## Citation Analysis

Compare citation domains:

```bash
geo citations compare --days 30 --limit 50
```

Look for:

- domains that cite competitors but not the brand
- brand-led domains worth defending
- contested domains where both brand and competitors appear
- high-authority domains missing from brand answers
- emerging domains with recent trend movement

Additional commands:

```bash
geo citations emerging --days 7 --order-by trend
geo citations top-authority --limit-per-category 10
geo citations pages --search pricing --platform openai
geo citations classify example.com
```

## Connect To Prompt Evidence

Before making a strong recommendation, pull prompt-level evidence:

```bash
geo runs export --since 7d --include response,citations,mentions,metrics,errors
```

Tie citation gaps to actual prompt answers. A domain gap is more actionable when
it appears in prompts that matter to buyers.

## Recommendation Types

| Finding | Likely Action |
| --- | --- |
| Competitor appears in many answers but is pending | Confirm competitor and rerun analysis. |
| Competitor owns high-authority citation domains | Build content/PR plan targeting those source types. |
| Brand is mentioned but not recommended | Inspect answer themes and missing proof points. |
| Brand has citations but weak visibility | Check prompt intent and whether citations answer buyer questions. |
| Emerging domain is gaining share | Evaluate if it is worth outreach, content, or monitoring. |
| Data is stale or sparse | Refresh analytics or widen export window before deciding. |

## Final Answer Shape

For decisions, include:

- current state
- evidence boundary
- top 3 findings
- recommendation
- confidence level
- next command or owner
