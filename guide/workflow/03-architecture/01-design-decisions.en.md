# Blog Service Design Assumptions and Technology Choices

## Design Assumptions

Scope: Members write/manage posts; visitors read public posts. Expected scale is the design baseline.

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
| Data growth | Title and body: approximately 1MiB/day. Online growth allowance: 2GiB/year, including members, sessions, metadata, and indexes |
| Client | Cookie-based web browser. UI technology selected separately |
| Operating region | South Korea, single region |

Request frequency, action mix, data growth, and region: example assumptions. Average post size: capacity estimate, not an input limit.

## Constraints and Quality Targets

### Runtime and Operations

| Item | Condition |
| --- | --- |
| Host operating system | Linux |
| Execution | Docker; separate application and PostgreSQL containers |
| Initial deployment | 1 host; 1 application and 1 database container |
| Host resources | 4 vCPU, 8GiB RAM, 100GiB SSD |
| Application limit | 1 CPU equivalent, 1GiB RAM |
| Database limit | 2 CPU equivalents, 4GiB RAM |
| Resource headroom | Operating system, Docker, and operational tasks |
| Monthly budget | ≤ KRW 100,000; includes server, storage, backups, and network |
| Development and operations staff | 1 person |
| Skills and time | Basic Go, SQL, Linux, and Docker; routine operations: 2 hours/week |
| Operations | Automated health checks/alerts; respond on receipt. No continuous manual monitoring |

Fixed: Linux and Docker. Example assumptions: resources, budget, and staffing. Not a product quote or measured capacity.
Include deployment, backup, and monitoring costs for added components.

### Quality Targets

Actions: registration, login, logout, post creation, public list retrieval, public detail retrieval, editing, and permanent deletion; 8 in total.

| Item | Criterion |
| --- | --- |
| Response time | 100 concurrent users; successful requests per action: p95 ≤ 2 seconds |
| Measurement interval | Service request receipt to result availability |
| Exclusions | Client-side network transfer, rendering, and user think time |
| Monthly availability | ≥ 99.5% per action. Time able to provide normal results for valid, authorized requests / total time |
| Normal rejection | Correct rejection of invalid or unauthorized requests excluded from unavailability |
| Monthly downtime allowance | 0.5% of total time; 216 minutes for 30 days, including deployment downtime |
| Recovery time | Within 60 minutes of service interruption |
| Recovery point | Full database recovery: recovery point no more than 24 hours old |
| Change visibility | Changes visible to reads started after a successful creation, edit, or deletion response |
| Failure integrity | No successful result for partial changes |

Assess monthly downtime separately from individual recovery time. Verify single-host alerting, backup, and recovery targets through recovery drills.

### Security Conditions

- Credentials and post-changing requests: HTTPS.
- Passwords: no plaintext storage or logging.
- Post editing and deletion: server-side current-member and author ownership checks.
- Database: no direct client access or public service.
- Database credentials/session identifiers: no source-code or ordinary request-log exposure.

Details: password protection, session/cookie security, network access, and secret injection. Owners: security, system, and operations designs.

## Technology and Structural Choices

### Modular Monolith

- **Need**: Separate responsibilities; small-team deployment and incident management.
- **Choice**: Separate accounts/authentication, posts, HTTP, and data access modules; deploy one application.
- **Conditions**: Small-scale usage, 1 operator, limited budget.
- **Rationale**: Internal calls and single-database transactions; no inter-service communication or separate deployment management.
- **Alternatives rejected**: Microservices: contract/deployment/failure overhead. No modules: unclear change boundaries.
- **Costs**: Application-wide deployment/process-failure impact; no independent module deployment or scaling.

### Go and Standard HTTP Handling

- **Need**: HTTP API, account management, and post management.
- **Choice**: Go 1.27.3; standard `net/http` server and routing.
- **Conditions**: Basic account/public post features; operator Go skills.
- **Rationale**: Single server program, standard HTTP, no separate framework conventions.
- **Alternatives rejected**: Go frameworks: no mandatory need. Other languages: no staffing/runtime reason to switch.
- **Costs**: Design error handling, validation, and authentication integration directly. External libraries allowed.

### PostgreSQL

- **Need**: Persist members, sessions, and posts; ensure uniqueness, ownership, and change integrity.
- **Choice**: PostgreSQL 18.6, single authoritative store; same database for reads/writes.
- **Conditions**: Relational data; current results on reads after successful changes.
- **Rationale**: Uniqueness and referential constraints; transactions for related changes.
- **Alternatives rejected**: Containerized PostgreSQL over SQLite. Document stores: no mandatory need. Multiple stores: more consistency/recovery boundaries.
- **Costs**: Database process, connections, migrations, backups; database failure affects the whole application.

### Linux and Docker on a Single Host

- **Need**: Separate runtimes; repeatable deployments.
- **Choice**: 1 Linux host; separate application/database Docker containers. Persist database data independently of container replacement.
- **Conditions**: Fixed Linux/Docker; resource limits, 1 operator, limited budget.
- **Rationale**: Single host; separate execution and replacement units.
- **Alternatives rejected**: Direct installation: violates container deployment conditions. Multi-host orchestration/database redundancy: not adopted for first release.
- **Costs**: Shared host-failure/resource-contention impact. No high-availability guarantee.

System/deployment details: Linux distribution, Docker version, image pinning, network, and volumes.

### Database-Backed Server Sessions

- **Need**: Identify members per request; block member-only actions in the logged-out usage context.
- **Choice**: Cookie carries session identifier; check session in PostgreSQL. Server manages account information, lifetime, and validity.
- **Conditions**: Browser access, logout for the current usage context, single store.
- **Rationale**: Server-side login validity/revocation; no separate session store.
- **Alternatives rejected**: Self-contained tokens: extra logout/revocation rules. Redis sessions: extra store operations.
- **Costs**: Session lookups/expiry cleanup, cookie security, request-forgery protection.

### No Separate Cache, Message Broker, or Search Server

- **Need**: Minimal implementation and operational components.
- **Choice**: Account/post processing only in the application and PostgreSQL.
- **Conditions**: Search and media uploads excluded; immediate visibility of public post changes.
- **Rationale**: Single store; no asynchronous public updates or cache invalidation boundaries.
- **Alternatives deferred**: Cache read distribution, broker task separation, search servers: reconsider once needed.
- **Costs**: Concentrated read/write load. If targets are missed, first inspect queries, indexes, connections, and resources.

## Verification and Reconsideration Criteria

### Test Environment and Load

| Item | Condition |
| --- | --- |
| Server | Separate Linux host; production-equivalent resources, images, and container limits |
| Load generator | Separate host from the server |
| Initial data | 1,000 synthetic accounts; 100,000 public posts; average title and body combined: 10KiB |
| Dataset rationale | Accumulated data exceeding approximately 9,000 posts expected over 3 months |
| Sustained load | 100 virtual users × average 1 request/5 seconds = 20 requests/second |
| Authenticated actions | Valid accounts and sessions; own posts for editing and deletion |
| Registration | Unregistered email addresses |
| Data changes | Record post creations/deletions with measurement results |
| Successful samples | At least 1,000 per action |
| p95 calculation | Sort successful completion times ascending per action; value at rank `ceil(0.95 × sample count)` |
| Unexpected failures | Record valid, authorized request failures separately as functional/stability issues |
| Burst requests | 100 logged-in users; shared start signal; 1 simultaneous valid request each |
| Burst assessment | Check errors/resource saturation separately; never combine with sustained-load p95 |

Mixed load: public list 45%, public detail 45%, login 3%, logout 2%, post creation 2%, editing 2%, registration 0.5%, permanent deletion 0.5%.

8 tested actions are distinct from API counts/paths. Finalize requests after API design.

### Resource, Cost, and Recovery Verification

| Item | Criterion |
| --- | --- |
| Load stability | No out-of-memory termination or container restart |
| CPU | Average host utilization ≤ 70% during sustained load |
| Disk | Review capacity/retention if usage and growth predict 70% utilization within 3 months |
| Cost | Server, storage, backups, and network: total ≤ KRW 100,000/month |
| Recovery drills | Test application interruption and host loss separately. Recover within 60 minutes of interruption; recovery point no more than 24 hours old |
| Backup restoration | Consistent accounts/posts/sessions; no re-exposure of permanently deleted posts |
| Monthly availability | Observe production per action; load/recovery tests alone do not establish compliance |

Retention/restoration rules: define in storage and operations designs.

### Conditions for Reconsideration

| Change or unmet target | Review |
| --- | --- |
| Planned total or concurrent users exceed assumptions | Recalculate requests, data volume, and test load |
| Response time target missed | Inspect handling, queries, indexes, connections, and resources before adding instances or changing structure |
| Availability or recovery targets missed | Single-host placement; backup and recovery operations |
| Staffing or budget changes; independent module deployment required | Deployment units and responsibility boundaries |
| Security or maintenance issues with a selected version | Version change and compatibility verification |
| New feature requiring cache, asynchronous work, or search | Component adoption based on feature and load |

Verification: plan only; implementation, load, cost, and recovery checks not performed.
Decision changes: update the item directly; synchronize the overview.
