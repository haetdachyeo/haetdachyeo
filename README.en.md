# Haetdachyeo

[한국어](README.md) | [English](README.en.md) | [日本語](README.ja.md)

> Let's say it's implemented.

Haetdachyeo is not a project for implementing applications.

It is a GitHub Organization that **completes the requirements and designs of multiple projects to the point where their implementation outcomes are determined**, then manages each one as a separate GitHub Repository.

Providing the project documentation alone to an AI or developer should be enough for implementation to begin without further questions or additional core design work. The goal is for any implementer to converge on a materially equivalent outcome within predefined boundaries for functionality, external contracts, domain rules, data models, architecture, and operational characteristics.

Once that state has been reached, actual implementation becomes repetitive work that translates an already-decided design into code rather than requiring new decisions. Haetdachyeo does not omit implementation because implementation is impossible; **it intentionally omits implementation because the design has already determined the implementation outcome sufficiently.**

## Why It Exists

Advances in AI have made implementing an application easier than before when requirements and appropriate context are available. However, the following decisions that determine project quality remain the developer's responsibility.

- What to build and what not to build
- How to divide the boundaries of the domain and the system
- How to model the data
- How to define external interfaces and state transitions
- Which architecture, infrastructure, and technologies to choose
- What level of extensibility the current requirements need
- Which alternatives are available and what trade-offs each entails

Haetdachyeo focuses on these decisions that must be made before writing code. Its purpose is not to produce a large volume of documentation, but to **finalize in documentation the decisions required for implementation**.

## Meaning of the Name

**Haetdachyeo** means **“Let's say it's implemented.”**

If the requirements and design are specific enough that AI does not need to decide core matters on its own, and different implementers can produce materially equivalent outcomes, we consider the following to be true.

> At this point, let's say it's implemented.

## Goal

The intended final state of each project is as follows.

> A state in which an AI or developer can implement the project from its documentation alone, without additional questions or core design work

An equivalent outcome does not mean source code that is identical character for character. It means equivalence in the elements that determine product and system behavior, including the following.

- Features provided and items outside the scope
- User-visible behavior and outcomes
- Normal, exceptional, and failure cases, as well as boundary conditions
- Domain rules and state transitions
- API and external system contracts
- Data structures and persistence rules
- System composition and technology choices
- Security, performance, availability, and operational criteria
- Testing and acceptance criteria

Implementation details that do not change the outcome, such as variable names or code organization, may differ. However, decisions that affect the outcome are not left to AI discretion.

## Core Principles

- **Do not implement application code.**
- **Finalize requirements before design and technology choices.**
- **Specify not only the scope, but also what is outside the scope.**
- **Define exceptions, failures, and boundary conditions alongside normal flows.**
- **Leave no core design decisions for AI to infer.**
- **Prioritize simplicity and avoid overengineering.**
- **Provide the extensibility required by confirmed requirements and reasonably foreseeable changes.**
- **Record the rationale, alternatives, and trade-offs for important choices.**
- **Assess completeness by implementability, not by the volume of documentation.**

## What Each Project Designs

Depending on the nature and scale of the project, the following areas are designed to the level required.

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

Projects do not fill every area as a formality or force all content into a single Markdown file. As the content grows, documents are divided by domain or concern.

For example, API documentation may be organized as follows.

```text
api/
├── README.md
├── common.md
├── auth.md
├── users.md
└── projects.md
```

The document structure and level of detail are determined by the project's scale, complexity, and the amount of information required for implementation.

## Recording Design Decisions

Important decisions record more than the final choice. Whenever possible, they include the following.

1. **Problem**: What problem must be solved?
2. **Constraints**: What conditions limit the choice?
3. **Choice**: What was chosen?
4. **Rationale**: Why is this choice appropriate for the current requirements?
5. **Alternatives**: Which options were considered?
6. **Trade-offs**: What is gained and what is given up?

The conclusion “Use PostgreSQL” is not sufficient on its own. The documentation must explain why PostgreSQL was chosen given the current requirements and constraints, why other alternatives were not chosen, and what costs the decision entails.

## Level of Design

Haetdachyeo is not a project that designs every possible future in advance.

It completes the decisions required to implement the current requirements while avoiding structures that would make reasonable future changes excessively difficult. Extensibility is evaluated not against vague possibilities, but on the following grounds.

- Currently confirmed requirements
- Expected usage and quality criteria
- Already identified follow-up requirements
- Areas that are likely to change and costly to modify

Unjustified abstractions and generalizations are not added.

## Role of AI

AI is not the entity that makes a project's major design decisions.

During the design phase, it may be used as a review tool to support the following.

- Identifying missing requirements and ambiguous wording
- Researching design alternatives
- Analyzing advantages, disadvantages, and trade-offs
- Checking for contradictions and omissions across documents
- Structuring documents and improving their wording

During the implementation phase, AI is an executor that translates the design finalized by the developer into code. It must not decide core matters absent from the documentation based on convention or guesswork. If AI must ask additional questions or make decisions that could change the outcome, the issue is not implementation capability; it is that the design is not yet complete.

## Long-Term Goal

The following is not part of the project's immediate scope. It is the long-term direction to pursue once enough design projects have accumulated.

Haetdachyeo's ultimate goal is to extract reusable criteria from the design decisions accumulated across projects and establish **standards for AI development**. Rather than designing every new project from scratch, it should become possible to assemble approaches reviewed and finalized in previous projects according to the current project's conditions.

To support this, Haetdachyeo will accumulate three kinds of assets.

1. **Design templates**: Reusable design units for areas such as APIs, authentication, real-time communication, data storage, deployment, and operations
2. **Decision rules**: Criteria that define the conditions under which each template should be selected or excluded
3. **Composition rules**: Criteria that define dependencies, compatibility, conflicts, and integration among selected templates

The most important of these assets is the **decision rules**. An appropriate design can be selected only by considering not just the required features, but also traffic, budget, security level, clients, operational capability, and quality requirements.

Therefore, Haetdachyeo does not fix a single technology to a single feature with rules such as “a blog uses REST” or “a mobile app uses JWT.” The same feature may require a different choice depending on the project's conditions and constraints. Each template must define its applicable conditions, exclusion conditions, alternatives, and trade-offs.

In the long term, Haetdachyeo aims for the following flow.

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

AI may propose a design draft based on the accumulated standards, but that draft is not automatically considered suitable for a production service. Before implementation, a developer or user must review and confirm that it fits the current project's requirements and constraints.

## GitHub Structure

Each design project is managed as an independent Repository under the GitHub Organization `haetdachyeo`.

```text
haetdachyeo/
├── haetdachyeo
├── project-a
├── project-b
├── project-c
└── ...
```

Each Repository is an independent design project.

### `haetdachyeo/haetdachyeo`

This Repository is not an individual project, but the main Repository that manages standards for the entire Organization.

- The purpose and philosophy of Haetdachyeo
- Project principles
- Required design areas
- Documentation standards
- Project completion criteria
- Project Template
- Standards for using AI

## Completion Criteria

There is one core criterion for project completion.

> Can implementation begin without additional questions or core design work?

If an AI or developer must ask any of the following questions again, the design is considered insufficient.

- What must be built?
- What must not be built?
- How should users and the system behave in each situation?
- How should errors and boundary conditions be handled?
- How is the system divided by structure and responsibility?
- Under what rules is data stored and changed?
- What are the external interfaces, and which contracts do they follow?
- Where and how is the system deployed and operated?
- Which technologies are used, and why were they chosen?
- How is the implementation verified against the requirements?

The **Haetdachyeo completion state** is reached when the project documentation provides consistent and verifiable answers to these questions, leaving no core design discretion to the implementer.
