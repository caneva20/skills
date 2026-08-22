---
name: systematic-debugging
description: Use when diagnosing a bug, test failure, build failure, performance problem, or other unexpected technical behavior before changing code.
---

# Systematic Debugging

Find evidence for the root cause before changing code. Scale the investigation to the problem rather than following fixed phases.

1. Read the complete error and relevant logs.
2. Reproduce the problem when feasible and inspect recent related changes.
3. Trace incorrect data or behavior back to its source. Compare with a nearby working pattern when useful.
4. State one concrete hypothesis and test it with the smallest discriminating check.
5. Apply the smallest fix at the source, avoiding unrelated cleanup.
6. Re-run the original reproduction and the most relevant existing checks.

Do not guess when evidence can be gathered. If reproduction is impractical, add or request targeted diagnostics and clearly label uncertainty. A regression test is valuable when it protects meaningful behavior, but test-first development is not mandatory.

If repeated hypotheses fail or evidence points to a structural problem, pause and discuss the architecture rather than stacking speculative fixes.
