---
name: todo
description: Use when the user asks to add a todo, capture follow-up work, note an idea for later, or turn feedback into a GitHub backlog issue.
---

# Todo

Draft every requested todo before creating it. GitHub Issues with the configured todo label are the backlog. Run `$todo-setup` when `.agents/todo.json` is missing, incomplete, or has `setupComplete: false`.

## Read-only GitHub delegation

Dispatch `github-todo-worker` for every read-only `gh` command below. Batch the
known queries needed before each parent decision point into one task. Give the
worker the configured repository, selected project, and exact data needed.

The parent agent reads configuration, asks questions, interprets ambiguous
results, drafts issues, gets approval, and runs every write command. Dispatch
the worker again for the final verification reads.

## Preflight

1. Read `.agents/todo.json` and run setup if it is missing or incomplete.
2. Run `gh auth status`. Stop with its remediation if authentication fails.
3. Run `gh repo view --json nameWithOwner,url,owner` and verify the configured repository.
4. Use the requested project, then the saved default. If neither exists, ask for a project for this operation and do not save it unless asked.
5. Validate the project with `gh project view <number> --owner <owner> --format json`.
6. Read its fields with `gh project field-list <number> --owner <owner> --format json`. Require the configured single-select priority field and its current options.
7. Check the configured todo and applicable contextual labels. If a required label is missing, stop and direct the user to setup. Do not create labels here.

Use the project field for priority, never a label. With policy `ask`, ask for a current option. With policy `default`, use the configured option unless the user overrides it. Run setup if the option or default no longer exists.

## Workflow

1. Search open and closed todo issues for duplicates with `gh issue list --state all --label <todo-label> --search "<key terms> in:title,body" --limit 100`.
2. Check the same results for related work and dependencies. Inspect plausible issues' `blockedBy`, `blocking`, `parent`, and `subIssues` fields when available. Search broader terms when the objective spans more than one area.
3. Show plausible duplicates, related issues, and dependencies before drafting. Ask whether to update or use an existing issue when needed.
4. If an existing issue may block the new work, or the new work may block an existing issue, identify that relationship explicitly. Do not infer a dependency from similar wording alone. Ask one question when the relationship or target issue is uncertain.
5. State the objective, affected area, confirmed related work, dependencies, project, contextual labels, and priority.
6. Ask one question at a time when missing information changes the objective, labels, project, priority, or dependency relationship. Do not design the feature here.
7. Draft one issue in the format below. Include confirmed dependency issue numbers and the relationship in `## Context`.
8. Show the title, body, labels, project, and priority. Get explicit approval before creating it.
9. Create it with `gh issue create --title "<title>" --body "<body>" --label <todo-label>`, adding applicable contextual labels.
10. Add the issue with `gh project item-add <number> --owner <owner> --url <issue-url> --format json` and capture the project item ID.
11. Set the priority with `gh project item-edit --id <item-id> --project-id <project-id> --field-id <priority-field-id> --single-select-option-id <priority-option-id>`.
12. Re-read the project items, verify the issue and priority, and report the issue number and URL.

If the user requests no project, skip project validation and project-item steps 10-11. Verify the created issue and report its URL without claiming a project priority. If project addition or priority setting fails after issue creation, report the issue URL and the partial result. Do not create a replacement.

## Issue format

```markdown
## Objective

<One concise outcome statement.>

## Context

<Only the context and affected area needed to understand the work.>
```

Use a concise outcome-oriented title. Keep the body specific enough for later refinement without designing the solution.

## Safety

- Never create a duplicate automatically.
- Never create an issue before draft approval.
- Never create or apply a priority label.
- Never create a Git commit for an issue operation.
