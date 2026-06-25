# software-design-docs Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build the `software-design-docs` agent skill, publish it as a public MIT-licensed repo under the `mlwelles` GitHub account, then install and autoload it across the author's agentic tools.

**Architecture:** A single-skill repository. The installable artifact lives at `skills/software-design-docs/` (a lean `SKILL.md` router plus `references/`, `templates/`, and `checklist.md`); the repo root carries `README.md`, `LICENSE`, and `docs/`. Publishing pushes to GitHub; `openskills install` copies the skill into the shared skills dir; `openskills sync` registers it in each tool's `AGENTS.md`.

**Tech Stack:** Markdown (Anthropic SKILL.md format), `git` + `gh` (mlwelles account), the `openskills` CLI, the `skill-judge` skill for validation.

**Source material:** Michael Lynch, "Write an Effective Design Doc," *Refactoring English* — https://refactoringenglish.com/excerpts/write-an-effective-design-doc/

**Conventions for every task:**
- All paths are absolute under the repo root `~/Developer/mlwelles/software-design-docs/`.
- This repo is under `~/Developer/mlwelles`, so use the **mlwelles** GitHub account. Prefix any `git`/`gh` command that touches the remote with `gh auth switch --user mlwelles >/dev/null 2>&1 && `.
- Commit policy is `default` (agent discretion); commit after each task. Push only in Task 11+.
- The `.gitignore` and the design spec are already committed (commit `41ffb40`).

---

## File Structure

| File | Responsibility |
|------|----------------|
| `LICENSE` | MIT license text. |
| `README.md` | Human-facing: what the skill is, prominent credit to Lynch, install steps per tool. |
| `skills/software-design-docs/SKILL.md` | Lean router: trigger description, two modes, principles, pointers. |
| `skills/software-design-docs/references/cost-of-being-wrong.md` | The core "what to include" decision rule. |
| `skills/software-design-docs/references/when-to-write.md` | The "should this doc exist" test. |
| `skills/software-design-docs/references/section-guide.md` | Section catalog: core spine + situational, per-section guidance. |
| `skills/software-design-docs/references/writing-quality.md` | Composition with the writing skills; diagram-tool advice. |
| `skills/software-design-docs/templates/design-doc.md` | Full fill-in template; situational sections commented. |
| `skills/software-design-docs/templates/design-doc-minimal.md` | Lean template for small efforts. |
| `skills/software-design-docs/checklist.md` | Pre-share self-review checklist. |

---

## Task 1: LICENSE (MIT)

**Files:**
- Create: `~/Developer/mlwelles/software-design-docs/LICENSE`

- [ ] **Step 1: Write the file** with exactly this content:

````text
MIT License

Copyright (c) 2026 Michael Welles

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
````

- [ ] **Step 2: Verify** the file is non-empty and starts with `MIT License`.

Run: `head -1 ~/Developer/mlwelles/software-design-docs/LICENSE`
Expected: `MIT License`

- [ ] **Step 3: Commit**

```bash
git -C ~/Developer/mlwelles/software-design-docs add LICENSE
git -C ~/Developer/mlwelles/software-design-docs commit -m "chore: add MIT license"
```

---

## Task 2: SKILL.md (the router)

**Files:**
- Create: `~/Developer/mlwelles/software-design-docs/skills/software-design-docs/SKILL.md`

- [ ] **Step 1: Write the file** with exactly this content:

````markdown
---
name: software-design-docs
description: Use when writing, structuring, or reviewing a software design document — a design doc, technical design, RFC, or architecture write-up. Provides a test for whether the doc should exist (the when-to-write test), a rule for what to put in it (the cost-of-being-wrong test), a catalog of sections (core vs situational) with per-section guidance, fill-in templates, and a pre-share checklist. Distilled from Michael Lynch's "Write an Effective Design Doc."
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

**Quick reference (default).** The agent is already drafting. Pull the section catalog, apply the cost-of-being-wrong test to choose sections, follow the per-section guidance, fill a template, then run the checklist and a writing-quality pass.

**Guided walkthrough (complex or high-stakes docs).** Step through: (1) run the when-to-write test; (2) select sections with the cost-of-being-wrong test; (3) draft each chosen section using the section guide; (4) review against the checklist and the writing skills. Use this when the system is large, the decisions are hard to reverse, or the reader asks to be walked through it.

## Principles (the short version)

- Frame goals as user or business outcomes, not implementation. "Minimize deploy-related outages," not "adopt Kubernetes."
- Make the doc self-contained — a reader new to the project should follow it without outside context.
- State non-goals to bound what the reader assumes is in scope.
- Use concrete, measurable terms. "≤200 ms p99," not "fast."
- Diagram with editable tools (Excalidraw, draw.io, Mermaid, D2), never a photographed whiteboard.
- Define terms inline, where the reader meets them, instead of in a separate glossary.
- Record the alternatives you rejected and why, to answer "why not X?" before it is asked.
- Leave out trivial reversible decisions, exhaustive implementation detail, and a log of every rejected idea.

## Relationship to other skills

- `superpowers:brainstorming` runs the decision-making dialogue and produces a spec; this skill structures and writes the design-doc artifact. Use them together: brainstorm the decisions, then write the doc.
- For prose quality, compose with `writing-clearly-and-concisely` and `technical-writing-density` — see `references/writing-quality.md`.
- For diagrams, defer to `mermaid-diagrams`, `excalidraw`, `draw-io`, or `c4-architecture` if available.
````

- [ ] **Step 2: Verify the frontmatter parses** (name + description present, single document):

Run: `awk 'NR==1&&$0=="---"{f=1} f&&/^name:/{n=1} f&&/^description:/{d=1} END{print (n&&d)?"OK":"MISSING"}' ~/Developer/mlwelles/software-design-docs/skills/software-design-docs/SKILL.md`
Expected: `OK`

- [ ] **Step 3: Commit**

```bash
git -C ~/Developer/mlwelles/software-design-docs add skills/software-design-docs/SKILL.md
git -C ~/Developer/mlwelles/software-design-docs commit -m "feat: add SKILL.md router for software-design-docs"
```

---

## Task 3: references/cost-of-being-wrong.md

**Files:**
- Create: `~/Developer/mlwelles/software-design-docs/skills/software-design-docs/references/cost-of-being-wrong.md`

- [ ] **Step 1: Write the file** with exactly this content:

````markdown
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
````

- [ ] **Step 2: Verify** the file contains the rule blockquote.

Run: `grep -c "hard-to-reverse cost" ~/Developer/mlwelles/software-design-docs/skills/software-design-docs/references/cost-of-being-wrong.md`
Expected: `1`

- [ ] **Step 3: Commit**

```bash
git -C ~/Developer/mlwelles/software-design-docs add skills/software-design-docs/references/cost-of-being-wrong.md
git -C ~/Developer/mlwelles/software-design-docs commit -m "feat: add cost-of-being-wrong reference"
```

---

## Task 4: references/when-to-write.md

**Files:**
- Create: `~/Developer/mlwelles/software-design-docs/skills/software-design-docs/references/when-to-write.md`

- [ ] **Step 1: Write the file** with exactly this content:

````markdown
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
````

- [ ] **Step 2: Verify** the six-question test is present.

Run: `grep -c "^[0-9]\." ~/Developer/mlwelles/software-design-docs/skills/software-design-docs/references/when-to-write.md`
Expected: `6`

- [ ] **Step 3: Commit**

```bash
git -C ~/Developer/mlwelles/software-design-docs add skills/software-design-docs/references/when-to-write.md
git -C ~/Developer/mlwelles/software-design-docs commit -m "feat: add when-to-write reference"
```

---

## Task 5: references/section-guide.md

**Files:**
- Create: `~/Developer/mlwelles/software-design-docs/skills/software-design-docs/references/section-guide.md`

- [ ] **Step 1: Write the file** with exactly this content:

````markdown
# Section Guide

> Section set distilled from Michael Lynch, "Write an Effective Design Doc," *Refactoring English*.

Pick sections with the cost-of-being-wrong test (`cost-of-being-wrong.md`): include a section only when the decisions it records are costly to get wrong. The list below marks a small **core** spine that almost every doc needs, then **situational** sections to add when relevant.

## Core spine (almost always)

### Title
Short, distinctive, evocative. "RecencyBank," not "Project Flying Silver Horse." The reader should recall it and know roughly what it is about.

### Metadata
Author, status (draft / under review / approved), creation date, and the authoritative URL for the doc.

### Objective
One sentence, plain language: what this project is and why it exists. If you cannot state it in a sentence, the scope is not clear yet.

### Background
Why the project matters, the problem it solves, and prior attempts. Written so a reader new to the project needs no outside context.

### Goals
High-level outcomes framed as user or company impact — not implementation. "Minimize outages from deploys," not "add Kubernetes." Implementation belongs in later sections.

### Non-Goals
What you are explicitly not doing. Non-goals stop the reader from assuming scope you do not intend and preempt "but what about X?"

### Alternatives Considered
The approaches you rejected and why. This answers "why not X?" before a reviewer asks, and shows the decision was reasoned, not defaulted. Summarize each rejected option in a few lines — do not transcribe every idea.

## Situational (include when the decision is costly to get wrong)

### Related Documents
Links to testing plans, functional specs, or related design docs. Include when the doc depends on or extends others.

### Scenarios
Concrete walk-throughs of the finished system in use. Include when the value or correctness is easier to see through examples than description.

### Diagrams
Data flow, component relationships, system interactions. Include when structure is hard to follow in prose. Use editable tools (Excalidraw, draw.io, Mermaid, D2); never paste a photographed whiteboard.

### Glossary
Include only if you cannot define terms inline. Prefer inline definitions where the reader meets the term — a glossary forces the reader to jump sections.

### Constraints
Budget, infrastructure, client, or dependency limits that shape the design. Include when a constraint rules options in or out.

### Service Level Objectives (SLOs)
Measurable targets: uptime, latency, throughput, scale, stated in concrete numbers ("≤200 ms p99," "99.9% monthly"). Include for anything with reliability or performance requirements.

### Monitoring and Alerting
How you detect SLO breaches in production and what triggers an alert. Include whenever you set SLOs.

### Timeline
Milestones with deliverables that produce useful artifacts for stakeholders. Include when scheduling or sequencing is a real coordination concern.

### Interfaces
UI sketches, API semantics, file formats, CLI specs. Include when the contract with users or other systems is hard to change later — interfaces are usually expensive to reverse.

### Dependencies and Infrastructure
Languages, hardware, storage backends, third-party libraries. Include the ones that are costly to swap; these are classic cost-of-being-wrong decisions.

### Security
Threat model, attack surface, trust boundaries. Include when the system handles untrusted input, authenticates users, or guards sensitive operations.

### Privacy
Sensitive data handled, retention, access controls, protection. Include when the system touches personal or regulated data.

### Legal Considerations
Regulatory compliance, licensing, contractual obligations. Include when any of these bind the design.

### Logging
Which events you log, log levels, retention, and access. Include when audit, debugging, or compliance depends on it.

### Open Issues
Unresolved questions, each with a proposed direction and a next step. Include while decisions are still pending — it makes the doc a live working surface.

### Resolved Issues
Open issues now closed, with the decision recorded. Include to preserve the "why" behind settled questions so they do not get relitigated.
````

- [ ] **Step 2: Verify** both the core and situational headings are present.

Run: `grep -cE "^## (Core spine|Situational)" ~/Developer/mlwelles/software-design-docs/skills/software-design-docs/references/section-guide.md`
Expected: `2`

- [ ] **Step 3: Commit**

```bash
git -C ~/Developer/mlwelles/software-design-docs add skills/software-design-docs/references/section-guide.md
git -C ~/Developer/mlwelles/software-design-docs commit -m "feat: add section-guide reference"
```

---

## Task 6: references/writing-quality.md

**Files:**
- Create: `~/Developer/mlwelles/software-design-docs/skills/software-design-docs/references/writing-quality.md`

- [ ] **Step 1: Write the file** with exactly this content:

````markdown
# Writing Quality and Composition

This skill governs the *architecture* of the doc — which sections, in what order, holding what. The quality of the *prose* inside each section is a separate concern, owned by two companion skills. Apply them if they are available in your environment.

## Division of labor

- **This skill** — document architecture: section choice, ordering, and content; whether the doc should exist.
- **`writing-clearly-and-concisely`** (Strunk / *The Elements of Style*) — sentence-level craft: omit needless words, prefer the active voice, use concrete and specific language, put one idea in each sentence.
- **`technical-writing-density`** — technical-prose density: cut empty words, but keep every reason, constraint, and disambiguation. A design doc is technical writing, so apply this alongside the Strunk rules.

## How to apply them

After drafting the doc, make one pass for structure (this skill's checklist) and one pass for prose:

1. Run `writing-clearly-and-concisely` over the draft for clarity and concision.
2. Because the doc is technical, run `technical-writing-density` too: never trade away a constraint, a reason for a decision, or a disambiguation just to make a sentence shorter.

If those skills are not installed, apply the principles directly: active voice, concrete nouns, one idea per sentence, and no filler — while keeping every constraint and reason intact.

## Diagrams

Use editable diagram tools so the doc stays revisable: Excalidraw, draw.io, Mermaid, or D2. Never paste a photographed whiteboard — it freezes a decision you will want to change. If a diagram skill is available (`mermaid-diagrams`, `excalidraw`, `draw-io`, `c4-architecture`), defer to it.
````

- [ ] **Step 2: Verify** both companion skills are named.

Run: `grep -c -e "writing-clearly-and-concisely" -e "technical-writing-density" ~/Developer/mlwelles/software-design-docs/skills/software-design-docs/references/writing-quality.md`
Expected: `2` or greater (both names appear)

- [ ] **Step 3: Commit**

```bash
git -C ~/Developer/mlwelles/software-design-docs add skills/software-design-docs/references/writing-quality.md
git -C ~/Developer/mlwelles/software-design-docs commit -m "feat: add writing-quality composition reference"
```

---

## Task 7: templates (full + minimal)

**Files:**
- Create: `~/Developer/mlwelles/software-design-docs/skills/software-design-docs/templates/design-doc.md`
- Create: `~/Developer/mlwelles/software-design-docs/skills/software-design-docs/templates/design-doc-minimal.md`

- [ ] **Step 1: Write `design-doc.md`** with exactly this content:

````markdown
# <Title — short, distinctive, evocative>

**Author:** <name>
**Status:** Draft | Under review | Approved
**Created:** <YYYY-MM-DD>
**Canonical URL:** <link to this doc>

## Objective
<One plain sentence: what this is and why it exists.>

## Background
<Why this matters, the problem it solves, prior attempts. Self-contained — assume no outside context.>

## Goals
- <Outcome framed as user or company impact, not implementation.>

## Non-Goals
- <What this explicitly does not do.>

## Alternatives Considered
- **<Rejected option>:** <why it was rejected, in a few lines.>

<!-- Situational sections — keep one only if the decision it records is costly to get wrong. Delete the rest. See references/section-guide.md. -->

<!-- ## Related Documents -->
<!-- ## Scenarios -->
<!-- ## Diagrams  (editable tool: Excalidraw / draw.io / Mermaid / D2) -->
<!-- ## Constraints -->
<!-- ## Interfaces  (UI / API / file format / CLI) -->
<!-- ## Dependencies and Infrastructure -->
<!-- ## Service Level Objectives -->
<!-- ## Monitoring and Alerting -->
<!-- ## Security -->
<!-- ## Privacy -->
<!-- ## Legal Considerations -->
<!-- ## Logging -->
<!-- ## Timeline -->

## Open Issues
- <Unresolved question — proposed direction — next step.>
````

- [ ] **Step 2: Write `design-doc-minimal.md`** with exactly this content:

````markdown
# <Title>

**Author:** <name>  ·  **Status:** Draft  ·  **Created:** <YYYY-MM-DD>

## Objective
<One sentence.>

## Background
<The problem and why it matters. Self-contained.>

## Goals
- <Outcome, not implementation.>

## Non-Goals
- <Out of scope.>

## Alternatives Considered
- **<Rejected option>:** <why.>
````

- [ ] **Step 3: Verify** both templates exist and carry the core spine.

Run: `for f in design-doc design-doc-minimal; do grep -c "## Alternatives Considered" ~/Developer/mlwelles/software-design-docs/skills/software-design-docs/templates/$f.md; done`
Expected: `1` then `1`

- [ ] **Step 4: Commit**

```bash
git -C ~/Developer/mlwelles/software-design-docs add skills/software-design-docs/templates/
git -C ~/Developer/mlwelles/software-design-docs commit -m "feat: add full and minimal design-doc templates"
```

---

## Task 8: checklist.md

**Files:**
- Create: `~/Developer/mlwelles/software-design-docs/skills/software-design-docs/checklist.md`

- [ ] **Step 1: Write the file** with exactly this content:

````markdown
# Pre-Share Checklist

Run this before you circulate a design doc.

## Existence and scope
- [ ] The when-to-write test scored two or more "yes," or you deliberately chose the minimal template.
- [ ] Every documented decision is costly to reverse. Reversible decisions are cut.

## The spine
- [ ] Title is short, distinctive, evocative.
- [ ] Metadata present: author, status, date, canonical URL.
- [ ] Objective stated in one plain sentence.
- [ ] Background lets a newcomer follow the doc with no outside context.
- [ ] Goals are outcomes, not implementation.
- [ ] Non-goals bound the scope.
- [ ] Alternatives considered answer "why not X?"

## Content quality
- [ ] Targets are concrete and measurable (numbers, not adjectives).
- [ ] Terms are defined inline where they first appear.
- [ ] Diagrams use an editable tool, not a photo.
- [ ] No section duplicates the implementation or logs every rejected idea.

## Prose
- [ ] Ran writing-clearly-and-concisely (or applied Strunk's rules by hand).
- [ ] Ran technical-writing-density: no reason, constraint, or disambiguation was cut for brevity.

## Open questions
- [ ] Open issues list unresolved decisions, each with a proposed next step.
````

- [ ] **Step 2: Verify** the checklist has checkbox items.

Run: `grep -c "^- \[ \]" ~/Developer/mlwelles/software-design-docs/skills/software-design-docs/checklist.md`
Expected: a number `>= 15`

- [ ] **Step 3: Commit**

```bash
git -C ~/Developer/mlwelles/software-design-docs add skills/software-design-docs/checklist.md
git -C ~/Developer/mlwelles/software-design-docs commit -m "feat: add pre-share checklist"
```

---

## Task 9: README.md

**Files:**
- Create: `~/Developer/mlwelles/software-design-docs/README.md`

- [ ] **Step 1: Resolve the canonical openskills link** (avoids a guessed URL):

Run: `npm view openskills homepage repository.url 2>/dev/null`
Use the returned homepage (or repository URL) as `<OPENSKILLS_URL>` in Step 2. If npm returns nothing, use `https://www.npmjs.com/package/openskills`.

- [ ] **Step 2: Write the file** with exactly this content (substituting `<OPENSKILLS_URL>` from Step 1):

````markdown
# software-design-docs

An auto-invoked agent skill that helps coding agents write effective software design documents.

It distills the framework from Michael Lynch's essay **"Write an Effective Design Doc"** into a reusable skill: a test for whether a doc is worth writing, a rule for what each doc should contain, a catalog of sections, fill-in templates, and a pre-share checklist.

## Credit

This skill distills and adapts ideas from:

> **Michael Lynch — "Write an Effective Design Doc"**, an excerpt from the book *Refactoring English*.
> https://refactoringenglish.com/excerpts/write-an-effective-design-doc/

All credit for the underlying framework — the cost-of-being-wrong test, the when-to-write test, and the section catalog — belongs to Michael Lynch. This repository is an independent, original write-up of those ideas as an agent skill; it does not reproduce the article's text. Please read and support the original at the link above.

## What it does

The skill triggers when an agent writes, structures, or reviews a design doc (design doc, technical design, RFC, architecture write-up). It then:

- decides whether a doc is even warranted (the when-to-write test),
- chooses what goes in it (the cost-of-being-wrong test — document only decisions that are expensive to reverse),
- structures it from a catalog of core and situational sections,
- offers fill-in templates (full and minimal), and
- checks the result against a pre-share checklist.

It composes with prose-quality skills (`writing-clearly-and-concisely`, `technical-writing-density`) when they are present, and defers diagrams to dedicated diagram skills.

## What's inside

```
skills/software-design-docs/
├── SKILL.md
├── references/
│   ├── when-to-write.md
│   ├── cost-of-being-wrong.md
│   ├── section-guide.md
│   └── writing-quality.md
├── templates/
│   ├── design-doc.md
│   └── design-doc-minimal.md
└── checklist.md
```

## Installation

### With openskills (recommended — works across Claude Code, opencode, Zed, Codex)

The `openskills` CLI (<OPENSKILLS_URL>) is a universal skills loader for AI coding agents.

```bash
# install the skill from this repo
openskills install mlwelles/software-design-docs

# register it in your AGENTS.md so each tool knows about it
openskills sync
```

`openskills sync` writes a skills table into your `AGENTS.md`, so opencode, Zed, and Codex load the skill with `npx openskills read software-design-docs`.

### Claude Code

Claude Code auto-discovers skills in its skills directory and reads each `SKILL.md` `description` to decide when to invoke. Install with openskills (above), or copy the skill in directly:

```bash
git clone https://github.com/mlwelles/software-design-docs
cp -r software-design-docs/skills/software-design-docs ~/.claude/skills/
```

### opencode / Zed / Codex (manual)

These tools load skills listed in an `AGENTS.md`. After installing the skill, run `openskills sync -o /path/to/AGENTS.md`, or add an entry to the `<available_skills>` block by hand.

### Other agentic tools

Any tool that reads `AGENTS.md`, or that supports the Anthropic `SKILL.md` format, can use this skill. Point it at `skills/software-design-docs/SKILL.md`.

## How auto-invocation works

The `SKILL.md` frontmatter carries a `description` that names the trigger conditions ("design doc," "technical design," "RFC," "document the architecture"). Agents match the task against installed skill descriptions and load the matching skill before acting.

## License

MIT — see [LICENSE](LICENSE). The license covers this repository's original text and templates. The underlying ideas are Michael Lynch's; please credit the source article.
````

- [ ] **Step 3: Verify** the credit link to Lynch's article is present.

Run: `grep -c "refactoringenglish.com/excerpts/write-an-effective-design-doc" ~/Developer/mlwelles/software-design-docs/README.md`
Expected: `1`

- [ ] **Step 4: Commit**

```bash
git -C ~/Developer/mlwelles/software-design-docs add README.md
git -C ~/Developer/mlwelles/software-design-docs commit -m "docs: add README with attribution and install instructions"
```

---

## Task 10: Validate the skill package

**Files:** none created; this task validates and may trigger small edits.

- [ ] **Step 1: Run the skill-judge skill** against the package.

Invoke the `skill-judge` skill on `~/Developer/mlwelles/software-design-docs/skills/software-design-docs/`. It scores the SKILL.md against the official skill spec (frontmatter correctness, description quality, progressive disclosure, no bloat).

- [ ] **Step 2: Confirm the description triggers correctly.**

Re-read the `description` field. Confirm it names concrete trigger phrases ("design doc," "technical design," "RFC," "architecture") and stays distinct from `superpowers:brainstorming`'s ideation trigger. Adjust wording only if skill-judge flags an issue.

- [ ] **Step 3: Check internal links resolve.** Every reference path named in `SKILL.md` must exist.

Run:
```bash
cd ~/Developer/mlwelles/software-design-docs/skills/software-design-docs
for p in references/when-to-write.md references/cost-of-being-wrong.md references/section-guide.md references/writing-quality.md templates/design-doc.md templates/design-doc-minimal.md checklist.md; do test -f "$p" && echo "OK $p" || echo "MISSING $p"; done
```
Expected: every line begins with `OK`.

- [ ] **Step 4: Fix and commit** any issues skill-judge raised.

```bash
git -C ~/Developer/mlwelles/software-design-docs add -A skills/
git -C ~/Developer/mlwelles/software-design-docs commit -m "fix: address skill-judge findings" || echo "no changes to commit"
```

---

## Task 11: Create the public GitHub repo and push

**Note:** This is the first outward-facing, hard-to-reverse step — it publishes a public repo under the user's identity. The user explicitly authorized publishing. Use the **mlwelles** account.

**Files:** none.

- [ ] **Step 1: Confirm the active account.**

Run: `gh auth switch --user mlwelles >/dev/null 2>&1 && gh auth status 2>&1 | grep -i "account mlwelles"`
Expected: a line confirming the active account is `mlwelles`.

- [ ] **Step 2: Create the public repo and push** `main` in one step.

```bash
gh auth switch --user mlwelles >/dev/null 2>&1 && \
gh repo create mlwelles/software-design-docs \
  --public \
  --source ~/Developer/mlwelles/software-design-docs \
  --remote origin \
  --description "An auto-invoked agent skill for writing effective software design docs. Distilled from Michael Lynch's \"Write an Effective Design Doc.\"" \
  --push
```
Expected: repo created; `main` pushed; remote `origin` set.

- [ ] **Step 3: Verify** the remote and the pushed branch.

Run: `git -C ~/Developer/mlwelles/software-design-docs remote -v && git -C ~/Developer/mlwelles/software-design-docs ls-remote --heads origin`
Expected: `origin` points at `github.com/mlwelles/software-design-docs`; a `refs/heads/main` line appears.

- [ ] **Step 4: Confirm the README renders on GitHub** with a working article link.

Run: `gh repo view mlwelles/software-design-docs --web` (opens in browser) — or `gh api repos/mlwelles/software-design-docs/readme --jq .name`
Expected: README present; the Lynch link works when clicked.

---

## Task 12: Install the skill into the shared skills dir

**Goal:** Get the published skill into `~/.config/agent/skills/` (the canonical dir Claude Code reads via the `~/.claude/skills` symlink) using `openskills`, matching how existing skills were installed.

**Files:** none in the repo; `openskills` writes into `~/.config/agent/skills/software-design-docs/`.

- [ ] **Step 1: Record the current install location convention.**

Run: `cat ~/.config/agent/skills/crafting-effective-readmes/.openskills.json`
Note the `source`/`subpath` shape — the new skill's manifest should look analogous (`source: mlwelles/software-design-docs`, `subpath: skills/software-design-docs`).

- [ ] **Step 2: Install from GitHub** (global install targets the shared dir).

Run: `openskills install -g mlwelles/software-design-docs`
If `openskills` prompts to select skills, choose `software-design-docs`. If `-g` does not resolve to `~/.config/agent/skills`, re-run without `-g` or with `-u` and compare; the goal is a directory at `~/.config/agent/skills/software-design-docs/`.

- [ ] **Step 3: Verify the install landed in the shared dir** with a manifest.

Run:
```bash
test -f ~/.config/agent/skills/software-design-docs/SKILL.md && echo "SKILL ok"
cat ~/.config/agent/skills/software-design-docs/.openskills.json
openskills list | grep -A1 software-design-docs
```
Expected: `SKILL ok`; a manifest naming `mlwelles/software-design-docs`; the skill appears in `openskills list`.

- [ ] **Step 4: Confirm Claude Code sees it via the symlink.**

Run: `ls -l ~/.claude/skills/software-design-docs/SKILL.md`
Expected: the file resolves through the `~/.claude/skills -> ../agent/skills` symlink.

---

## Task 13: Sync AGENTS.md across tools and verify autoload

**Goal:** Register the skill in every tool's `AGENTS.md` so opencode, Zed, and Codex autoload it. Claude Code already sees it (Task 12, Step 4).

**Files:** the `AGENTS.md` files that carry an openskills skills table.

- [ ] **Step 1: Discover every AGENTS.md with an openskills table.**

Run:
```bash
grep -rl "SKILLS_TABLE_START" ~/.config ~/.codex 2>/dev/null
```
Expected: a list of `AGENTS.md` paths (at least `~/.config/opencode/AGENTS.md` and `~/.config/zed/AGENTS.md`; possibly a Codex/JetBrains one). These are the sync targets.

- [ ] **Step 2: Sync each discovered AGENTS.md.**

For each path `P` from Step 1, run:
```bash
openskills sync -y -o "P"
```
(Replace `P` with the actual path. `-y` syncs all installed skills non-interactively.)

- [ ] **Step 3: Verify the skill now appears in each table.**

Run:
```bash
for P in $(grep -rl "SKILLS_TABLE_START" ~/.config ~/.codex 2>/dev/null); do echo "== $P =="; grep -c "software-design-docs" "$P"; done
```
Expected: a count `>= 1` for each file.

- [ ] **Step 4: Smoke-test skill retrieval.**

Run: `openskills read software-design-docs | head -20`
Expected: the SKILL.md content prints (confirms agents can load it on demand).

- [ ] **Step 5: Manual auto-invocation check (Claude Code).**

In a fresh Claude Code session, give a design-doc prompt (for example, "help me write a design doc for a rate limiter"). Confirm the agent auto-invokes `software-design-docs`. This is a manual check; no command asserts it.

- [ ] **Step 6: Commit any AGENTS.md changes that live in a git repo.**

`~/.config` is a git repo. Commit the updated tool AGENTS.md files there (honor that repo's commit conventions — use `config-commit`/`commit-all` as appropriate). The skill repo itself needs no further commit.

---

## Self-Review

**1. Spec coverage** — every spec section maps to a task:

- Repo layout → Tasks 1, 2–9 (files), File Structure table.
- Skill content + two modes → Task 2 (SKILL.md), Tasks 3–8 (references, templates, checklist).
- Cost-of-being-wrong / when-to-write / section catalog → Tasks 3, 4, 5.
- Composition with writing skills → Task 6, plus checklist (Task 8) prose section.
- Trigger description → Task 2 frontmatter; validated in Task 10.
- Attribution + MIT license → Task 1 (LICENSE), Task 9 (README credit), credit lines in Tasks 2 and 5.
- Install + autoload pipeline → Tasks 11 (push), 12 (install), 13 (sync + verify).
- Verification (openskills list, AGENTS.md grep, README links, smoke test) → Tasks 10, 11, 12, 13.
- Open issue: exact openskills flag → Task 12 resolves it by inspecting an existing skill.

No spec requirement is left without a task.

**2. Placeholder scan** — the only angle-bracket placeholders are inside the *template files*, where fill-in slots (`<Title>`, `<name>`) are the intended deliverable, not gaps in the plan. `<OPENSKILLS_URL>` in Task 9 is resolved by Task 9 Step 1 before use. No `TBD`/`TODO`/"handle edge cases" steps exist.

**3. Type consistency** — file paths and names are consistent across tasks: `software-design-docs` (skill + repo), the four reference filenames, the two template filenames, and `checklist.md` match between the File Structure table, the SKILL.md pointers (Task 2), the README tree (Task 9), and the link check (Task 10, Step 3).

---

## Execution Handoff

Plan complete and saved to `docs/plans/2026-06-25-software-design-docs-skill.md` in the repo. Two execution options:

1. **Subagent-Driven (recommended)** — a fresh subagent per task, with review between tasks.
2. **Inline Execution** — execute tasks in this session with checkpoints.

Which approach?
