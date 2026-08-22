---
name: verification-before-completion
description: Use before claiming that changed code or configuration is complete, fixed, passing, or ready to commit or publish.
---

# Proportionate Verification

Use fresh evidence for completion claims, choosing checks proportional to the change and risk.

1. Identify the specific claim being made.
2. Run the smallest command or inspection that directly proves it.
3. Read the output and exit status before reporting the result.
4. State what was verified and disclose anything that could not be checked.

Examples of proportionate scope:

- Documentation or instructions: inspect the final file and relevant discovery output.
- Configuration: run the native parser or resolved-config command.
- Localized code: run focused tests, then broader checks only when dependencies or shared behavior make them relevant.
- Cross-cutting changes: run the applicable build and test suite.

Do not require redundant reruns, forced red-green cycles, reverting fixes to prove tests, or staged verification rituals. Never turn an unrun check into a success claim; report the limitation directly.
