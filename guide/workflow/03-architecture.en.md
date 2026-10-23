# Blog Service Technology and Architecture

## Design Direction

The blog service consists of one application and one relational database. Account and post management are separated into modules within the application, using a modular monolith deployed as a single program.

The technologies and structure in this document are a proposed design for the blog example. The design assumes the following conditions.

- Three months after the first release, the service is expected to have 1,000 total users and 50 concurrent users.
- Performance verification uses 100 concurrent users, with a p95 completion time of no more than 2 seconds for successful requests, measured separately for each action.
- The monthly availability target for each action is at least 99.5%.
- The first release operates in a single region and does not use a structure involving multiple separately deployed and managed services.
- Public views must reflect post creation, editing, and deletion once processing completes.

Keep deployment and request processing simple by placing account and post management in one application. Do not introduce a separate cache, message broker, or search server.

## System Overview

```text
Client
    |
    | HTTPS
    v
Blog Application
    |
    | SQL
    v
PostgreSQL
```

- **Client**: Requests registration, login, and post creation, retrieval, editing, and deletion, and displays the results. Specific screens and client technologies are outside this server configuration.
- **Blog application**: Handles HTTPS requests, input validation, user identification, permission checks, account and post management, and response generation.
- **PostgreSQL**: The authoritative store for member accounts, login sessions, and posts. Post queries use the same database in which changes have been applied.

Within the application, separate the responsibilities of HTTP request and response handling, accounts and authentication, post management, and data access. The account and authentication module identifies the current member, while the post management module applies ownership rules for creation, editing, and deletion.

For write requests, return success only after the necessary data changes complete. Define transaction boundaries so that processing failures do not leave partial changes. Do not defer updates to public views to a separate asynchronous task.

For the first release, run one application container and one database container with Docker on a Linux host. A single-instance failure can interrupt the service, so verify operational targets through restart and recovery procedures and actual availability measurements. Expected usage scale alone does not establish that performance or availability targets are met.

## Technology Summary

| Area | Technology and Version | Purpose |
| --- | --- | --- |
| Server language | Go 1.27.3 | Blog application implementation |
| HTTP handling | Go 1.27.3 standard library `net/http` | HTTPS request handling and API responses |
| Database | PostgreSQL 18.6 | Persistent storage of members, login sessions, and posts |
| Host operating system | Linux | Host for running Docker |
| Server runtime environment | Docker | Run the application and database in separate containers |

Start with standard HTTP functionality without a separate web framework. Consolidate storage in one relational database to manage account uniqueness, post ownership, and the integrity of data changes.

Use server-managed sessions for login. The client sends a session identifier in a cookie, and the server checks the session's validity and identifies the member. Session lifetimes, cookie security attributes, password protection, the database access library, and specific runtime environment versions will be defined in the detailed design.

## Detailed Design Documents

Detailed designs are organized by topic in the `03-architecture/` folder. Start with design assumptions and technology choices, then proceed to the detailed designs for individual topics.

- [Blog Service Design Assumptions and Technology Choices](03-architecture/01-design-decisions.en.md): Usage and operational conditions, rationale for technology and structural choices, verification, and criteria for reconsidering decisions.
- [Design Assumptions and Technology Choices Template (Korean)](03-architecture/01-design-decisions.example.md): Expected usage, constraints and quality targets, selection rationale, alternatives, trade-offs, verification, and criteria for reconsidering decisions.

The detailed design will cover the following topics.

- **Constraints and operational criteria**: Server resources, budget, deployment environment, load composition, and conditions for verifying performance and availability.
- **Technology choices**: Reasons for choosing technologies and versions, alternatives considered, and trade-offs.
- **System and module boundaries**: Component responsibilities, internal dependency directions, request and data flows, and transaction and failure boundaries.
- **Authentication and security**: Password protection, session issuance, expiration and revocation, cookie security, permission checks, and HTTPS configuration.
- **Storage and operations**: Data integrity, concurrent changes, migrations, deployment and recovery, backups, and observability.

Once detailed documents are written, list their actual paths and reading order in this section. Each design item will have its authoritative definition in the responsible detailed document, while this overview remains a summary of the overall structure and major choices.
