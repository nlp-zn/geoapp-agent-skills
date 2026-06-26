---
name: geo-reporting
version: 0.2.0
description: Turn Geo CLI analysis into durable Markdown reports saved back to Geo App. Use this skill whenever the user wants a weekly report, executive summary, data-backed recommendation, decision memo, operational review, or any final analysis artifact created from prompts, runs, analytics, competitors, citations, or server-skill results.
min_cli_version: 0.1.0
tested_cli_version: 0.1.0
required_scopes:
  - reports:read
  - reports:write
  - runs:read
  - analytics:read
---

# Geo Reporting

Use this skill to convert analysis into a saved Geo App report.

## Required Shared Context

Run `geo-shared` first. If report evidence comes from runs or analytics, run
the relevant analysis skill before writing.

```bash
geo doctor
geo auth status
geo brand current
geo schema reports create
```

## When To Use

- The user asks for a saved report, weekly analysis, decision memo, or durable
  summary.
- The user wants recommendations from prompt runs, analytics, citations,
  competitors, or server-skill output.
- Another Geo skill produced evidence and the user wants it persisted.

## Do Not Use

- For raw exploration when the user only wants terminal output.
- For unsupported claims without run ids, export windows, prompt ids, analytics
  sections, or citation evidence.
- For creating prompt-run data; use `geo-prompt-ops` and `geo-run-analysis`
  first.

## Task Router

| User Goal | Command Path | Notes |
| --- | --- | --- |
| List saved reports | `geo reports list --limit 20` | Use before updating context. |
| Read a report | `geo reports get <report_id> --format markdown` | Markdown output is raw stdout. |
| Save new report from stdin | `cat report.md | geo reports create --title "<title>" --content -` | Preferred for generated reports. |
| Save from file | `geo reports create --title "<title>" --from-file report.md` | Use when file already exists. |
| Delete report | `geo reports delete <report_id> --dry-run`, then `--yes` | Ask user before `--yes`. |

## Report Workflow

1. Identify audience: operator, executive, content team, or engineering.
2. Gather evidence from prior commands. Do not invent report ids or metrics.
3. Draft the report using the structure in `references/report-contract.md`.
4. Keep raw exports out of the report unless the user asks for appendices.
5. Save through `geo reports create`.
6. Return the created report id and a short summary of what was saved.

## Required Report Shape

Use Markdown. For decision-oriented reports, include:

```markdown
# [Title]

## Executive Summary

## Key Findings

## Evidence

## Decisions

## Risks And Unknowns

## Next Actions
```

Read `references/report-contract.md` before writing weekly reports, executive
summaries, or recommendations that need a polished structure.

## Evidence Rules

- Cite run ids, prompt ids, export windows, analytics sections, or citation
  domains.
- Separate facts from interpretation.
- Say when a conclusion relies on a small sample.
- Include concrete next actions, owners, or follow-up commands when useful.
- Do not include raw credentials, tokens, or hidden CLI config.

## Recovery

| Error Or Symptom | Recovery |
| --- | --- |
| `missing_report_content` | Pass `--content -`, `--content @file`, or `--from-file`. |
| `permission_denied` | Token needs `reports:write`. |
| Markdown printed as JSON parse error | `geo reports get --format markdown` is raw stdout, not a JSON envelope. |
| Report too long | Save a concise report and attach raw exports only if the user requested appendices. |
| Delete requested | Dry-run first and ask the user before `--yes`. |

## References

- Read `references/report-contract.md` for report templates, evidence quality,
  and final response requirements.
