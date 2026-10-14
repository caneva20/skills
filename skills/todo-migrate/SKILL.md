---
name: todo-migrate
description: Use when a file-backed TODO.md backlog must be migrated into GitHub Issues, resumed after an interrupted import, verified, or retired safely.
---

# Todo migrate

Move a legacy backlog into GitHub Issues while preserving its state and provenance. Run the `todo-setup` skill when `.agents/todo.json` is missing, incomplete, or has `setupComplete: false`. If the source file does not exist, report that there is nothing to migrate.

## Read-only GitHub delegation

Dispatch `github-todo-worker`, when the agent tool provides it, for every
read-only `gh` command below; otherwise run them directly. Batch the
known queries needed before each parent decision point into one task. Give the
worker the configured repository, selected project, source path, and exact data
needed. The worker may read the source to inventory entries and match markers.

The parent agent asks clarification and approval questions, drafts issues,
creates or updates GitHub data, and deletes the source only after approval.
Dispatch the worker again for migration verification reads.

## Preflight and setup

1. Read `.agents/todo.json` and run setup if it is missing or incomplete.
2. Run `gh auth status`. Stop with its remediation if authentication fails.
3. Run `gh repo view --json nameWithOwner,url,owner` and verify the configured repository.
4. Use the requested project, then the saved default. If neither exists, ask for a project for this migration and do not save it unless asked.
5. Validate the project and read its fields. Require the configured single-select priority field and its current options.
6. Read the source file. Parse active and completed entries in source order. Preserve source IDs or assign stable IDs such as `T001` by source order.
7. Run `gh label list --limit 100`. If the todo or an applicable contextual label is missing, stop and direct the user to setup.

Use current project options, not legacy priority names. Use a source-backed option or the configured migration default. If neither is valid, ask once for a current option.

## Provenance and idempotency

Every migrated issue body ends with a deterministic marker using the configured prefix and legacy ID:

```html
<!-- infinity-climber-todo-legacy:T001 -->
```

Use the configured prefix in place of the example. Fetch bodies with `gh issue list --state all --label <todo-label> --limit 200 --json number,body,state,labels,url`. Reuse one exact marker match, stop on duplicates, and create an issue only when no match exists.

## Active items

For each active item, in source order:

1. Summarize the objective and affected area.
2. Ask one focused question at a time until the objective is clear.
3. Select a current project priority according to the configured creation policy, allowing an explicit override.
4. Draft a title and body with `Objective`, `Context`, `Legacy ID`, source dates when available, and the provenance marker.
5. Show the complete draft, labels, destination project, and project priority. Require explicit approval.
6. Create the issue with the configured todo label, add it to the selected project, and set its priority.
7. Verify the project item and report its number and URL.
8. If refinement proves the item obsolete, require approval before closing it as `not planned`.

Do not design the feature during migration.

## Completed items

For each completed item, in source order:

1. Select a current project priority from source evidence or the configured migration default.
2. Preserve the outcome as `Objective`.
3. Record the legacy ID, source dates when available, the selected-priority note, and the provenance marker.
4. Create the issue with the configured todo label, add it to the selected project, set its priority, and close it with `gh issue close <number> --reason completed`.

Completed history does not require questions or draft approval, but it must pass the idempotency checks.

## Verification and source removal

Before deleting the configured source file, query all migrated issues and verify:

- Every source entry has exactly one unique legacy marker.
- Every active entry has its individually approved final state.
- Every completed entry identifies an issue closed as `completed`.
- Every migrated issue has the configured todo label, belongs to the selected project, and has one current priority option set.
- No migrated issue receives a priority label.
- Every available source date and legacy ID is preserved.
- No legacy ID maps to multiple issues.

On failure, report the mismatch and keep the source file. On success, show the results and get approval before deletion. After deletion, run `git diff --check` and `git status --short`. Commit only when explicitly asked.

## Safety

- Never fall back to a local todo file as the active backlog.
- Never create a duplicate automatically.
- Never delete the source before verification and approval.
- Never create or apply a priority label.
- Never create a Git commit for a migration.
