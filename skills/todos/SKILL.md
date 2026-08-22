---
name: todos
description: Use when the user asks what todos exist, wants a backlog issue summarized or reviewed, or asks to complete tracked work in GitHub Issues.
---

# Todos

GitHub Issues with the configured todo label are the backlog. Run `$todo-setup` when `.agents/todo.json` is missing, incomplete, or has `setupComplete: false`. The project supplies priority metadata.

## Preflight

1. Read `.agents/todo.json` and run setup if it is missing or incomplete.
2. Run `gh auth status`. Stop with its remediation if authentication fails.
3. Run `gh repo view --json nameWithOwner,url,owner` and verify the configured repository.
4. Use the requested project, then the saved default. If neither exists, ask for a project for this operation and do not save it unless asked.
5. Validate the project and read its fields with `gh project view <number> --owner <owner> --format json` and `gh project field-list <number> --owner <owner> --format json`.
6. Require the configured single-select priority field and its current options. Never use labels for priority.
7. If the configured todo label is missing, stop and direct the user to setup.

## List todos

1. Query the project with `gh project item-list <number> --owner <owner> --limit 100 --format json`.
2. Restrict results to open todo issues in the current repository.
3. Sort by the configured priority policy. The default is the project's current option order, then issue number. Put items without project priority last and mark them as invalid backlog data.
4. Present a numbered list containing issue number, priority, title, and URL.

Include closed issues only when the user asks for history or completed work.

## Review a todo

1. Prefer an issue number or URL.
2. For a text description, search with `gh issue list --state all --label <todo-label> --search "<terms> in:title,body" --limit 100`.
3. If more than one plausible issue matches, show them and ask the user to choose.
4. Read the selected issue with `gh issue view <number> --json number,title,body,state,labels,comments,url`.
5. Match its URL to the selected project's item list and read priority from that project item. If it is absent, say that it is not in the selected project instead of inferring priority from labels.
6. Summarize its objective, project priority, state, and relevant discussion without inventing progress.

## Complete a todo

1. Identify one open todo issue confidently.
2. Confirm implementation evidence from the current work or ask the user for it. Intent alone is not evidence.
3. Post a concise evidence comment with `gh issue comment <number> --body "<completion evidence>"`.
4. Close it with `gh issue close <number> --reason completed`.
5. Re-read the issue and report its number, title, closed state, and URL.

Keep labels when closing an issue. They preserve backlog history.

## Safety

- Never close an ambiguous issue.
- Never claim completion without evidence and a successful close command.
- Never close an issue as `not planned` when the work was completed.
- Never treat an issue label as priority.
- Never edit a local todo file or create a Git commit for an issue operation.
