---
name: work-intake
description: Use before starting non-trivial work, especially tasks with acceptance criteria (ACs), issues, specs, related documentation or tests, behavior changes, or unclear requirements. Reviews requirements against the current code and invokes interview-me for material ambiguity.
---

# Work Intake

Check that the requested work still matches the repository before planning or implementation.

## When to Use

Use this skill when work has acceptance criteria, changes behavior, spans responsibilities, requires design or debugging, or depends on an issue or spec. Skip it for routine, self-contained changes whose outcome is explicit.

## Intake

1. Read the current request, its acceptance criteria, linked issues or specs, and relevant repository documentation and tests.
2. Inspect the current code involved. Treat the repository as authoritative for what previous work has already changed.
3. Revalidate each acceptance criterion against the current code. Identify criteria that are already satisfied, stale, contradictory, ambiguous, or based on an incorrect premise.
4. Check whether related documentation, tests, code, and the user's request disagree. Do not silently choose one source.
5. If a material uncertainty remains, give the user the context needed to decide, then ask one focused question. Load and apply `interview-me` (or ask one focused question if it is unavailable) when the underlying intent, success condition, scope, or tradeoff is unclear.
6. Challenge the user's premise only when code, requirements, risks, or tradeoffs provide evidence. State that evidence directly.
7. Once the criteria are current, run `work-decomposition` before planning or
   implementation. If it finds a reusable part, follow its proposal and
   confirmation gate. If it finds no useful split, proceed with the task.

Keep the intake proportional to the work. Do not search unrelated parts of the repository or turn a clear task into a requirements exercise.

## Communication

Load and apply `unslop` and `concise-writing`. Report only findings that change the work, expose a decision, or prevent a wrong implementation.
