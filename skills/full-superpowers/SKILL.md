---
name: full-superpowers
description: Use ONLY when the user explicitly requests the full, strict, or original Superpowers development workflow.
---

# Full Superpowers Workflow

Apply the original strict methodology only after explicit opt-in. Do not infer opt-in from task size or complexity.

## Workflow

1. **Brainstorm:** Explore project context, ask one question at a time, present meaningful alternatives, and obtain approval for the design in sections.
2. **Specify:** Write the approved design to `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`, self-review it, and obtain user review.
3. **Isolate:** Create and verify a Git worktree when permitted by the user's repository instructions.
4. **Plan:** Write `docs/superpowers/plans/YYYY-MM-DD-<topic>.md` with exact files, small tasks, tests, commands, expected results, and explicit handoff instructions.
5. **Execute:** Let the user choose subagent-driven or inline execution. Follow the plan task by task with review checkpoints.
6. **Use TDD:** For each behavior change, write and observe a failing test, implement the minimum fix, observe it passing, and refactor while green.
7. **Review:** Check specification compliance before code quality. Resolve material findings before continuing.
8. **Verify:** Run fresh, complete checks supporting every completion claim.
9. **Finish:** Present merge, pull request, keep, or cleanup options and follow the user's choice.

The strict workflow does not override higher-priority user or repository rules. Never commit, use subagents, create worktrees, push, or open a pull request without the permission those rules require.
