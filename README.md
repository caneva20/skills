# Agent skills

Tool-agnostic skills for coding agents.

## Included skills

- `brainstorming`: Resolve material design choices before implementation.
- `concise-writing`: Write direct, standalone prose.
- `full-superpowers`: Apply the strict Superpowers workflow only when explicitly requested.
- `systematic-debugging`: Find evidence for a root cause before changing code.
- `todo`: Draft and create GitHub backlog issues with project priority.
- `todo-migrate`: Move a legacy TODO.md backlog into GitHub Issues.
- `todo-setup`: Configure the GitHub todo workflow for a repository.
- `todos`: List, review, and complete GitHub todo issues.
- `unslop`: Remove predictable AI writing patterns while preserving meaning and voice.
- `verification-before-completion`: Verify completion claims with fresh evidence.
- `work-intake`: Check requirements against the current repository before non-trivial work.

Each skill lives in its own directory and uses a `SKILL.md` file with standard
frontmatter. Copy or reference the `skills/` directory from your agent tool.

## Source of truth

This repository is the source of truth for the included skills. Add or update
skills under `skills/`, then commit and push the changes. Agent-specific skill
directories are runtime installations or copies and are not canonical.

## External skills

These skills are not hosted here. Install them from their upstream projects:

- [`impeccable`](https://github.com/pbakaus/impeccable): frontend design guidance and tooling, Apache-2.0.
- [`interview-me`](https://github.com/addyosmani/agent-skills/tree/main/skills/interview-me): intent discovery, MIT.

## License

Original material in this repository is licensed under the MIT License. See
[`LICENSE`](LICENSE) and [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md) for
attribution and source details for adapted material.
