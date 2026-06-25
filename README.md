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

The `openskills` CLI (https://github.com/numman-ali/openskills#readme) is a universal skills loader for AI coding agents.

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
