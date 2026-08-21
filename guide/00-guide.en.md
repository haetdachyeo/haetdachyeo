# Project Design Guide

[한국어](00-guide.md) | [English](00-guide.en.md) | [日本語](00-guide.ja.md)

This guide defines the sequence for designing a service to the point where it can be implemented without making any additional core design decisions.

It is structured so that even people with limited development experience can avoid overlooking necessary decisions by completing the numbered documents in order. Decisions finalized in earlier documents become inputs for later documents. If a problem is discovered at a later stage, return to the relevant earlier documents and revise them.

## Example Project

The documents that follow in this folder are **examples based on the assumption that a blog service is being designed**.

The examples show what each document should contain and the level of detail required. They do not mean that every service must use the same requirements or technologies as a blog. For an actual project, follow the same sequence and decision criteria, but write content appropriate to the project's purpose and constraints.

## Working Principles

1. Read and complete the numbered documents in order.
2. Base decisions on confirmed goals, requirements, and constraints rather than assumptions.
3. Mark unresolved matters as `미결정` (Undecided), and do not complete any affected later documents until those matters are resolved.
4. If an omission or contradiction is discovered at a later stage, revise the earlier document and review all affected documents again.
5. For important choices, record the rationale, alternatives considered, advantages, disadvantages, and trade-offs.
6. Do not introduce abstractions or large-scale structures in advance for an unconfirmed future.
7. Begin implementation only when all documents are mutually consistent and verifiable.

## Document Sequence

### 01. [`01-overview.md`](01-overview.en.md) — Service Overview

Define why the service is being built, whom it serves, and what value it provides.

Required content:

- The problem to solve and its background
- Target users and key stakeholders
- The core value provided by the service
- Product goals and measurable success criteria
- Key terminology

Completion result: The project's reason for existing and the criteria for determining its success are clear.

### 02. [`02-scope.md`](02-scope.en.md) — Scope and Priorities

Distinguish what will and will not be built in this project.

Required content:

- In-scope and out-of-scope items
- The minimum scope required for the first release
- Priority of each feature
- Items deferred to later stages
- Scope that depends on external systems or organizations

Completion result: The implementation scope does not expand unnecessarily, and what must be omitted is also clear.

### 03. [`03-requirements.en.md`](03-requirements.en.md) — Requirements

Define the functionality and quality the service must provide in verifiable statements.

Required content:

- Functional requirements and unique identifiers
- Non-functional requirements
- Inputs, outputs, and validation rules
- Permitted actions for each permission level
- Requirement priorities
- Acceptance criteria for each requirement

Completion result: Tests can determine whether each requirement has been satisfied.

### 04. `04-user-flows.md` — User Flows and Failure Scenarios

Define how users and the system behave in each situation and what outcomes they receive.

Required content:

- Key user journeys and preconditions
- Normal flows
- Alternative flows
- Input validation failures and insufficient permissions
- External system and internal processing failures
- Cancellation, retries, duplicate requests, and boundary conditions
- Results displayed to users

Completion result: Normal and failure behavior is described without gaps from the beginning to the end of each key feature.

### 05. `05-constraints.md` — Constraints and Quality Goals

Define, with metrics and criteria, the practical conditions that constrain subsequent design and technology choices.

Required content:

- Expected number of users, average and peak traffic, and data growth
- Response-time and throughput goals
- Availability goals and acceptable downtime
- Monthly or annual budget
- Security level and data sensitivity
- Supported clients, regions, and regulations
- Team size, technical capabilities, and available operations time
- Thresholds for determining when to scale

Completion result: A common set of criteria exists for evaluating architecture, databases, infrastructure, and technology choices.

### 06. `06-domain.md` — Domain Model and Business Rules

Define the service's core concepts and rules independently of storage methods or frameworks.

Required content:

- Core domain concepts and terminology
- Relationships and ownership among concepts
- Business rules and invariants
- Conditions for creation, modification, and deletion
- Business-level authorization rules
- Handling of rule violations

Completion result: The service rules that must remain unchanged even when the technology changes are clear.

### 07. `07-state.md` — Data and State Changes

Define which events change the states of domain objects and business processes.

Required content:

- Lifecycles of key data
- Possible states and their meanings
- State-transition conditions and the actors that trigger them
- Disallowed transitions
- Rules for concurrent changes and duplicate processing
- Deletion, retention, recovery, and expiration rules

Completion result: Every state change has starting conditions, outcomes, and failure rules.

### 08. `08-interfaces.md` — APIs and External Interfaces

Define contracts on which clients and external systems can depend.

Required content:

- APIs, events, webhooks, and file interfaces
- Request and response structures
- Authentication and authorization requirements
- Error formats and error codes
- Pagination, idempotency, and rate limiting
- Versioning and compatibility policies
- External service dependencies and behavior during failures

Completion result: Interface consumers can integrate without knowing the internal implementation.

### 09. `09-architecture.md` — Architecture and Module Responsibilities

Define the system structure and responsibility boundaries needed to satisfy the requirements and constraints.

Required content:

- System context and external dependencies
- Responsibilities of components and modules
- Direction of dependencies between modules
- Synchronous and asynchronous communication methods
- Transaction and failure boundaries
- Scaling methods and single points of failure
- Placement of cross-cutting responsibilities such as security, logging, and configuration

Completion result: It is clear which module implements each feature and how the components collaborate.

### 10. `10-database.md` — Database Design

Translate domain and state rules into concrete storage structures and integrity constraints.

Required content:

- The role of each data store
- Tables, collections, fields, and types
- Relationships, keys, constraints, and defaults
- Indexes and key query patterns
- Transaction, consistency, and concurrency handling
- Data retention and deletion methods
- Schema migrations and compatibility
- Backup and recovery requirements

Completion result: Implementers do not need to make additional decisions about storage structures or data-change rules.

### 11. `11-infrastructure.md` — Infrastructure, Deployment, and Networking

Define the environments needed to run the service and change it safely.

Required content:

- Development, test, staging, and production environments
- Compute, storage, and managed service configurations
- Network boundaries, public and private access, and DNS
- Deployment, release, and rollback procedures
- Configuration and secret injection methods
- Capacity planning and scaling methods
- Availability configuration and disaster-recovery objectives
- Estimated costs and cost limits

Completion result: How and where to deploy and recover the service can be reproduced.

### 12. `12-security.md` — Security, Permissions, and Privacy

Define concrete security controls based on the assets and threats that require protection.

Required content:

- Protected assets and trust boundaries
- Anticipated threats and misuse cases
- Authentication, session, and authorization methods
- Role and permission model
- Classification of personal and sensitive information
- Encryption in transit and at rest, and key management
- Secret management
- Audit logs, rate limiting, and abuse prevention
- Incident-response obligations

Completion result: Security requirements are connected to implementable controls and verification items.

### 13. `13-operations.md` — Operations, Observability, and Incident Response

Define how to assess the service's health after deployment and how to detect, respond to, and recover from problems.

Required content:

- Service-level indicators and objectives
- Logs, metrics, and distributed tracing
- Dashboards and alerting conditions
- Health checks and operator functions
- Incident severity levels and response procedures
- Owners and escalation paths
- Backup restoration and disaster-recovery checks
- Cost and capacity monitoring
- Runbooks for recurring tasks

Completion result: Operators can determine whether the service is healthy and respond consistently to incidents.

### 14. `14-decisions.md` — Technology Choices and Decision Records

Track the important design and technology decisions made at each stage in one place, and perform a final review of whether each choice is justified by the conditions identified in earlier documents. Do not defer decisions until stage 14; continuously add each decision to this document at the stage when it is made.

Required content:

- The problem requiring a decision
- Related requirements and constraints
- Alternatives considered
- Final choice and rationale
- Advantages, disadvantages, and trade-offs of each alternative
- Consequences and risks created by the choice
- Conditions under which the decision must be revisited

Completion result: Every important decision is linked to the relevant requirements and constraints, making it possible to explain not only which technology is used but why it was chosen under the current conditions.

### 15. `15-testing.md` — Testing and Acceptance Criteria

Define how to verify that the implementation satisfies all preceding documents.

Required content:

- Traceability between requirements and tests
- Unit, integration, contract, and user-flow tests
- Normal, failure, and boundary cases
- Authorization and security tests
- Performance and load tests
- Migration, backup, and recovery tests
- Test data and execution environments
- Acceptance checklists for each feature and before deployment

Exclude test types that are unnecessary for the nature of the project, but record the reasons for excluding them.

Completion result: Implementation completion and deployment readiness can be determined through repeatable procedures.

### 16. `16-ai-instructions.md` — AI Implementation Instructions

Define the working agreement that AI must follow when translating the preceding design documents into an implementation.

Required content:

- Document reading order and precedence among documents
- Implementation sequence and units of work
- Directory and module organization rules
- Rules for coding, dependencies, and scope of changes
- Matters that AI must not decide at its own discretion
- How to stop and report when omissions, contradictions, or undecided matters are discovered
- Tests and operational artifacts that must be created together with the implementation
- Commands for build, test, static analysis, and deployment verification
- Implementation completion report format

Completion result: AI acts not as a design decision-maker, but as an implementation executor that follows the finalized specification.

## Why Follow This Sequence

The purpose and users must be established first in order to determine the scope, and the scope must be established before verifiable requirements and user flows can be written. Requirements, flows, and constraints are inputs for determining domain rules and state changes.

External contracts must be reviewable independently of the internal implementation, and the architecture must be structured to satisfy the behavior and constraints established earlier. The database translates domain rules and state transitions into storage structures, while infrastructure, security, and operations are the decisions required to run the finalized structure safely in a real environment.

Technologies are not selected before requirements. As necessary conditions emerge at each design stage, compare alternatives and trade-offs, record each choice in the decision record, and at stage 14 perform a final review of the rationale and consistency of all decisions. Finally, testing defines how to verify the entire specification, and the AI implementation instructions convey the finalized documents into implementation.

## When Documents Should Be Split Further

The file list above is the default structure. A document may be split into a subdirectory or multiple documents under the following conditions:

- There are multiple domains that change independently
- API or external integration contracts are large and versioned separately
- Data migrations or security policies require independent review
- Deployment environments or operational procedures are divided among multiple teams

Do not split documents in advance merely because they might be needed in the future. Even after splitting, there must be exactly one authoritative document for each decision.

## Overall Completion Criteria

The complete set of documents written in sequence is the project's final specification. Do not create a separate `final-spec.md` that duplicates the same content.

The design is considered complete only when all of the following conditions are satisfied:

- All required documents have been completed, with no undecided matters remaining.
- Terminology, requirements, states, interfaces, and technology decisions are consistent across documents.
- Every requirement and every failure and boundary condition has a verification method.
- All decisions required to implement, test, deploy, and operate the service are documented.
- Neither AI nor developers need to make any additional product, domain, interface, security, availability, or technology decisions that could materially change the outcome.

Add only a reference link to this guide in the root README. Do not duplicate the guide's content in the README.
