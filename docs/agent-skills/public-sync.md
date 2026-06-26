# Public Agent Skills Sync

Geo App keeps CLI source in the private monorepo and publishes the CLI through
npm/GitHub Releases. The public open-source surface for user-owned agents is the
agent skills, CLI usage docs, and examples.

Use this sync path to mirror the public-safe subset into a dedicated repository
such as `nlp-zn/geoapp-agent-skills`.

## Source Of Truth

The private monorepo remains the source of truth:

```text
packages/agent-skills/README.md
packages/agent-skills/skills
docs/agent-skills
selected public-safe docs/cli files
examples/agent-skills
```

The public repository is a curated mirror. It should not receive direct edits to
generated files unless the same change is also made in the private source.

## Local Commands

Validate the export without writing:

```bash
pnpm sync:agent-skills:public
```

Write into a local checkout without committing:

```bash
pnpm sync:agent-skills:public -- --public-dir ../geoapp-agent-skills --write
```

Write, commit, and push:

```bash
pnpm sync:agent-skills:public -- \
  --public-dir ../geoapp-agent-skills \
  --target-branch main \
  --push
```

`--push` implies `--write`. The script exits without committing when there are no
public changes.

## GitHub Actions

The manual workflow `.github/workflows/sync-agent-skills-public.yml` runs the
same script.

Inputs:

- `public_repository`: public mirror repository, for example
  `nlp-zn/geoapp-agent-skills`.
- `public_branch`: target branch in the public repository.
- `dry_run`: defaults to true and does not checkout or push the public repo.

For real pushes, configure `PUBLIC_REPO_SYNC_SSH_KEY` as a GitHub Actions
secret. Use a dedicated deploy key instead of a broad personal token:

1. Generate an SSH key pair for this sync job.
2. Add the public key to the public mirror repository as a deploy key with
   write access.
3. Store the private key in the private source repository secret
   `PUBLIC_REPO_SYNC_SSH_KEY`.

The key should have write access only to the public mirror repository.

## Safety Contract

The sync script is allowlist-based. It only manages:

```text
README.md
skills
docs/agent-skills
docs/cli/auth.md
docs/cli/command-reference.md
docs/cli/errors.md
docs/cli/raw-api.md
docs/cli/schema-and-manifest.md
docs/cli/security-and-audit.md
examples
PUBLIC_SYNC_MANIFEST.json
```

It rejects files outside those targets, disallows unexpected file types, caps
file size, and scans for likely secret material such as private keys, PATs, npm
tokens, OpenAI-style keys, database URLs with passwords, and assigned secret
values.

Private-development CLI docs such as source build instructions, local aliases,
and release-process notes are intentionally excluded from the public mirror.

The sync must never include:

```text
apps/backend
apps/frontend
packages/cli/src
infra
.env*
artifacts
docs/releases/internal evidence
database migrations
backend server-side skills runtime internals
```

## Versioning

Keep agent skill metadata aligned with the published CLI:

- `min_cli_version` should be the oldest CLI version that can safely run the
  skill.
- `tested_cli_version` should match the CLI version used by release validation.
- Run the public sync after CLI publication verification when a release changes
  skill behavior or CLI command contracts.
