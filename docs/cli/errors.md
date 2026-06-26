# Errors And Exit Codes

Errors are structured JSON by default:

```json
{
  "ok": false,
  "error": {
    "code": "missing_brand",
    "message": "No brand context is configured.",
    "hint": "Run `geo brand use <brand_id>`, pass `--brand`, or set GEO_BRAND_ID.",
    "retryable": false,
    "param": "brand_id"
  }
}
```

## Exit Codes

| Code | Meaning |
| --- | --- |
| `0` | Success |
| `1` | Validation or usage error |
| `2` | Authentication required or refresh failed |
| `3` | Permission denied |
| `4` | Not found |
| `5` | Conflict or rate limit |
| `6` | Server or network error |
| `7` | Cancelled |

## Common Recovery

```text
missing_api_url      geo config set api_url <url> or pass --api-url
auth_required        geo auth login or geo auth token set <token>
missing_brand        geo brand use <brand_id> or pass --brand
permission_denied    check token scopes and brand allowlist
missing_export_boundary pass --since, --until, --run-id, or --prompt-id
confirmation_required pass --dry-run first, then --yes when intended
```
