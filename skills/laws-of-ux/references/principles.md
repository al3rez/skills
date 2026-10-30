# Principles for product decisions

Use the section that explains the observed burden. These are practical adaptations of *Laws of UX*, second edition, by Jon Yablonski (O'Reilly, 2024). Chapter numbers identify the source concept; diagnostic questions and implementation examples are operational guidance for this skill.

## Contents

1. Jakob: expectations and mental models
2. Fitts: acquiring an action
3. Miller: grouping and memory
4. Hick: deciding what to do
5. Postel: input tolerance and resilience
6. Peak–End: consequential moments and closure
7. Aesthetic–Usability: polish and perceived ease
8. Von Restorff: selective emphasis
9. Tesler: allocating complexity
10. Doherty: response and waiting
11. Applying principles: a testable local rule
12. Ethics: agency and useful friction

## 1. Jakob's law — expectations and mental models

**Look for:** Familiar controls, vocabulary, navigation, and interaction patterns that establish expectations the product does not meet. Include differences between the actor's goal and the destination an action opens.

**Ask:** What does the label or appearance promise? Does the action do that? Are common return paths and interaction conventions preserved? Is novel behavior worth learning for this audience?

**Design moves:** Reuse an appropriate convention; align wording with actual behavior; keep established interactions consistent across related surfaces. For a substantial migration, consider how existing users retain orientation and learn the change.

**Avoid:** Cloning another product's layout without considering the task, or treating novelty as inherently wrong. A chat-shaped interface should be evaluated against the conversational expectations it creates.

## 2. Fitts's law — acquiring an action

**Look for:** Small or crowded hit areas, long travel between related actions, controls that move before selection, and important actions that require unnecessary precision.

**Ask:** Where is the pointer or hand when this action is needed? What is the effective clickable area, including padding? Can the intended user acquire it reliably on the relevant input device?

**Design moves:** Enlarge the effective target, separate adjacent actions, place related controls near the work, and keep target positions stable where possible. Check keyboard alternatives as a complementary interaction concern.

**Avoid:** Measuring only the icon glyph or claiming the law prescribes a universal pixel size. Current platform and accessibility target-size criteria are separate requirements, with their own contexts and exceptions.

## 3. Miller's law — grouping and memory

**Look for:** Arbitrary information groups, repeated facts users must retain across screens, and tasks that require mentally combining several independently presented states.

**Ask:** What must the person remember rather than recognize? Do labels and spatial groups reveal meaningful relationships? Can the decision be made with its supporting information in view?

**Design moves:** Group by meaning, use informative headings, provide a review summary, and carry context between steps. A consolidated readiness view can remove the need to reconstruct a verdict from separate subsystems.

**Avoid:** Capping menus, tabs, cards, or fields at seven. Visible choices do not impose the same memory task as recalling absent information. Meaningful chunking matters more than a numerical limit.

## 4. Hick's law — deciding what to do

**Look for:** Several plausible starting points with no distinction, choices requiring unfamiliar terminology, and repeated decisions with no useful default or recommendation.

**Ask:** Does the person know which choice advances their goal? Is choice complexity caused by quantity, ambiguity, weak grouping, or lack of context? Would hiding options actually make the task harder?

**Design moves:** Group choices by intent, clarify differences and consequences, suggest an appropriate starting point, or break a difficult decision into meaningful stages. Apply progressive disclosure when occasional choices can remain easy to find.

**Avoid:** Removing necessary choices, replacing clear text with mysterious icons, imposing a fixed maximum, or forcing every task into a wizard. Optimize the decision rather than the count alone.

## 5. Postel's law — input tolerance and resilience

**Look for:** Valid human variations rejected unnecessarily, validation mismatches, silent omissions or truncation, input lost during failure, and errors that leave the next action unclear.

**Ask:** Which variations are safely equivalent? What is accepted, changed, or rejected? Does the user retain their intent and original work when the system cannot proceed?

**Design moves:** Normalize unambiguous formatting, expose meaningful limits before they cause surprise, use actionable validation, preserve entered values, and offer a recovery path for the actual failure.

**Avoid:** Accepting ambiguous meaning without clarification, silently discarding excess files or values, coercing unsafe data, or weakening authorization and data integrity to appear accommodating. Tolerance needs explicit boundaries.

## 6. Peak–End Rule — consequential moments and closure

**Look for:** High-effort or emotionally consequential points such as first value, submission, approval, payment, publication, failure, and recovery. Examine the final state of the user's whole task.

**Ask:** Is the stressful moment understandable and recoverable? Does the ending say what happened, where the result is, and what remains? Does success refer to the actual user goal?

**Design moves:** Give consequential transitions special care, retain evidence of completion, make results accessible, and provide a natural next action. Clearly distinguish accepted, queued, saved, published, and usable states when they differ.

**Avoid:** Adding celebration to mask unresolved work or polishing the ending while leaving the core journey broken. A strong final moment complements reliable task completion.

## 7. Aesthetic–Usability Effect — polish and perceived ease

**Look for:** Visual coherence that supports perceived ease, and attractive surfaces that may conceal problems with task completion. Separate user preference from observed effectiveness.

**Ask:** Can the person complete the task, not merely describe the screen as appealing? Do spacing, typography, and visual consistency help them understand it?

**Design moves:** Maintain a coherent visual system; evaluate real tasks as well as impressions. Use low-fidelity prototypes when useful to isolate structure from visual appeal.

**Avoid:** Treating a polished screenshot, favorable first impression, or resemblance to a popular product as evidence that the interaction works. Do not infer a functional bug from aesthetic preference alone.

## 8. Von Restorff Effect — selective emphasis

**Look for:** The intended next action blending into its peers, decorative content dominating consequential information, or many elements competing for equal attention.

**Ask:** What should stand out for this actor in this state? Is the distinction understandable without relying on color or motion alone? Could an overemphasized element be ignored as promotional noise?

**Design moves:** Reserve strong emphasis for the important distinction; combine text, position, scale, shape, and spacing appropriately. Keep warnings and primary actions distinguishable and respect motion preferences.

**Avoid:** Highlighting everything, assuming brighter always means clearer, or assigning arbitrary colors to every peer. Hierarchy should reflect the task's current needs.

## 9. Tesler's law — allocating complexity

**Look for:** User-managed orchestration, repeated entry, unnecessary configuration, and decisions requiring knowledge of the product's internal architecture.

**Ask:** Which complexity is inherent? Who is currently carrying it? What could the system infer, prefill, consolidate, or explain without taking meaningful decisions away from the user?

**Design moves:** Reuse known information, expose task-level actions, summarize prerequisites, and manage routine coordination in the system. Keep advanced controls available to those who need them.

**Avoid:** Hiding essential complexity behind a vague action, presenting an administrator's workflow to routine users by default, or assuming every powerful capability must be visible at every step. Simplicity must retain intelligibility and control.

## 10. Doherty Threshold — response and waiting

**Look for:** Unacknowledged actions, blank transitions, blocked typing during background work, misleading progress, and long operations that lose continuity when users navigate away.

**Ask:** How quickly does the interface acknowledge input? Can the user distinguish accepted work from completed work? What real progress, uncertainty, or next action can be shown while waiting?

**Design moves:** Aim for immediate interaction feedback; load progressively; show actual phases when available; keep appropriate work usable during background operations. Use optimistic presentation only when pending and failure states can be reconciled honestly.

**Avoid:** Treating roughly 400 ms as a guaranteed completion deadline, adding artificial delays to manufacture trust, inventing percentages, or displaying successful irreversible effects before confirmation. Perceived performance must preserve the truth of the operation.

## 11. Applying principles — a testable local rule

Translate a product goal through an applicable observation into a decision rule that resolves a real design choice. Keep the rule local enough to be useful and broad enough to guide related decisions.

For example, when recovery requires a missing prerequisite to be reloaded, a useful rule is: preserve the draft, identify the failed prerequisite, and offer its recovery action beside the error. Verify that the user can continue after recovery without re-entering their work.

Use task observation to choose among plausible approaches. Card sorting can test unfamiliar groupings; interviews can clarify users' language and goals; a journey map can expose handoffs and emotional peaks. Choose the method that resolves the actual uncertainty instead of requiring every research method for every change.

## 12. Ethics — agency and useful friction

Evaluate whether defaults, recommendations, emphasis, notifications, and continuation patterns serve the user's intention. Include plausible interrupted, mistaken, and vulnerable-user scenarios relevant to the feature.

Remove unnecessary work while retaining deliberate checkpoints for consequential choices. Make privacy, sharing, payment, or irreversible-action consequences understandable when those choices arise. Avoid manipulative urgency, hidden alternatives, and attention capture as an end in itself.

Use completion, error recovery, comprehension, and user explanations to assess benefit. Engagement or conversion alone cannot establish that a change helps the user. When recommending friction, explain which consequence it protects against and why that intervention fits the task.
