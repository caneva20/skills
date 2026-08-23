---
name: work-decomposition
description: Use before planning or implementing non-trivial work to find reusable capabilities, propose focused work items, and get user confirmation before splitting the work.
---

# Work decomposition

Find useful boundaries in non-trivial work before implementation. Treat the
user's request as the outcome to preserve, then look for capabilities that
could serve more than one consumer.

## When to use

Use this skill after work intake and before planning or implementation for
feature work, systems, behavior changes, architectural changes, and requests
that span more than one responsibility. Run it even when the request appears
small if any part could reasonably be reused.

Do not split a request only because it contains multiple edits. Split it when
it contains a capability, policy, integration, or other responsibility that
could be owned, tested, or delivered separately.

## Inspect first

1. Read the relevant code, tests, documentation, issues, and stated plans.
2. Identify the responsibilities in the requested outcome and where each one
   belongs in the current architecture.
3. Search for current consumers, related domain concepts, and nearby patterns
   that indicate likely future consumers.
4. Treat repository evidence, roadmap evidence, domain inference, and agent
   judgment as different kinds of evidence. Label them clearly.

When the request produces multiple work items, map dependencies before writing
the proposal. For each item, record its prerequisites, the work it unblocks,
and whether it can run in parallel with other items. Check both dependencies
between proposed items and dependencies on existing work found during
inspection. Do not derive an order from the list order alone.

Use repository evidence for confirmed dependencies and label inferred or
uncertain relationships. Ask one focused question when an uncertain
relationship changes the implementation order. Reject cyclic dependencies or
rework the split before presenting it as executable work.

Do not invent a generic abstraction just because another use is theoretically
possible. If a proposed future consumer affects the design, name it and ask
the user to confirm it.

## Decide whether to split

Propose a split whenever you find a reusable part, even if the current request
has only one known consumer. A reusable part may start with the narrowest
contract needed by the confirmed consumers, but the work item should give that
responsibility a clear owner and boundary.

Keep a request together when the proposed parts cannot be understood, tested,
or reviewed independently, or when separating them would create a speculative
framework without a named consumer.

Separate splitting the work from generalizing the implementation. A split does
not permit requirements for unconfirmed consumers. A confirmed or user-
approved likely consumer may shape the contract, data model, ownership, and
tests.

## Proposal

Before changing code, show a concise proposal containing:

1. The original outcome and what remains in scope.
2. Each proposed work item with its objective, owner, consumers, exclusions,
    dependencies, unblocked work, and observable completion criteria.
3. The evidence for each current or future consumer, marked as confirmed or
   inferred.
4. Why the split improves reuse, ownership, testing, review, or delivery.
5. The dependency-aware implementation order, including parallel work where applicable, and the boundaries between work items.
6. The verification approach for each work item.

If no split is useful, say why and continue with the original request.

If a split is useful, stop and ask the user to confirm, revise, or reject the
proposal. Do not begin implementation before the user decides. Offer both
options below:

- Continue with the accepted work items in the current thread.
- Record the work items with an available todo, work-item, planning, or issue
  skill, then stop so the next item can start in a clean context.

Do not choose a storage or tracking mechanism silently. Use the available skill
for the destination the user selects, and follow that skill's approval and
verification rules.

If the user chooses to record multiple work items, carry their confirmed
dependency relationships into the selected tracking workflow. Do not create
independent backlog items when one item must wait for another.

## Writing and handoff

Apply `unslop` and `concise-writing` to the proposal and any handoff text. Keep
the proposal concrete. Preserve identifiers, commands, acceptance criteria,
and other exact technical text.

When the user accepts a split, maintain the original outcome as the parent
goal. Do not mark a work item complete until its own completion criteria and
verification are satisfied.
