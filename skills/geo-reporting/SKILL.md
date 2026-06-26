---
name: geo-reporting
description: Turn Geo App CLI analysis into durable markdown reports saved back to the platform.
min_cli_version: 0.1.0
tested_cli_version: 0.1.0
required_scopes:
  - reports:read
  - reports:write
  - runs:read
  - analytics:read
---

# Geo Reporting

Use this skill when the user wants a saved report, weekly analysis, operational
recommendations, or a durable summary of run/analytics data.

## Commands

List reports:

```bash
geo reports list --limit 20
```

Read a report body:

```bash
geo reports get <report_id> --format markdown
```

Create a report from stdin:

```bash
cat report.md | geo reports create --title "Weekly GEO Analysis" --content -
```

Create from a markdown file:

```bash
geo reports create --title "Weekly GEO Analysis" --from-file report.md
```

Delete only after approval:

```bash
geo reports delete <report_id> --dry-run
geo reports delete <report_id> --yes
```

## Report Structure

Use markdown:

```markdown
# Title

## Executive Summary

## Evidence

## Decisions

## Risks

## Next Actions
```

## Safe Defaults

- Cite run ids, prompt ids, and export windows.
- Keep raw exports out of the report unless the user asks for appendices.
- State uncertainty when analysis relies on small samples.
- Save final analysis through `geo reports create`; do not invent report ids.

## Recovery

- `missing_report_content`: pass `--content -`, `--content @file`, or `--from-file`.
- `permission_denied`: token needs `reports:write`.
- Markdown output is raw stdout; do not parse it as a JSON envelope.
