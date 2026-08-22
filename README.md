# Agent skills

A small, tool-agnostic collection of skills for coding agents.

## Included skills

- `brainstorming`: Resolve material design choices before implementation.
- `concise-writing`: Write direct, standalone prose.
- `full-superpowers`: Apply the strict Superpowers workflow only when explicitly requested.
- `systematic-debugging`: Find evidence for a root cause before changing code.
- `unslop`: Remove predictable AI writing patterns while preserving meaning and voice.
- `verification-before-completion`: Verify completion claims with fresh evidence.
- `work-intake`: Check requirements against the current repository before non-trivial work.

Each skill lives in its own directory and uses a `SKILL.md` file with standard
frontmatter. Copy or reference the `skills/` directory from the agent tool you
use.

## External skills

These skills are useful but are not hosted here. Install them from their
upstream projects:

- [`impeccable`](https://github.com/pbakaus/impeccable): frontend design guidance and tooling, Apache-2.0.
- [`interview-me`](https://github.com/addyosmani/agent-skills/tree/main/skills/interview-me): intent discovery, MIT.

Keeping external skills out of this repository avoids stale copies and keeps
their licenses and update process with their authors.

## License

Original material in this repository is available under the MIT License. See
[`LICENSE`](LICENSE) and [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md) for
attribution and source details for adapted material.
