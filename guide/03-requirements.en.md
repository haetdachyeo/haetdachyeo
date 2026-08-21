# Blog Service Requirements

[한국어](03-requirements.md) | [English](03-requirements.en.md) | [日本語](03-requirements.ja.md)

← Previous: [02. Blog Service Scope and Priorities](02-scope.en.md)

## Application

| Item | Value |
| --- | --- |
| Basis | Stage 01 product goals, Stage 02 first-release scope |
| Target | All functional and non-functional requirements in the Stage 03 requirement document set |
| Priority | All Mandatory, no optional sub-priorities |

## Roles

| Role | Identification | Allowed Scope |
| --- | --- | --- |
| Visitor | Regardless of sign-in status | Public Post list and detail viewing. When not signed in, Member-only actions prohibited except sign-up and sign-in |
| Member | Sign-up completed with a unique email and password, currently signed in | Same public viewing as Visitor. While signed in: create Posts, modify or permanently delete own existing public Posts, sign out |

## Requirement Documents

1. [Account Requirements](requirements/01-account.en.md)
2. [Post Requirements](requirements/02-posts.en.md)
3. [Authorization Requirements](requirements/03-authorization.en.md)
4. [Quality Requirements](requirements/04-quality.en.md)

## Requirement Index

| Requirement ID | Function / Quality | Role / Target | Priority |
| --- | --- | --- | --- |
| `FR-AUTH-001` | [Sign-up](requirements/01-account/FR-AUTH-001.en.md) | Visitor | Mandatory |
| `FR-AUTH-002` | [Sign-in](requirements/01-account/FR-AUTH-002.en.md) | Visitor | Mandatory |
| `FR-AUTH-003` | [Sign-out](requirements/01-account/FR-AUTH-003.en.md) | Member | Mandatory |
| `FR-POST-001` | [Create Post](requirements/02-posts/FR-POST-001.en.md) | Member | Mandatory |
| `FR-POST-002` | [View public Post list](requirements/02-posts/FR-POST-002.en.md) | Visitor, Member | Mandatory |
| `FR-POST-003` | [View public Post details](requirements/02-posts/FR-POST-003.en.md) | Visitor, Member | Mandatory |
| `FR-POST-004` | [Modify Post](requirements/02-posts/FR-POST-004.en.md) | Member | Mandatory |
| `FR-POST-005` | [Delete Post](requirements/02-posts/FR-POST-005.en.md) | Member | Mandatory |
| `FR-AUTHZ-001` | [Ownership and permission restrictions](requirements/03-authorization/FR-AUTHZ-001.en.md) | Visitor, Member | Mandatory |
| `NFR-PERF-001` | [First-release capability response time](requirements/04-quality/NFR-PERF-001.en.md) | Key actions | Mandatory |
| `NFR-AVAIL-001` | [First-release capability monthly availability](requirements/04-quality/NFR-AVAIL-001.en.md) | Key actions | Mandatory |

## Stage Boundaries and Deferred Decisions

| Stage | Owned Decision | Fixed Condition |
| --- | --- | --- |
| Stage 02 | Excluded scope | Maintain stated excluded capabilities; no additions through the Stage 03 requirement document set |
| Stage 04 | Sequence of normal, alternative, and failure flows; user-system interactions; boundary cases | Requirements in the Stage 03 requirement document set cannot change |
| Stage 05 | Expected usage, traffic composition, data growth, quality-target measurement conditions, action mix for 100 concurrent users, measurement environment, load-estimation basis | 100 concurrent users, p95 no more than 2 seconds, monthly availability at least 99.5% cannot change |
| Stage 08 | Request/response structures, API formats, error formats/codes, exact subsequent-result retrieval method for the public list | Public list: at most 20 per result, original creation time descending, every target Post retrievable through subsequent results |
| Stage 09 | Architecture, module responsibilities | Not selected in this document |
| Stage 10 | Database schema, integrity rules, storage/removal method for permanent deletion | Not selected in this document |
| Stage 12 | Specific security mechanisms for authentication, sessions, password protection, authorization | Not selected in this document |

| Deferred Decision | Status |
| --- | --- |
| HTTP method | Not selected |
| API schema | Not selected |
| Status/error codes | Not selected, Stage 08 |
| Authentication token/session method | Not selected, Stage 12 |
| Password hashing method | Not selected, Stage 12 |
| Database structure | Not selected, Stage 10 |
| Storage/removal method for permanent deletion | Not selected, Stage 10 |
| Architecture | Not selected, Stage 09 |
| UI | Not selected |
| Exact subsequent-result retrieval method for the public list | Not selected, Stage 08 |
