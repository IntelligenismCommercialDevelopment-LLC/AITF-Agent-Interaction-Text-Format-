# AITF — Agent Interaction Text Format

**Human language is too inefficient for agent-to-agent interaction. This is a standardized language format protocol designed to improve the efficiency of text-based interaction between agents.**

---

## What is AITF?

AITF is a structured language specification that enables AI agents to read and write documents in a consistent, traceable, and machine-parseable format. It defines a shared vocabulary of node types, status codes, and structural rules — nothing more, nothing less.

Any agent that loads this protocol can read any AITF-format document and write any AITF-format document, regardless of the document's purpose.

---

## The Problem

When AI agents generate analysis, extract knowledge from books, delegate tasks to other agents, or propose solutions, the output is typically unstructured natural language. This creates several problems:

**For machines:** Unstructured text cannot be reliably parsed, referenced, or built upon. When Agent B reads Agent A's analysis, it has no systematic way to identify which parts are assumptions vs. evidence vs. conclusions, what has been verified vs. what is speculative, or where the reasoning gaps are.

**For humans:** When reviewing AI-generated analysis, there is no standard way to see the reasoning chain, challenge specific assumptions, or track what has been independently verified.

**For collaboration:** When multiple agents work on the same problem, their contributions cannot be cleanly separated, attributed, or reconciled without a shared format.

---

## Design Principles

### 1. One protocol, not many

Early versions of this protocol split rules by scenario — one extension for proposals, another for knowledge extraction, another for task delegation. This meant an agent had to predict what type of document it was dealing with before it could read it, and different document types used incompatible vocabularies.

The insight that drove the current design: **reading and writing are not separate activities, and scenarios are infinite.** An agent reads a document, forms its view, and writes a new document incorporating both the original content and its own contribution. The rules governing how it reads and how it writes are the same rules — the expression language itself.

So AITF collapsed everything into a single protocol: one node type registry, one set of status codes, one set of structural rules. Templates (optional guides for common scenarios) sit on top, but the protocol itself is scenario-agnostic.

### 2. Nodes are vocabulary, not operations

The protocol defines **what things are** (a node labeled `A` is an assumption, a node labeled `K` is a risk), not **what to do with them**. There are no "operations" like "challenge" or "modify_status" — because all operations reduce to the same thing: writing new signed nodes.

If you disagree with someone's claim, you write your own node expressing the disagreement. If you verify an assumption, you write a node stating the verification. The protocol doesn't need to enumerate operations because the vocabulary is expressive enough to capture any action.

### 3. Self-verification is dishonest

A core rule: **authors never mark their own assumptions, claims, solutions, mechanisms, or risks as "Verified."** The status `V` (verified) means independently confirmed by another party. Self-verification equals no verification.

This rule is not a suggestion — it's baked into the status assignment rules. It forces intellectual honesty: every document openly shows what has been independently checked and what hasn't.

### 4. Documents are self-contained

Every AITF document must be fully understandable by any agent reading it, without external context. No chat history, no message queues, no "you had to be there." One file = one complete context.

### 5. Append-only, signed contributions

No agent modifies another agent's content. To disagree, you write your own signed node. This creates an immutable audit trail: you can always trace who wrote what, when, and why. Version history is captured in a mandatory changelog.

---

## Node Type Registry

The protocol provides four categories of node types. Agents use whichever types best express their content — types are **not locked to scenarios**.

### Analysis Nodes
For reasoning, arguments, and solutions.

| Code | Meaning |
|------|---------|
| P | Problem statement |
| E | Evidence (data, citation, observation) |
| A | Assumption (taken as true without direct proof) |
| C | Claim (conclusion from evidence + assumptions) |
| S | Solution (proposed action) |
| M | Mechanism (how a solution works) |
| K | Risk (what could go wrong) |
| R | Rule (constraint or principle) |
| D | Dependency (prerequisite) |
| G | Gap (missing information) |

### Knowledge Nodes
For structured knowledge extracted from books, papers, or documents.

| Code | Meaning |
|------|---------|
| CON | Concept (independent knowledge unit) |
| DEF | Definition |
| REL | Relation (between concepts) |
| PRO | Property (attribute of a concept) |
| EXA | Example |
| CTR | Counter-example (boundary or exception) |
| APP | Application (useful scenario) |
| SRC | Source reference |
| TAG | Retrieval tag (search-optimized keywords) |
| GAP | Knowledge gap |

### Task Nodes
For work delegation and execution tracking.

| Code | Meaning |
|------|---------|
| OBJ | Objective (task goal) |
| CTX | Context (why the task is needed) |
| IN | Input (files, data, prior results) |
| OUT | Output (expected deliverable) |
| CST | Constraint (what not to do) |
| ACC | Acceptance criteria |
| DEP | Dependency (prerequisite) |
| Q | Question (request for clarification) |
| LOG | Execution log |

### Collaboration Nodes
For inter-agent dialogue.

| Code | Meaning |
|------|---------|
| SUG | Suggestion |
| RSP | Response |
| JUS | Justification (reasoning for a decision) |
| MOD | Modification (adapted version) |

---

## Status Codes

Every node carries a status that honestly represents its verification state.

| Code | Meaning |
|------|---------|
| V | Verified — independently confirmed |
| U | Untested — proposed but not yet examined |
| X | Contested — conflicting evidence exists |
| N | No evidence — placeholder or speculative |
| F | Flag human — requires human judgment |
| D | Done — completed |
| B | Blocked — waiting on dependency |
| W | Withdrawn — retracted by author |

---

## Quality Standards

The protocol includes built-in quality safeguards:

**Self-check requirements:** Every document must pass attribute completeness, reference integrity, and status rule compliance before finalization.

**Six anti-patterns** that authors must actively avoid:
1. Confidence inflation (high confidence without strong evidence)
2. Assumption hiding (assumptions buried inside claims)
3. Risk blindness (solutions without risk analysis)
4. Gap avoidance (complex documents with no acknowledged unknowns)
5. Circular reasoning (claims based on assumptions derived from the claims)
6. Premature solutioning (solutions before problem scoping)

**Honest self-assessment:** For substantive documents, authors identify their weakest node, most uncertain assumption, and biggest gap.

---

## Templates

Templates are optional efficiency guides for common scenarios. They suggest typical node combinations and execution flows, but do not add new rules — everything they describe uses the same Core Protocol vocabulary.

| Template | Typical Use |
|----------|-------------|
| [Proposal Authoring](template_proposal_authoring.md) | Agent generates a structured proposal from its own reasoning |
| [Knowledge Extraction](template_knowledge_extraction.md) | Converting books or documents into structured knowledge |
| [Task Delegation](template_task_delegation.md) | Assigning and tracking work between agents |
| [Proposal Review](template_proposal_review.md) | Evaluating and critiquing an existing proposal |
| [Suggestion](template_suggestion.md) | Lightweight improvement proposals between agents |

---

## Why not just use natural language?

Natural language is expressive but ambiguous. When an agent writes "this approach should work well," you cannot systematically determine: Is this a tested claim or an assumption? What evidence supports it? What risks has the agent considered? Has anyone independently verified this?

AITF makes these distinctions explicit and machine-parseable, while remaining human-readable. The structured format is not a replacement for reasoning — it is a way of making reasoning transparent.

---

## Architecture Evolution

This protocol evolved through four major versions, each driven by practical problems:

**v2.0** — Split by scenario (separate extensions for proposals, tasks, knowledge, suggestions). Each extension defined its own node types and operations.

**v3.0** — Filled gaps in v2.0 (missing operation formats, incomplete execution sequences, weak termination conditions). Added a new extension for proposal authoring.

**v3.0 → v4.0 breakthrough** — A series of first-principles challenges revealed that the scenario-based split was the wrong abstraction:

- *"If an agent reads a proposal and then reads a book, why should it need two different protocols?"* — Node vocabularies shouldn't be locked to scenarios.
- *"Generating a proposal, extracting knowledge, making suggestions, delegating tasks — these are all just writing."* — There's no fundamental difference between these "operations."
- *"Reading and writing follow the same rules."* — A vocabulary that lets you understand a document is the same vocabulary that lets you write one.
- *"Append, delete, modify, and create are all just 'write new content.'"* — The append-only rule means every operation is the same primitive.

**v4.0** — Unified everything into a single expression language (~3,400 tokens). Templates became optional add-ons (~600–800 tokens each), not protocol components.

---

## Token Budget

| What to load | Tokens | Capability |
|---|---|---|
| Core Protocol only | ~3,400 | Full read/write for any document type |
| Core + any one template | ~4,000–4,300 | Full capability + scenario guidance |
| Core + all templates | ~7,100 | Full capability + all scenario guidance |

---

## Languages

The protocol is available in both English and Chinese:

- [Core Protocol (English)](core_protocol_v4.0.md)
- [核心协议（中文）](core_protocol_v4.0_zh.md)
- Templates: English versions in root directory, Chinese versions with `_zh` suffix

---

## Author

Designed by **Minghai Zhuo**.

AITF draws on principles from Intelligenism (智能主义), a theoretical framework studying how intelligence emerges through bottom-up collaboration across scales — from neurons to brains, from individuals to organizations, from agents to agent networks.

The core insight that a protocol should define *how to express*, not *what to do*, mirrors the Intelligenism principle that organizational intelligence emerges from shared language and structure, not from top-down command.

---

## License

Copyright © 2026 Minghai Zhuo. All rights reserved.

License terms to be determined. See this repository's commit history for provenance timestamps.

---

## Contact

For inquiries about AITF, licensing, or collaboration, please open an issue in this repository.
