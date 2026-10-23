# Blog Service Design Assumptions and Technology Choices

## Design Assumptions

Scope: Members writing and managing posts; visitors reading public posts. Expected scale as the design baseline.

| Item | Assumption |
| --- | --- |
| Forecast point | 3 months after the first release |
| Total users | 1,000 members and visitors combined |
| Concurrent users | 50 |
| Primary usage | Public post lists and details |
| Action mix | Reads 90%; account operations and post changes 10% |
| Average request rate | 1 request/second = 10 active users × 1 request/10 seconds |
| Busy-period request rate | 10 requests/second = 50 active users × 1 request/5 seconds |
| Burst request rate | 50 requests/second = 50 active users × 1 request/second |
| Post growth | 100 posts/day |
| Average post size | Title and body combined: 10KiB |
| Data growth | Title and body: approximately 1MiB/day. Online storage growth allowance, including members, sessions, metadata, and indexes: 2GiB/year |
| Client | Cookie-based web browser. UI technology selected separately |
| Operating region | South Korea, single region |

Request frequency, action mix, data growth, and operating region: example assumptions. Average post size: capacity planning, not an input limit.

## Constraints and Quality Targets

### Runtime and Operations

| Item | Condition |
| --- | --- |
| Host operating system | Linux |
| Execution | Docker; separate application and PostgreSQL containers |
| Initial deployment | 1 host; 1 application container and 1 database container |
| Host resources | 4 vCPU, 8GiB RAM, 100GiB SSD |
| Application limit | 1 CPU equivalent, 1GiB RAM |
| Database limit | 2 CPU equivalents, 4GiB RAM |
| Resource headroom | Operating system, Docker, and operational tasks |
| Monthly budget | Up to KRW 100,000; server, storage, backups, and network included |
| Development and operations staff | 1 person |
| Skills and time | Basic Go, SQL, Linux, and Docker skills; 2 hours/week for routine operations |
| Operations | Automated health checks and alerts; response upon alert receipt. No continuous manual monitoring |

Fixed conditions: Linux and Docker. Example assumptions: resources, budget, and staffing. Not a product quote or measured capacity.
Additional components: account for deployment, backup, and monitoring costs.

### Quality Targets

Actions: registration, login, logout, post creation, public list retrieval, public detail retrieval, editing, and permanent deletion; 8 in total.

| Item | Criterion |
| --- | --- |
| Response time | 100 concurrent users; successful requests per action: p95 ≤ 2 seconds |
| Measurement interval | Service request receipt to result availability |
| Exclusions | Client-side network transfer, rendering, and user think time |
| Monthly availability | ≥ 99.5% per action. Time able to provide normal results for valid, authorized requests / total time |
| Normal rejection | Correct rejections for invalid input or insufficient permissions excluded from unavailability |
| Monthly downtime allowance | 0.5% of total time. 216 minutes for a 30-day month, including deployment downtime |
| Recovery time | Within 60 minutes of service interruption |
| Recovery point | For full database recovery: a recovery point no more than 24 hours old |
| Change visibility | Changes visible to reads started after a successful creation, edit, or deletion response |
| Failure integrity | No successful result for partial changes |

Monthly downtime allowance: assessed separately from individual recovery time compliance.
Single-host alerting, backup, and recovery targets: verified through recovery drills.

### Security Conditions

- Credentials and post-changing requests: HTTPS.
- Passwords: no plaintext storage or logging.
- Post editing and deletion: server-side current-member and author ownership checks.
- Database: no direct client access or public service exposure.
- Database credentials and session identifiers: no exposure in source code or ordinary request logs.

Details to define: password protection, session and cookie security, network access, and secret injection.
Ownership: security, system, and operations designs.

## Technology and Structural Choices

### Modular Monolith

- **Need**: Separate functional responsibilities; deployment and incident management by a small team.
- **Choice**: Separate account/authentication, post management, HTTP handling, and data access modules. Single application deployment.
- **Conditions**: Small-scale usage, 1 operator, limited budget.
- **Rationale**: Internal calls and single-database transactions; no separate inter-service communication or deployment management.
- **Alternatives rejected**: Microservices: contract, deployment, and failure management overhead. No module separation: unclear change boundaries.
- **Costs**: Application-wide deployment and process-failure impact. No independent module deployment or scaling.

### Go and Standard HTTP Handling

- **Need**: HTTP API, account management, and post management.
- **Choice**: Go 1.27.3; standard `net/http` server and routing.
- **Conditions**: Basic account and public post features; operator proficiency in Go.
- **Rationale**: Single server program, standard HTTP functionality, no separate framework conventions.
- **Alternatives rejected**: Go web frameworks: no mandatory requirement. Other languages: no current staffing or runtime need to switch.
- **Costs**: Custom design for integrating errors, input validation, and authentication. External libraries permitted.

### PostgreSQL

- **Need**: Persistent members, sessions, and posts; uniqueness, ownership relationships, and change integrity.
- **Choice**: PostgreSQL 18.6 as the single authoritative store. Same database for reads and writes.
- **Conditions**: Relational data; current results on reads after successful changes.
- **Rationale**: Uniqueness and referential constraints; transactions for related changes.
- **Alternatives rejected**: Independently containerized PostgreSQL chosen over SQLite. Document stores: no mandatory requirement. Multiple stores: additional consistency and recovery boundaries.
- **Costs**: Database process, connections, migrations, and backups. Application-wide impact from a single database failure.

### Linux and Docker on a Single Host

- **Need**: Separate execution environments; repeatable deployment configuration.
- **Choice**: 1 Linux host; separate application and database Docker containers. Database persistence independent of container replacement.
- **Conditions**: Fixed Linux/Docker environment; resource limits, 1 operator, limited budget.
- **Rationale**: Single host; separate execution and replacement units.
- **Alternatives rejected**: Direct installation: incompatible with container deployment conditions. Multi-host orchestration and database redundancy: not adopted for the first release.
- **Costs**: Shared host-failure and resource-contention impact. No high-availability guarantee.

System and deployment details: Linux distribution, Docker version, image pinning, network, and volume configuration.

### Database-Backed Server Sessions

- **Need**: Identify members per request; block member-only actions in the affected usage context after logout.
- **Choice**: Session identifier sent via cookie; session checked in PostgreSQL. Account information, session lifetime, and validity managed by the server.
- **Conditions**: Browser access, logout for the current usage context, single store.
- **Rationale**: Server-side login validity and revocation; no separate session store.
- **Alternatives rejected**: Self-contained tokens: additional logout and revocation rules. Redis sessions: additional store operations.
- **Costs**: Session lookups, expired-session cleanup, cookie security, and request-forgery protection.

### No Separate Cache, Message Broker, or Search Server

- **Need**: Minimal implementation and operational components.
- **Choice**: Account and post processing limited to the application and PostgreSQL.
- **Conditions**: Search and media uploads excluded; immediate visibility of public post changes.
- **Rationale**: Single store without asynchronous public updates or cache invalidation boundaries.
- **Alternatives deferred**: Cache-based read distribution, broker-based task separation, and search servers; reconsider when a need is established.
- **Costs**: Concentrated read/write load. On target failure, first inspect queries, indexes, connections, and resource allocation.

## Verification and Reconsideration Criteria

### Test Environment and Load

| Item | Condition |
| --- | --- |
| Server | Separate Linux host; same resources, images, and container limits as production |
| Load generator | Separate host from the server |
| Initial data | 1,000 synthetic accounts; 100,000 public posts; average title and body combined: 10KiB |
| Dataset rationale | Accumulated-data condition exceeding approximately 9,000 posts expected over 3 months |
| Sustained load | 100 virtual users × average 1 request/5 seconds = 20 requests/second |
| Authenticated actions | Valid accounts and sessions; own posts for editing and deletion |
| Registration | Unregistered email addresses |
| Data changes | Record post creations and deletions alongside measurement results |
| Successful samples | At least 1,000 per action |
| p95 calculation | Successful completion times sorted ascending per action; value at rank `ceil(0.95 × sample count)` |
| Unexpected failures | Failures of valid, authorized requests recorded separately as functional or stability issues |
| Burst requests | 100 logged-in users; shared start signal; 1 valid request each, sent simultaneously |
| Burst assessment | Errors and resource saturation checked separately. Not combined with sustained-load p95 |

Mixed load: public list 45%, public detail 45%, login 3%, logout 2%, post creation 2%, editing 2%, registration 0.5%, permanent deletion 0.5%.

8 tested actions are separate from API counts and paths. Actual requests finalized after API design.

### Resource, Cost, and Recovery Verification

| Item | Criterion |
| --- | --- |
| Load stability | No out-of-memory termination or container restart |
| CPU | Average host utilization ≤ 70% during sustained load |
| Disk | Review capacity and retention if current usage and growth predict 70% utilization within 3 months |
| Cost | Server, storage, backups, and network: total ≤ KRW 100,000/month |
| Recovery drills | Application interruption and host loss tested separately. Recovery within 60 minutes of interruption; recovery point no more than 24 hours old |
| Backup restoration | Account, post, and session consistency; no re-exposure of permanently deleted posts |
| Monthly availability | Per-action production observation. Load and recovery tests alone insufficient to establish compliance |

Retention and restoration rules: defined in storage and operations designs.

### Conditions for Reconsideration

| Change or unmet target | Review |
| --- | --- |
| Planned total or concurrent users exceed assumptions | Recalculate request volume, data volume, and test load |
| Response time target missed | Inspect request handling, queries, indexes, connections, and resources before increasing instances or changing structure |
| Availability or recovery targets missed | Single-host placement; backup and recovery operations |
| Staffing or budget changes; independent module deployment required | Deployment units and responsibility boundaries |
| Security or maintenance issues with a selected version | Version change and compatibility verification |
| New feature requiring cache, asynchronous work, or search | Component adoption based on feature and load |

Verification status: plan only. Implementation, load measurement, cost checks, and recovery drills not performed.
Decision changes: update the relevant item directly; synchronize the overview summary.
