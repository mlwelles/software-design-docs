# Design: the `software-design-docs` skill

**Author:** Michael Welles
**Status:** Draft — approved for planning
**Created:** 2026-06-25
**Repository:** `github.com/mlwelles/software-design-docs` (to be created)
**Source material:** Michael Lynch, "Write an Effective Design Doc," *Refactoring English* — https://refactoringenglish.com/excerpts/write-an-effective-design-doc/

## Objective

Build a reusable, auto-invoked agent skill that guides an agent to write an effective software design document, and publish it as a public MIT-licensed repository installable across Claude Code, opencode, Zed, Codex, and other agentic coding tools.

## Background

Agents draft design docs with no shared standard for what a doc should contain or how to structure it. The result is predictable: docs that over-document reversible decisions, omit the irreversible ones, bury goals under implementation detail, or grow so complete they duplicate the implementation. Michael Lynch's article supplies a tested decision framework — a cost-of-being-wrong test for what to include, a when-to-write test for whether a doc should exist, and a catalog of candidate sections. Encoding that framework as a skill delivers the guidance at the moment an agent writes a doc, inside every tool the author uses.

The author maintains one canonical skills directory, `~/.config/agent/skills`. Claude Code reads it directly through the `~/.claude/skills` symlink. opencode, Zed, and Codex receive it through `openskills sync`, which regenerates a skill table inside each tool's `AGENTS.md`. Skills install from Git repositories via `openskills install`, which records the source and subpath in an `.openskills.json` manifest. This skill rides that existing pipeline rather than introducing a new mechanism.

## Goals

- Encode Lynch's framework as an auto-invoked skill: the cost-of-being-wrong test, the when-to-write test, the section catalog, and the writing principles.
- Trigger automatically when an agent writes, structures, or reviews a software design document.
- Offer two modes: a default quick-reference mode, and an optional guided walkthrough for complex, high-stakes docs.
- Compose with `writing-clearly-and-concisely` and `technical-writing-density` for prose quality, and degrade gracefully when those skills are absent.
- Credit and link Michael Lynch and *Refactoring English*.
- Install and autoload across the author's agentic tools through the existing openskills pipeline.

## Non-Goals

- Replacing `superpowers:brainstorming`. Brainstorming runs the decision-making dialogue and emits a spec; this skill authors the design-doc artifact. The two compose.
- Generating diagrams. The skill recommends editable diagram tools and defers to existing diagram skills (`mermaid-diagrams`, `excalidraw`, `draw-io`, `c4-architecture`).
- Encoding the author's personal storage conventions (`docs/designs`, `docs/plans`). Those belong in the author's `AGENT.md`, not in a public skill.
- Reproducing Lynch's prose. The skill is original writing that attributes the underlying ideas to him.

## Design

### Repository layout

```
software-design-docs/
├── README.md                 # what it is; credits and links Lynch / Refactoring English; install steps per tool
├── LICENSE                   # MIT
├── .gitignore
├── docs/
│   ├── designs/              # this spec
│   └── plans/                # the implementation plan
└── skills/
    └── software-design-docs/         # the installable artifact
        ├── SKILL.md                  # lean router: trigger, modes, decision rules, pointers
        ├── references/
        │   ├── when-to-write.md       # the 6-question test; "sometimes zero investment is right"
        │   ├── cost-of-being-wrong.md # the core filter for what to include
        │   ├── section-guide.md       # the section catalog: core vs situational, per-section guidance
        │   └── writing-quality.md     # hand-off to the writing skills; editable-diagram advice
        ├── templates/
        │   ├── design-doc.md          # full template; situational sections marked optional
        │   └── design-doc-minimal.md  # lean template for smaller efforts
        └── checklist.md               # pre-share self-review checklist
```

`openskills install` copies the skill directory, not the repository. Placing the skill at `skills/software-design-docs/` keeps the installed artifact clean — `SKILL.md`, `references/`, `templates/`, `checklist.md` — while `README.md`, `LICENSE`, and `docs/` stay at the repository root, out of the install. This matches the `softaworks/agent-toolkit` layout already present on the author's machine.

### Skill content and operating modes

`SKILL.md` stays short and routes to the reference files, which load on demand. This follows progressive disclosure: the entry point costs little context on every invocation, and detail loads only when needed.

The skill offers two modes:

- **Quick reference (default).** The agent is drafting a doc. The skill supplies the section catalog, applies the cost-of-being-wrong filter to choose which sections matter, gives per-section guidance, and offers a template to fill. The agent then runs the checklist and a writing-quality pass.
- **Guided walkthrough (for complex, high-stakes docs).** The skill steps through the work: run the when-to-write test, select sections via cost-of-being-wrong, draft each section with guidance, then review against the checklist and the writing skills. This is lighter than brainstorming's full dialogue and stays focused on the artifact.

The skill encodes these principles from the article:

- Frame goals as outcomes, not implementation. "Minimize outages from deploying new versions," not "add Kubernetes."
- Make the doc self-contained, so a reader unfamiliar with the project understands it without external context.
- State non-goals explicitly to bound the reader's assumptions.
- Use concrete metrics. "≤200ms latency," not "performant."
- Use editable diagram tools (Excalidraw, draw.io, Mermaid, D2), never photographed whiteboards.
- Prefer inline definitions over a glossary, so the reader never jumps sections.
- Record alternatives considered, to preempt the reader's "why not X?"
- Leave out trivial reversible decisions, exhaustive implementation detail, and every rejected idea.

`section-guide.md` classifies the article's candidate sections into **core** (almost always worth including) and **situational** (include only when the cost-of-being-wrong test rates the decision expensive to reverse).

### Composition with the writing skills

The skill draws a clean division of labor and makes the hand-off explicit in `writing-quality.md`:

- **This skill** owns document architecture: which sections, in what order, containing what, and whether the doc should exist at all.
- **`writing-clearly-and-concisely`** owns sentence-level craft: omit needless words, active voice, concrete language.
- **`technical-writing-density`** owns technical-prose density: cut empty words, but keep every reason, constraint, and disambiguation.

`writing-quality.md` names both skills and instructs the agent to apply them "if available in your environment." This wires the composition cleanly in the author's setup while keeping the public skill useful for users who lack those skills.

### Trigger description

The `SKILL.md` frontmatter `description` is the auto-invocation trigger. It fires on "design doc," "design document," "technical design," "RFC," and "document the architecture," while staying distinct from brainstorming's ideation trigger so the two coexist.

### Attribution and licensing

The repository uses the MIT license. The `README.md` credits Michael Lynch, names the article, links both the excerpt and the book, and states plainly that the skill distills and adapts the article's ideas. `SKILL.md` and `section-guide.md` carry a short source-credit line. MIT covers the project's original prose; the attribution credits the source ideas.

### Install and autoload

After the repository is pushed:

1. `openskills install mlwelles/software-design-docs` clones the skill into the shared directory and writes `.openskills.json` (source and subpath).
2. `openskills sync` regenerates the skill table inside each tool's `AGENTS.md` (opencode, Zed, Codex). Claude Code already sees the skill through the `~/.claude/skills` symlink.
3. Verification confirms the result (see below).

The exact `openskills install` flag — `--global` versus `--universal` — depends on how the author's current skills resolve into `~/.config/agent/skills`. Resolve it at implementation time by inspecting an installed skill, rather than guessing.

## Alternatives considered

- **Monolithic `SKILL.md`.** One file holding all guidance. Rejected: it bloats context on every invocation and abandons progressive disclosure.
- **Multi-skill toolkit repository** (author + reviewer + diagram helper). Rejected as YAGNI: the request is for one skill. The repository layout leaves room to add skills later without restructuring.
- **Hard dependency on the writing skills.** Rejected: it would break the skill for public users who lack them. Soft composition keeps the skill portable.

## Verification

- `openskills list` shows `software-design-docs`.
- Each tool's `AGENTS.md` (opencode, Zed, Codex) contains the new entry after `openskills sync`.
- A trigger-phrase prompt in Claude Code auto-invokes the skill (manual smoke test).
- The `README.md` renders with working links to Lynch's article and book.

## Open issues

- The exact `openskills install` flag for the shared directory. Resolve by inspecting an existing skill's install before running.
- Whether to register the repository with a marketplace or the author's openskills source list. Out of scope unless requested.
