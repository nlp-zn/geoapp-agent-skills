# Run Export Report Playbook

Use `geo-run-analysis` plus `geo-reporting`.

```bash
geo runs export --since 7d --include response,citations,mentions,metrics,errors > runs.json
cat report.md | geo reports create --title "Weekly GEO Analysis" --content -
```

Reports should include executive summary, evidence, decisions, risks, and next
actions.
