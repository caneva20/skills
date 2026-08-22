---
name: todo-setup
description: Use on first todo workflow use, or when the todo configuration is missing, incomplete, or stale.
---

# Todo setup

Configure the repository's GitHub todo workflow in `.agents/todo.json`. This skill alone may create missing todo labels, with user approval.

## When setup runs

Run setup before any todo workflow when `.agents/todo.json` is missing, incomplete, or has `setupComplete: false`. Users can also invoke `$todo-setup` to review or change the configuration.

Use only the current repository's configuration. Choose projects from GitHub, not from the repository name or another repository's settings.

## Setup workflow

1. Run `gh auth status`. Stop with its remediation if authentication fails.
2. Run `gh repo view --json nameWithOwner,url,owner`. Confirm it matches `repository` in `.agents/todo.json`; create the file with the current repository when it is missing.
3. Read the configuration. On the first run, ask every question below, including questions with defaults. On later runs, ask only about missing or changed values.
4. List projects with `gh project list --owner <repository-owner> --format json`.
5. Ask the user to choose a saved default project or no default. Without a default, each operation needs an explicit project.
6. Validate the project with `gh project view <number> --owner <owner> --format json`.
7. Read its fields with `gh project field-list <number> --owner <owner> --format json`. Ask which single-select field supplies priority. Prefer `Priority`.
8. Show the field's current options and ask what each means. Store meanings by name. Re-read option IDs and order during every operation.
9. Ask how new-todo priority should work:
   - `ask`: choose priority for every new todo.
   - `default`: use the configured default unless the user overrides it.
10. Ask for the default when the selected policy needs one. Ask separately for the migration default.
11. Ask which label marks a backlog issue. Suggest `todo`.
12. Ask how to choose contextual labels. Store each label with its name and use rule.
13. Run `gh label list --limit 100`. Show missing labels and ask before creating them. Never create priority labels.
14. Confirm the legacy source path and marker prefix. Default to `TODO.md` and `infinity-climber-todo-legacy`.
15. Write `.agents/todo.json` with `setupComplete: true`. Store no project option IDs because GitHub can change them.
16. Re-read and validate the JSON. Report the project, priority policy, labels, and migration settings.

Ask one question at a time. Reuse valid settings. Tell the user before changing the project or policy.

## Configuration shape

```json
{
  "version": 1,
  "repository": "owner/repository",
  "setupComplete": true,
  "project": { "owner": "owner", "number": 1 },
  "priority": {
    "field": "Priority",
    "meanings": { "P0": "urgent production work" },
    "newTodo": "ask",
    "default": null,
    "migrationDefault": null,
    "listOrder": "project-option-order"
  },
  "labels": {
    "todo": "todo",
    "contextual": [
      { "name": "game:infinity-climber", "when": "Specific to this game" }
    ]
  },
  "migration": {
    "source": "TODO.md",
    "markerPrefix": "infinity-climber-todo-legacy"
  }
}
```

The issue format, duplicate checks, approval gates, and evidence requirements are fixed workflow rules.

## Safety

- Keep GitHub Issues as the backlog source.
- Store priority in the project field, never in a label.
- Store no project or option IDs as authoritative configuration.
- Get approval before creating labels.
- Setup creates no issues or commits.

## Response pattern

1. State which settings are missing or changing.
2. Ask one focused question.
3. After completion, report the saved configuration and any labels created.
