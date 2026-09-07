---
name: grill-me
description: Interview the user relentlessly about a plan or design until reaching shared understanding, resolving each branch of the decision tree. Use when user wants to stress-test a plan, get grilled on their design, or mentions "grill me".
---

Interview me relentlessly about this plan until we reach a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one.

Ask the questions one at a time.

## Scope

**State what this session is deciding before the first question, and hold that line.**
Follow a branch only while it can still change the decision at hand. When a branch
leads to something a later stage will settle on better evidence, note it as deferred
and come back to the decision in front of you.

Grill the whole design only when I ask you to stress-test the whole design.

## Before asking

If a question can be answered by exploring the codebase, explore the codebase
instead. The same goes for the project's specs, design docs, and records of
decisions already made — read those first. Ask me only what genuinely can't be
derived from them.

Don't re-open something already settled unless you've found evidence that
contradicts it. If you have, lead with the evidence.

## For each question

Give your recommended answer, with the main reason for it and what it costs. A
recommendation I can't weigh is one I have to accept blind.

## Stopping

Stop when the core choices, the constraints, and what "done" looks like are all
pinned down — not when you run out of branches. Then summarise the decisions.
