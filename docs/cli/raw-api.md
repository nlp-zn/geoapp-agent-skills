# Raw API

`geo api` is the fallback for GUI or backend capabilities that are not promoted
to first-class command groups.

```bash
geo api GET /health
geo api GET /api/v1/prompts
geo api POST /api/v1/prompts --data @prompt.json --dry-run
geo api PATCH /api/v1/example/:id --data '{"status":"active"}'
geo api DELETE /api/v1/example/:id --yes
```

Input sources:

```text
--data '{"json":true}'   inline JSON
--data @payload.json     file
--data -                 stdin
```

Protected headers such as `Authorization`, `X-Brand-Id`, and CLI metadata
headers are rejected from user input. Let the CLI construct those headers from
auth and brand context.

Raw write methods require the `api:raw` scope and are audited on the backend.
