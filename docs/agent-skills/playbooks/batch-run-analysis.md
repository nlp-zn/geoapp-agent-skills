# Batch Run Analysis Playbook

Use `geo-run-analysis`.

Export analysis data:

```bash
geo runs export --since 7d --include response,citations,mentions,metrics,errors
```

Analyze prompt performance, provider errors, brand mentions, competitor
mentions, citation authority, and repeated failure modes. Save a durable report
with `geo-reporting`.
