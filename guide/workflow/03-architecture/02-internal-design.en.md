# Blog Service Internal Design

- Design baseline: [Design Assumptions and Technology Choices](01-design-decisions.en.md). 3 months after first release; 1,000 total users, 50 concurrent, 1 developer/operator

## Design Approach

### Deployment structure: Modular monolith, one Go program

Rationale: Selected to keep one deployment unit manageable by one developer/operator.

### Internal design: Feature separation with Controller, Service, and Repository roles

Rationale: Separate business rules from HTTP/SQL changes. Combining business rules and SQL in request handlers was judged to scatter rules and increase verification effort. Accept representation conversion and interface synchronization costs.

### Feature modules: Independent modules by feature

Rationale: Selected to distinguish data ownership and change scope. Shared layer folders without feature boundaries were rejected because those boundaries are harder to express.

### Request processing: Synchronous; return success after changes commit

Rationale: Selected to keep change completion and visibility to subsequent reads straightforward.

### Module connections: Direct function/method calls

Rationale: Selected to trace calls directly within one program and avoid operating a separate delivery mechanism.

### Implementation language: Go 1.27.3

Rationale: Selected for the developer/operator’s Go skills and deployment as one server program.

### Request handling technology: Go 1.27.3 standard `net/http`

Rationale: Current features were judged to need no separate web framework beyond standard HTTP functionality.

### Database: PostgreSQL 18.6, single authoritative store

Rationale: Selected to manage relational data uniqueness, referential constraints, and transactions in one store.

### Authentication: Server sessions stored in PostgreSQL

Rationale: Selected to validate and revoke the current login state on the server without operating a separate session store.

### Runtime: Docker on a Linux host

Rationale: Follows the project’s Linux/Docker conditions and separates application and database execution units.

### Excluded components: Separate domain layer, generic repository, shared transaction manager, event/command bus, cache, asynchronous query model

Rationale: Abstraction, wiring, and failure tracing costs were judged excessive for current features. Keep only the responsibility separation currently needed.

## Components and Responsibilities

| Component | Role and scope | Boundary |
| --- | --- | --- |
| Controller/Router/Middleware | Parse external requests, validate formats, call Services, convert responses | No business rules or SQL |
| Service | Own business rules, permissions, operation units, models, input/output types, and contracts | No HTTP objects, SQL, or database connections |
| Repository implementation | Implement storage contracts, convert query/constraint results, guarantee atomicity | No response decisions, Service calls, or arbitrary changes to another module’s data |
| External capability implementation | Implement technical integration contracts defined by Services | Do not expose concrete technologies or libraries to Services |
| Application entry point | Validate configuration, create/inject database and implementations, start/stop | No request-specific business rules |

## Dependencies

- Source-code references: Controller → Service; storage/external capability implementations → Service contracts
- Runtime calls: Service → injected implementations. No global database, session, or service lookup
- Component assembly: Create and wire concrete implementations at the application entry point
- Allowed/forbidden dependencies: Services use standard libraries and their own business types/contracts. No direct references to HTTP/database implementations or other business modules; no circular dependencies
- Boundary contracts: Consuming Service module owns contracts. Pass business types, values, conditions, results, and errors; hide SQL, database rows, and transaction objects
- Business rule representation: Module functions/types. No interface for every function or mandatory layer count

## Folder Structure

| Path | Role |
| --- | --- |
| `cmd/` | Entry point and implementation wiring |
| `internal/<module>/` | Placement pattern for feature Services, models, and contracts |
| `internal/transport/` | External request/response handling |
| `internal/storage/` | Repository implementations |
| `internal/adapter/` | External capability implementations |
| `internal/config/` | Runtime configuration loading/validation |
| `migrations/` | Database schema changes |
| `deploy/` | Deployment configuration |

`<module>` is a pattern replaced with a feature name. Concrete feature/file lists belong in feature specifications.

- Entry point: `cmd/`
- Test location: Target package
- Integration test location: `internal/storage/`, storage contract verification

## Key Rules

### Basic flow

- Confirmed value: Controller → Service → Repository → result → response. Reads/writes share business boundaries and the authoritative database

### Input validation

- Confirmed value: Controller validates request format; Service validates business input rules

### Permission checks

- Confirmed value: Pass authenticated caller context to Services; Services decide business permissions

### Data consistency

- Confirmed value: Reads started after a successful change response reflect the change. No separate cache or asynchronous query model

### Errors

- Confirmed value: Storage/external capability implementations convert technical errors to business errors; Controller converts them to external responses. No raw internal errors exposed

### Atomicity

- Confirmed value: Service states the changes that succeed/fail together in its contract. Repository guarantees them with one SQL statement or transaction. Multiple calls are not automatically atomic

### Failure/cancellation

- Confirmed value: No success before confirmed persistence or for partial changes. No automatic mutation retries on failure/uncertain outcomes. Propagate cancellation/deadlines to storage; cancellation/response transmission failure does not undo committed changes

### Structural reconsideration

- Confirmed value: Review changes to responsibilities, dependency directions, or deployment units, including atomic cross-module changes, complex rules, and independent deployment. API additions alone do not change this document; possible future needs alone do not justify extra layers

### Verification

- Confirmed value: Check forbidden/circular dependencies. Verify Service rules/failure propagation without HTTP or a real database. Use a real database for storage contracts, atomicity, error conversion, and change visibility

### Detailed specifications

- Confirmed value: Manage concrete features, data, and API rules in [Feature Behavior Rules](../02-scope.en.md#feature-behavior-rules). Apply the existing module placement pattern to new features. Concrete interfaces and security/deployment/recovery procedures remain subsequent design work

- Verification status: Document design only. No implementation, dependency checks, business tests, or database integration tests performed.
- Implementation readiness: Incomplete; feature/API/database/security/operations specifications contain unresolved items.
