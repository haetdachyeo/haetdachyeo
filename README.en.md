# Haetdachyeo

[한국어](README.md) | [English](README.en.md) | [日本語](README.ja.md)

> Let's say it's implemented.

Haetdachyeo is a GitHub Organization that **finalizes requirements and designs until they determine the implementation outcome**. Each design project has its own Repository.

The goal: AI or developers can implement from documentation alone, without further questions or core design work. Different implementers should produce equivalent features, contracts, domain rules, data, architecture, and operational characteristics.

Application implementation is excluded. **Design determines the outcome; implementation translates the finalized design into code.**

## Why It Exists

Even as AI implementation becomes easier, product and system decisions remain the developer's responsibility.

- Included and excluded scope
- Domain and system boundaries
- Data models, state transitions, and external contracts
- Architecture, infrastructure, technologies, and extensibility
- Alternatives and trade-offs

The purpose is **finalizing implementation decisions**, not producing more documentation.

## Meaning of the Name

**Haetdachyeo = “Let's say it's implemented.”**

When a design enables equivalent implementations without further core decisions, we consider it implemented.

## Goal

> AI or developers can implement from project documentation alone, without further questions or core design work.

Elements that must be equivalent:

- Features, exclusions, user behavior, and outcomes
- Normal, exceptional, failure, and boundary conditions
- Domain rules and state transitions
- API and external system contracts
- Data structures and persistence
- System composition and technology choices
- Security, performance, availability, and operations
- Testing and acceptance criteria

Source code need not match character for character. Variable names and code organization may differ if outcomes stay unchanged; outcome-changing decisions must not be left to AI discretion.

## Core Principles

- Exclude application implementation.
- Finalize requirements before design and technology choices.
- State included and excluded scope.
- Define normal, exceptional, failure, and boundary conditions.
- Leave no core design decisions for AI to infer.
- Prioritize simplicity; avoid overengineering.
- Provide extensibility for confirmed requirements and reasonably foreseeable changes.
- Record rationale, alternatives, and trade-offs.
- Judge completeness by implementability, not document volume.

## What Each Project Designs

[Design Workflow](guide/workflow/): writing templates and blog service examples.

1. [Purpose](guide/workflow/01-purpose.en.md): Purpose, background, users, scale, value, and success criteria
2. [Scope](guide/workflow/02-scope.en.md): Inclusions, exclusions, and feature boundaries
3. [Technology and Architecture](guide/workflow/03-architecture.en.md): Design direction, structure, technologies, and detailed design guidance

- `*.example.md`: Templates for authors to consider and fill in.
- Regular documents for the same stages: Filled-in blog service examples.
- Architecture overview: Overall summary. Detailed designs: `03-architecture/`.

Design the areas needed for each project's nature and scale.

```text
README
Requirements
Use Cases
Domain
Architecture
API
Database
Infrastructure
Security
Operations
ADR
AI Instructions
```

Neither filling every area as a formality nor using a single Markdown file is required. Split by domain or concern according to scale, complexity, and information needs.

Example API document structure:

```text
api/
├── README.md
├── common.md
├── auth.md
├── users.md
└── projects.md
```

## Recording Design Decisions

Record for important decisions:

| Item | Content |
| --- | --- |
| Problem | What needs solving |
| Constraints | Conditions limiting the choice |
| Choice | Selected approach |
| Rationale | Why it fits current requirements |
| Alternatives | Options considered and reasons for rejection |
| Trade-offs | Gains, sacrifices, and costs |

Example: Record not just the PostgreSQL choice, but its suitability, rejected alternatives, and costs.

## Level of Design

Complete decisions needed for current implementation without obstructing reasonable future changes.

Base extensibility on:

- Confirmed requirements
- Expected usage and quality criteria
- Identified follow-up requirements
- Likely, costly changes

No exhaustive future planning or unjustified abstractions and generalization.

## Role of AI

| Phase | Role |
| --- | --- |
| Design | Find omissions and ambiguity; research alternatives; analyze pros, cons, and trade-offs; check contradictions; improve document structure and wording |
| Implementation | Translate the developer's finalized design into code |

AI must not replace core design decisions or infer undocumented core behavior. Further questions or outcome-changing decisions mean the design is incomplete.

## Synchronizing Specifications and Implementation

**Current specifications alone must describe the entire project specification.** Keep them the up-to-date reference for intended behavior throughout the project's lifetime.

- Changes: **Update existing specifications directly**, not separate change specifications.
- API changes: Update inputs, outputs, errors, and rules to describe the complete changed behavior.
- Impact: Update related specifications and maintain consistency.
- History: Managed by Git. Current behavior must be clear without assembling past change documents or history.

AI synchronization order:

1. Identify specification changes and differences from the current implementation.
2. Align affected code and tests with the latest specifications.
3. Verify alignment through actual behavior and relevant tests.

Resolve omissions and contradictions in the specifications before implementation. **Align code and tests with finalized specifications, not specifications with code.**

## Long-Term Goal

Build **standards for AI development** once enough design projects accumulate. This is not immediate scope. Reuse reviewed, finalized approaches according to project conditions.

| Asset | Content |
| --- | --- |
| Design templates | Reusable units for APIs, authentication, real-time communication, storage, deployment, and operations |
| Decision rules | Conditions for selecting or excluding templates |
| Composition rules | Dependencies, compatibility, conflicts, and integration among templates |

**Decision rules** are the core asset. Consider features, traffic, budget, security, clients, operational capability, and quality together. Avoid fixed rules such as “blog = REST” or “mobile = JWT”; record each template's applicable and exclusion conditions, alternatives, and trade-offs.

```text
Service features and constraints
        ↓
Find applicable design templates and decision rules
        ↓
Analyze compatibility, cost, and trade-offs
        ↓
Assemble a design draft for the project
        ↓
Developer or user review and confirmation
        ↓
AI implementation from the finalized project specification
```

An AI draft does not guarantee production suitability. Before implementation, a developer or user must review and confirm its fit for current requirements and constraints.

## GitHub Structure

Each design project has an independent Repository under the GitHub Organization `haetdachyeo`.

```text
haetdachyeo/
├── haetdachyeo
├── project-a
├── project-b
├── project-c
└── ...
```

### `haetdachyeo/haetdachyeo`

The main Repository manages Organization standards, not an individual design project.

- Purpose, philosophy, and project principles
- Required design areas, writing standards, and completion criteria
- Project templates and AI usage standards

## Completion Criteria

> Can implementation begin without additional questions or core design work?

Documents must answer:

- Included and excluded scope
- User and system behavior in each situation
- Error and boundary handling
- System structure and responsibilities
- Data storage and change rules
- External interfaces and contracts
- Deployment and operations
- Technology choices and rationale
- Verification against requirements

**Complete** means consistent, verifiable answers with no core design discretion left to the implementer.
