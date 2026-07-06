# Geo Agent Skills Manual Test Plan

Use this plan to manually test whether a user-owned agent can install Geo CLI
skills, operate the `geo` CLI, and turn real prompt, run, competitor, citation,
skill, and report data into useful decisions.

## Goal

Validate the end-to-end operator experience:

1. A tester installs the published CLI.
2. The tester authenticates against the hosted Geo API.
3. The tester installs open-source agent skills into Codex or Claude Code.
4. The agent reads the skills and completes real operating tasks.
5. The tester records whether the result is usable by an operations user.

This is a human judgment pass, not an automated benchmark suite.

## Current Beta Constraint

`geoapp-cli@beta` does not yet bundle the agent skill files in the npm tarball.
For this beta test, install the CLI from npm, but use the public skills
repository as the skills source:

```bash
git clone https://github.com/nlp-zn/geoapp-agent-skills.git ~/geoapp-agent-skills
export GEO_AGENT_SKILLS_SOURCE_DIR=~/geoapp-agent-skills/skills
```

If the repository already exists:

```bash
git -C ~/geoapp-agent-skills pull --ff-only origin main
export GEO_AGENT_SKILLS_SOURCE_DIR=~/geoapp-agent-skills/skills
```

Remove this workaround after the CLI package either bundles skills or fetches
them from the public repository.

## Test Matrix

| Target | Install Path | Required For This Pass |
| --- | --- | --- |
| Codex | `~/.codex/skills` | Yes |
| Claude Code | `~/.claude/skills` | Recommended |
| Cursor | Project rules from `docs/agent-skills/cursor-template.md` | Optional |

Use the default install path for real agent testing. Environment overrides such
as `GEO_AGENT_SKILLS_CODEX_DIR` and `GEO_AGENT_SKILLS_CLAUDE_DIR` are useful
for dry-run or isolated file checks, but most agents will not load those custom
directories unless separately configured.

## Preconditions

- The tester has Node.js and npm available.
- The tester can authenticate with browser/device login, or has a scoped PAT.
- The tester has access to at least one brand with useful prompt-run data.
- The tester can run Codex or Claude Code locally after installing skills.

Browser/device login is recommended for the first manual pass. If using PATs,
use `geo agent-skills list` and `geo schema <command>` to confirm required
scopes for the chosen workflows.

## Setup Checklist

### 1. Install The CLI

```bash
npm install -g geoapp-cli@beta
geo --version
```

Temporary no-install check:

```bash
npx geoapp-cli@beta --version
```

Pass criteria:

- `geo --version` prints a version.
- `which geo` points to the expected npm global executable.

### 2. Verify Hosted API

```bash
geo config get
geo doctor
```

Pass criteria:

- `geo config get` resolves `https://api.prompt-insights.com` unless the tester
  intentionally set a local or self-hosted override.
- `geo doctor` reaches the API.
- The response does not report an incompatible CLI/server version.

### 3. Authenticate

Browser login:

```bash
geo auth login
geo auth status
geo auth whoami
```

PAT path:

```bash
geo auth token set <pat>
geo auth status
geo auth whoami
```

Pass criteria:

- `geo auth status` reports an authenticated profile.
- `geo auth whoami` identifies the expected user.

### 4. Select Brand Context

```bash
geo brand list
geo brand use <brand_id>
geo brand current
geo doctor
```

Pass criteria:

- `geo brand current` returns the intended brand.
- Brand-scoped commands no longer fail with `missing_brand`.

### 5. Install Skills Into Codex

```bash
export GEO_AGENT_SKILLS_SOURCE_DIR=~/geoapp-agent-skills/skills
geo agent-skills list
geo agent-skills install --agent codex --dry-run
geo agent-skills install --agent codex
geo agent-skills doctor --agent codex
```

Expected target files:

```text
~/.codex/skills/geo-shared/SKILL.md
~/.codex/skills/geo-prompt-ops/SKILL.md
~/.codex/skills/geo-run-analysis/SKILL.md
~/.codex/skills/geo-reporting/SKILL.md
~/.codex/skills/geo-skill-execution/SKILL.md
~/.codex/skills/geo-competitor-citation-analysis/SKILL.md
```

Pass criteria:

- `geo agent-skills list` returns six skills.
- `geo agent-skills doctor --agent codex` returns `ok: true`.
- Restart Codex and confirm the skills are visible to the agent.

### 6. Install Skills Into Claude Code

```bash
export GEO_AGENT_SKILLS_SOURCE_DIR=~/geoapp-agent-skills/skills
geo agent-skills install --agent claude --dry-run
geo agent-skills install --agent claude
geo agent-skills doctor --agent claude
```

Expected target files:

```text
~/.claude/skills/geo-shared/SKILL.md
~/.claude/skills/geo-prompt-ops/SKILL.md
~/.claude/skills/geo-run-analysis/SKILL.md
~/.claude/skills/geo-reporting/SKILL.md
~/.claude/skills/geo-skill-execution/SKILL.md
~/.claude/skills/geo-competitor-citation-analysis/SKILL.md
```

Pass criteria:

- `geo agent-skills doctor --agent claude` returns `ok: true`.
- Restart Claude Code and confirm the skills are visible to the agent.

## Manual Test Tasks

Record the exact user prompt, agent commands, outputs used for decisions, and
final answer quality for each task.

### Task 1: Setup And Recovery

Prompt:

```text
Use the Geo skills. Check my Geo CLI setup, authentication, brand context, and
available command schema. If anything is missing, explain the exact command I
should run and stop before making changes.
```

Expected agent behavior:

- Uses `geo-shared`.
- Runs or asks to run `geo doctor`, `geo auth status`, `geo brand current`, and
  relevant `geo schema` commands.
- Does not guess a brand.
- Gives actionable recovery steps for auth, API URL, or brand failures.

Pass criteria:

- The agent reaches a correct setup diagnosis.
- The agent does not expose tokens or credential file contents.
- The agent stops before writes when setup is incomplete.

### Task 2: Prompt Generation And Batch Run

Prompt:

```text
Use the Geo skills. Generate 10 prompts for pricing, alternatives, and vendor
comparison for the current brand. Preview them first, explain which ones are
weak or duplicative, ask me before applying, then batch-run the approved Price
or Alternatives prompts with watch enabled.
```

Expected agent behavior:

- Uses `geo-prompt-ops` after `geo-shared`.
- Runs a preview before persistence.
- Uses `--dry-run` for writes where supported.
- Asks before applying generated prompts or starting broad runs.
- Captures run IDs, status URLs, stream URLs, or event URLs.

Pass criteria:

- Generated prompts are specific to the brand and decision topic.
- The agent does not persist prompts without approval.
- Batch-run output is parseable and includes follow-up run IDs.

### Task 3: Run Export, Analysis, And Report

Prompt:

```text
Use the Geo skills. Export the last 7 days of prompt runs with responses,
citations, mentions, metrics, and errors. Identify the top visibility changes,
cite the evidence you used, and save a concise Markdown report in Geo App.
```

Expected agent behavior:

- Uses `geo-run-analysis` and `geo-reporting`.
- Exports run data with a focused include list.
- Distinguishes missing data from weak performance.
- Creates a report only after producing an evidence-backed summary.

Pass criteria:

- The answer names the data window and brand.
- Claims are tied to run responses, citations, mentions, metrics, or errors.
- A saved report is created with a useful title and Markdown content.
- The final answer includes the report ID or URL when available.

### Task 4: Competitor And Citation Decision

Prompt:

```text
Use the Geo skills. Explain why the current brand is underperforming or
overperforming versus competitors. Review pending competitors, compare citation
domains from the last 30 days, identify emerging domains, and recommend the
next three actions for operations.
```

Expected agent behavior:

- Uses `geo-competitor-citation-analysis`.
- Pulls analytics, competitor, and citation data before recommending actions.
- Confirms only obvious competitors.
- Does not run `geo analytics refresh` unless the user explicitly asks for a
  refresh, because that command has no dry-run mode.

Pass criteria:

- Recommendations are specific and decision-oriented.
- The agent separates citation gaps, prompt coverage gaps, and competitor
  hygiene issues.
- The agent asks before competitor writes or analytics refresh.

### Task 5: Server-Side Skill Execution

Prompt:

```text
Use the Geo skills. List available Geo server-side skills, explain which ones
are safe to run for this brand, inspect the required input for one useful
skill, ask before execution, then run it only after approval and monitor the
result.
```

Expected agent behavior:

- Uses `geo-skill-execution`.
- Clearly distinguishes `geo skills` server-side skills from `geo agent-skills`.
- Inspects skill details and input requirements before execution.
- Uses watch/status commands after execution.

Pass criteria:

- The agent does not confuse agent skills with backend skills.
- The agent asks before executing server-side work.
- Execution status and final result are captured.

## Test Record Template

Create one record per task:

```markdown
## Task <number>: <name>

- Date:
- Tester:
- Agent:
- CLI version:
- API URL:
- Brand:
- Skills source commit:
- Installed skills target:
- User prompt:
- Commands run:
- Data/artifacts produced:
- Pass/fail:
- Notes:
- Follow-up issue:
```

## Issue Severity

| Severity | Meaning | Example |
| --- | --- | --- |
| P0 | Blocks any useful testing | CLI cannot authenticate or skills cannot install |
| P1 | Unsafe or misleading behavior | Agent writes without approval or uses wrong brand |
| P2 | Workflow gap | Agent misses evidence, uses unsupported options, or gives vague recommendations |
| P3 | Polish | Wording, ordering, or small recovery improvements |

## Stop Conditions

Stop the test and file an issue when:

- The agent attempts to print or persist raw tokens.
- The agent operates on the wrong brand.
- A write command runs without explicit approval.
- A skill instructs the agent to use an unsupported CLI option.
- The CLI package cannot install skills without a documented workaround.

## Cleanup

After testing:

```bash
geo reports list --limit 20
geo prompts list --limit 50
```

Keep useful generated reports. Delete test prompts only after confirming their
IDs and using the command's dry-run or confirmation path where available.

## Product Follow-Ups To Watch

- Bundle skills in the npm package or make `geo agent-skills install` fetch from
  the public repository by default.
- Add a first-class Cursor install path if Cursor becomes a supported target.
- Add a lightweight test-result template or issue form for manual skill QA.
