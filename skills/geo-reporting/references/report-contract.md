# Report Contract

Use this reference when writing a durable Geo App report.

## Standard Report

```markdown
# [Clear Title]

## Executive Summary

2-4 bullets explaining the decision, movement, or operational state.

## Key Findings

- Finding with evidence.
- Finding with evidence.
- Finding with evidence.

## Evidence

| Evidence | Source |
| --- | --- |
| Export window | `geo runs export --since 7d ...` |
| Run ids | `<run_id>` |
| Prompt ids | `<prompt_id>` |
| Citation domains | `example.com` |

## Decisions

- Decision or recommendation.

## Risks And Unknowns

- Data gaps, low sample size, stale runs, missing providers, or ambiguous competitors.

## Next Actions

- Concrete action.
- Follow-up command or owner when useful.
```

## Weekly GEO Report

Use this when the user asks for a weekly or operating review.

```markdown
# Weekly GEO Analysis - [Brand] - [Date Range]

## Executive Summary

## Visibility Movement

## Prompt Themes

## Competitor Movement

## Citation Domains

## Operational Issues

## Recommended Actions

## Appendix
```

## Evidence Standards

- Include the export window or run id.
- Include prompt ids for prompt-level claims.
- Include citation domains/pages for citation claims.
- Include provider names when comparing providers.
- Say "insufficient evidence" when sample size is too small.

## Style

- Keep the executive summary short.
- Prefer direct recommendations over vague observations.
- Do not paste entire raw exports into the main body.
- Use tables only when they make comparison easier.
- Make uncertainty visible rather than burying it.

## Save Command

Preferred:

```bash
cat report.md | geo reports create --title "<title>" --content -
```

Alternative:

```bash
geo reports create --title "<title>" --from-file report.md
```
