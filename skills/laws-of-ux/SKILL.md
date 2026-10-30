---
name: laws-of-ux
description: "Review or design product journeys using the behavioral principles in Jon Yablonski's Laws of UX. Use for interaction clarity, cognitive load, navigation, onboarding, feedback, and recovery; not for purely cosmetic edits or a formal accessibility conformance audit."
---

# Laws of UX

Apply behavioral psychology to help people complete their intended tasks with understandable choices, appropriate effort, and reliable feedback. Treat the book's laws as explanatory heuristics, not universal requirements or substitutes for research.

## Start from the journey

Identify the actor, intended outcome, entry point, and relevant constraints from the request and available artifacts. State assumptions when the audience or goal is uncertain. Examine a representative task end to end rather than evaluating isolated screens solely by appearance.

Scale the review to the request. A focused component question does not require a product-wide audit. Preserve the user's chosen product, platform conventions, design system, and implementation scope. A request for findings does not authorize a redesign; an authorized implementation does not require a new approval step just because this skill is used.

Read the relevant sections of [references/principles.md](references/principles.md) when choosing a law, proposing a tradeoff, or checking a common misinterpretation. Its headings follow the book's ten laws, then application and ethics. Do not force every law into every task.

## Inspect behavior and evidence

Trace what a person must notice, understand, choose, remember, enter, and wait for. Look for work that the product could reasonably do for them, while retaining meaningful control.

For the journey under review, inspect relevant transitions: entry and prerequisites, primary action, progress, completion, and plausible failure or interruption. Pay particular attention to:

- A familiar-looking control whose behavior breaks its promise, including labels that invite unavailable actions.
- Prerequisites, hidden state, repeated entry, or cross-screen decisions that the user must coordinate.
- Competing emphasis, unclear grouping, or disclosure that conceals the next step.
- Validation that changes at submission, unexplained omissions, lost input, and errors without a usable recovery action.
- End states that leave people unsure what happened, what remains, or where the result can be used.

Separate observed behavior, behavior established by source code, and hypotheses about user impact. A screenshot establishes appearance, not the behavior behind a control. A code path establishes its conditions, not measured user confusion or deployed behavior. Do not claim browser testing, production verification, or measured latency without that evidence.

## Review an existing experience

Lead with the most consequential obstacle to the user's outcome. Rank findings by task blockage, consequences, likely frequency, and recovery effort. Avoid invented numerical UX scores and lengthy inventories of minor styling preferences.

For each material finding, connect:

**Trigger and evidence → user burden → relevant principle → focused change → validation.**

Include a precise artifact location, reproduction, or screenshot region where available. Mark likely cognitive effects as hypotheses. Name the law that best explains the problem; merely naming a law does not establish a defect. State the inspection scope and important limits once, rather than repeating disclaimers for every finding.

Recommend a change specific enough to implement or test. Preserve useful behavior and explain any tradeoff, such as adding a deliberate review checkpoint or moving occasional controls behind a discoverable disclosure.

## Design or improve an experience

Start with the user's next meaningful action and the information needed to take it. Absorb avoidable coordination in the product through safe defaults, reuse of known information, clear grouping, and contextual assistance. Keep advanced control available where it is useful.

Use familiar interaction patterns where they match the task. Make the visible invitation, actual capability, pending state, and completion state agree. Show prompt acknowledgment for slow work, preserve input, and offer recovery that addresses the actual unmet condition.

Convert a principle into a local decision rule and an observable acceptance condition. For example: a failed prerequisite load keeps the draft and offers Retry; successful recovery restores the action without re-entry. Choose rules appropriate to this product rather than importing arbitrary limits from a past example.

## Validate proportionally

For an implementation, exercise the changed journey and relevant failure or return path. For a proposal, identify the smallest useful task test. Use unassisted completion, errors, wrong turns, recovery effort, or time to first useful result when those measures answer the question. Pair quantitative measures with users' explanations when the cause is uncertain.

Check relevant keyboard, touch, assistive-technology, and reduced-motion behavior. Use current platform or accessibility criteria when formal measurements are needed; this heuristic review alone does not establish conformance.

## Interpretation boundaries

- Miller's law supports meaningful grouping; it does not impose a seven-item navigation limit.
- Hick's law concerns decision difficulty; fewer visible controls can increase effort if the remaining interface becomes abstract or hides needed choices.
- Doherty's roughly 400 ms threshold motivates timely interaction feedback; it is not an end-to-end deadline for every operation.
- Perceived speed and visual polish do not prove completion, correctness, or usability. Do not fabricate progress, success, or deliberate waiting to imply work that did not occur.
- Reduce unnecessary friction while preserving informed choice and safeguards proportionate to consequences. Optimize for the user's outcome and agency rather than attention capture alone.

## Basis

Original operational synthesis of Jon Yablonski, *Laws of UX*, second edition (O'Reilly, 2024), chapters 1–12. The linked reference includes practical adaptations; it does not reproduce the book or turn its examples into product requirements.
