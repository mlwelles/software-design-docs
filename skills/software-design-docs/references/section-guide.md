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
