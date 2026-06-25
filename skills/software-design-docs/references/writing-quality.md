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
