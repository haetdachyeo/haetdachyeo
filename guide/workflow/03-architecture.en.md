# Blog Service Technology and Architecture

## Design Direction

Blog example proposal: **modular monolith**, 1 application and 1 relational database. Separate account and post modules; deploy as a single program.

| Item | Assumption or target |
| --- | --- |
| Expected scale | 3 months after first release: 1,000 total users; 50 concurrent |
| Performance verification | 100 concurrent users; successful requests per action: p95 ≤ 2 seconds |
| Monthly availability | ≥ 99.5% per action |
| Operations | Single region; no separately deployed and managed services |
| Change visibility | Reads reflect completed post creation, editing, and deletion |

Keep deployment and request handling simple. No separate cache, message broker, or search server.

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

| Component | Responsibility |
| --- | --- |
| Client | Request registration, login, and post creation, retrieval, editing, and deletion; display results. Screens and client technologies selected separately |
| Application | HTTPS handling, input validation, member identification, permission checks, account and post management, and responses |
| PostgreSQL | Authoritative store for members, sessions, and posts. Same database for reads and writes |

Internal responsibilities: HTTP requests/responses / accounts/authentication / post management / data access.
Accounts/authentication identifies the current member; post management applies ownership rules for creation, editing, and deletion.

- **Writes**: Return success after changes complete. Define transaction boundaries to prevent partial changes on failure.
- **Public updates**: No deferral to separate asynchronous tasks.
- **First release**: Docker on a Linux host; 1 application container and 1 database container.
- **Failure**: A single-instance failure can interrupt service. Verify restart/recovery procedures and measured availability; expected scale alone does not prove targets are met.

## Technology Summary

| Area | Technology and version | Purpose |
| --- | --- | --- |
| Server language | Go 1.27.3 | Blog implementation |
| HTTP handling | Go 1.27.3 standard `net/http` | HTTPS handling and API responses |
| Database | PostgreSQL 18.6 | Persistent members, sessions, and posts |
| Host operating system | Linux | Docker host |
| Server runtime | Docker | Separate application and database containers |

- **HTTP**: Standard functionality; no separate web framework.
- **Storage**: Single relational database for account uniqueness, post ownership, and change integrity.
- **Login**: Server sessions. Cookie carries the identifier; check session validity and member identity in PostgreSQL.

Details to define: session lifetime, cookie security, password protection, database access library, and specific runtime versions.

## Detailed Design Documents

Location: `03-architecture/`. Reading order: assumptions and technology choices → topic-specific detailed designs.

- [Design Assumptions and Technology Choices](03-architecture/01-design-decisions.en.md): Usage and operational conditions, rationale, verification, and reconsideration criteria.
- [Writing Template (Korean)](03-architecture/01-design-decisions.example.md): Usage assumptions, constraints, quality, rationale, alternatives, trade-offs, verification, and reconsideration.

Detailed design topics:

- **Constraints and operations**: Resources, budget, deployment environment, load, and performance/availability verification.
- **Technology choices**: Technologies, versions, rationale, alternatives, and trade-offs.
- **System and module boundaries**: Responsibilities, dependency directions, request/data flows, transactions, and failure boundaries.
- **Authentication and security**: Passwords, session issuance/expiration/revocation, cookies, permissions, and HTTPS.
- **Storage and operations**: Integrity, concurrent changes, migrations, deployment, recovery, backups, and observability.

Add paths and reading order as detailed documents are written. Each responsible detailed document is authoritative; this overview summarizes structure and major choices.
