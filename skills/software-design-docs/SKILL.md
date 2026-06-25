---
name: software-design-docs
description: Use when writing, structuring, or reviewing a software design document — a design doc, technical design, RFC, or architecture write-up. Applies after decisions are made, as the agent drafts or structures the document (not during pre-decision ideation — use superpowers:brainstorming for that). Provides a test for whether the doc should exist (the when-to-write test), a rule for what to put in it (the cost-of-being-wrong test), a catalog of sections (core vs situational) with per-section guidance, fill-in templates, and a pre-share checklist. Distilled from Michael Lynch's "Write an Effective Design Doc."
---

# Software Design Docs

> Distilled from Michael Lynch, "Write an Effective Design Doc," *Refactoring English*. Full attribution in the repository README.

## Purpose

A design doc earns its cost by getting expensive-to-reverse decisions right before implementation. Document a decision only when getting it wrong is costly; leave out anything cheap to change later. The doc is a thinking tool and a coordination tool, not a second copy of the code.

## What this skill does

When an agent writes, structures, or reviews a design doc, this skill supplies:

- a test for whether the doc should exist at all → `references/when-to-write.md`
- the rule for what each doc should contain → `references/cost-of-being-wrong.md`
- a catalog of sections, core vs situational, with per-section guidance → `references/section-guide.md`
- fill-in templates → `templates/design-doc.md`, `templates/design-doc-minimal.md`
- a pre-share checklist → `checklist.md`
- how to make the prose itself strong → `references/writing-quality.md`

## Two modes

**Quick reference (default).** The agent is already drafting. LOAD `references/section-guide.md` and `references/cost-of-being-wrong.md`, apply the cost-of-being-wrong test to choose sections, follow the per-section guidance, fill a template, then run the checklist and a writing-quality pass.

**Guided walkthrough (complex or high-stakes docs).** Step through: (1) LOAD `references/when-to-write.md` and run the test; (2) LOAD `references/cost-of-being-wrong.md` and select sections; (3) LOAD `references/section-guide.md` and draft each chosen section; (4) review against `checklist.md` and the writing skills. Use this when the system is large, the decisions are hard to reverse, or the reader asks to be walked through it.

## Principles (the short version)

- Frame goals as user or business outcomes, not implementation. "Minimize deploy-related outages," not "adopt Kubernetes."
- Make the doc self-contained — a reader new to the project should follow it without outside context.
- State non-goals to bound what the reader assumes is in scope.
- Use concrete, measurable terms. "≤200 ms p99," not "fast."
- Diagram with editable tools (Excalidraw, draw.io, Mermaid, D2), never a photographed whiteboard.
- Define terms inline, where the reader meets them, instead of in a separate glossary.
- Record the alternatives you rejected and why, to answer "why not X?" before it is asked.
- Leave out trivial reversible decisions, exhaustive implementation detail, and a log of every rejected idea.

## Never do these things

- **Never paste a photographed whiteboard.** It freezes a decision you will want to change; use an editable tool instead.
- **Never frame goals as implementation.** "Add Kubernetes" is not a goal. "Minimize deploy-related outages" is.
- **Never document a reversible decision** just because you made it. Cheap-to-fix decisions (page size, button color, swappable vendors) waste the reader's attention and bury the decisions that matter.
- **Never put a term in a glossary if you can define it inline.** A glossary forces the reader to jump sections; inline definitions keep context at the point of first contact.
- **Never transcribe every rejected idea** in Alternatives Considered. Summarize each rejected option in a few lines; a complete log of every idea buries the reasoning.
- **Never write the doc before the doc is warranted.** Run the when-to-write test first; forcing a design doc on a small, reversible, single-author change wastes time and produces a document no one reads.

## Relationship to other skills

- `superpowers:brainstorming` runs the decision-making dialogue and produces a spec; this skill structures and writes the design-doc artifact. Use them together: brainstorm the decisions, then write the doc.
- For prose quality, compose with `writing-clearly-and-concisely` and `technical-writing-density` — see `references/writing-quality.md`.
- For diagrams, defer to `mermaid-diagrams`, `excalidraw`, `draw-io`, or `c4-architecture` if available.
