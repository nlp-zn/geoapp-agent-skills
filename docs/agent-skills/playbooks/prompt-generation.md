# Prompt Generation Playbook

Use `geo-prompt-ops`.

1. Run `geo doctor`.
2. Preview candidates:

```bash
geo prompts generate --keywords "brand comparison,pricing" --count 20
```

3. Apply only after review:

```bash
geo prompts generate --keywords "brand comparison,pricing" --count 20 --apply --dry-run
geo prompts generate --keywords "brand comparison,pricing" --count 20 --apply
```

4. Batch-run selected prompts:

```bash
geo prompts batch-run --feature Price --concurrency 5 --watch
```
