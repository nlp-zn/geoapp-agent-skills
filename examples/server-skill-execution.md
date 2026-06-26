# Example: Execute A Server-Side Geo Skill

## User Prompt

```text
Run the server-side skill that summarizes recent prompt performance for this
brand. Watch it until it completes and save the result as a report if it
returns useful analysis.
```

## Skills That Should Trigger

- `geo-shared`
- `geo-skill-execution`
- `geo-reporting`

## Expected Agent Behavior

1. Run setup checks:

```bash
geo doctor
geo auth status
geo brand current
geo schema skills execute
```

2. Resolve the skill:

```bash
geo skills list
geo skills detail <slug>
geo skills config <slug>
```

3. Build a JSON object from the returned input contract.

4. Execute and watch:

```bash
geo skills execute <slug> --input @input.json
geo skills watch <execution_id>
```

5. If the output is report-worthy:

```bash
cat report.md | geo reports create --title "Server Skill Summary" --content -
```

## Quality Bar

- Do not guess input fields.
- Do not confuse `geo skills` with `geo agent-skills`.
- Capture execution id, task id, and status URL.
- Ask before cancelling an execution.
