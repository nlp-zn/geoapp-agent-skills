# Schema And Manifest

Agents should inspect the local command manifest before guessing command shapes.

```bash
geo schema commands
geo schema runs export
geo schema agent-skills install
```

The manifest includes:

```text
command
risk
required_scopes
requires_brand
supports_dry_run
requires_yes
examples
common_errors
```

Backend OpenAPI lookup:

```bash
geo schema refresh
geo schema api POST /api/v1/prompts
geo schema clear
```

`geo doctor` checks backend meta compatibility against the CLI version and warns
when the server recommends a newer CLI.
