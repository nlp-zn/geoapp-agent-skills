# Run Export Report Playbook

Use this playbook when an agent needs to export prompt-run data and save a
durable report back to Geo App.

## Skills

- `geo-shared`
- `geo-run-analysis`
- `geo-reporting`

## Commands

```bash
geo doctor
geo auth status
geo brand current
geo runs export --since 7d --include response,citations,mentions,metrics,errors --format jsonl > runs.jsonl
```

## Report Structure

```markdown
# Weekly GEO Analysis - [Brand] - [Date Range]

## Executive Summary

## Key Findings

## Evidence

## Decisions

## Risks And Unknowns

## Next Actions
```

## Save

```bash
cat report.md | geo reports create --title "Weekly GEO Analysis" --content -
```

## Quality Bar

- Cite the export command and date window.
- Cite run ids or prompt ids for prompt-level claims.
- Cite citation domains for citation claims.
- Keep raw exports out of the main body unless the user asks for appendices.
- Return the created report id.
