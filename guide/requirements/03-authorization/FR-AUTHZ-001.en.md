# `FR-AUTHZ-001` Ownership and Permission Restrictions

[한국어](FR-AUTHZ-001.md) | [English](FR-AUTHZ-001.en.md) | [日本語](FR-AUTHZ-001.ja.md)

← Return to: [Authorization Requirements](../03-authorization.en.md)

| Item | Value |
| --- | --- |
| Input | Attempt to create a Post or modify or permanently delete an existing public Post; current user identification state |
| Output / State | Only actions allowed by role and Post ownership applied |
| Validation | Ownership belongs to the authoring Member |
| Permission | Member-only actions require sign-in. Modification and permanent deletion of an existing public Post are allowed only to the author |
| Failure / Rejection | Unauthorized attempts do not change target content, public availability, or ownership |

| AC ID | Condition | Expected Result |
| --- | --- | --- |
| `AC-AUTHZ-001-01` | User not signed in | Cannot create Posts or modify or permanently delete existing public Posts |
| `AC-AUTHZ-001-02` | Member attempts to modify or permanently delete another Member's existing public Post | Modification and permanent deletion unavailable; target content, public availability, and ownership unchanged |
| `AC-AUTHZ-001-04` | Unauthorized attempt | Target content, public availability, and ownership unchanged |
