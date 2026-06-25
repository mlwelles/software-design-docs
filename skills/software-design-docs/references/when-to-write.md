# Should You Write a Design Doc at All?

A design doc is sometimes the wrong investment. Decide before you start.

## The test

Answer yes or no:

1. Are multiple people coordinating the implementation?
2. Will the project take more than about three months of full-time work?
3. Will the system run in production for years?
4. Does the work span teams?
5. Are the goals or requirements still ambiguous?
6. Could a design-phase mistake cause catastrophic, hard-to-undo damage?

**Two or more "yes" answers → a design doc is worth writing.** One or zero → consider skipping it, or write the minimal template instead.

## Sometimes zero is the right answer

A small, reversible, single-author change rarely needs a doc. Forcing one wastes time and produces a document no one reads. The goal is good decisions, not paperwork.

## When in doubt

Write the minimal template (`templates/design-doc-minimal.md`): objective, background, goals, non-goals, alternatives. It captures the irreversible decisions cheaply and grows into a full doc only if the project does.
