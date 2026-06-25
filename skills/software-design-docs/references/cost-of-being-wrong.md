# The Cost-of-Being-Wrong Test

The single rule that decides what belongs in a design doc.

> Include a decision in the doc only if getting it wrong carries significant, hard-to-reverse cost.

## How to apply it

For each decision you are tempted to document, ask: **if we choose wrong, how expensive is the fix later?**

- **Expensive to reverse → document it.** The programming language, the storage backend, the public API shape, the data model, trust boundaries — anything that ripples across the system or persists for years.
- **Cheap to reverse → leave it out.** Page size (20 vs 1,000 articles), a button color, an email provider you can swap in an afternoon.

Lynch's example: the choice of programming language is extremely costly to undo, so it earns a place in the doc. Whether a page shows 20 or 1,000 articles is fixable in hours, so it does not.

## Why this rule, and not "document everything"

A doc that records every decision costs more to write and read than it returns, and it buries the decisions that matter under ones that do not. Effort spent documenting a reversible choice is effort not spent pressure-testing an irreversible one.

## Calibrating effort

There is no fixed length for a design doc. Match the investment to the stakes: team size, risk, deadline, and how long the system will run. Sometimes the right investment is zero — see `when-to-write.md`. When you do write, spend your words on the decisions that are hard to take back.
