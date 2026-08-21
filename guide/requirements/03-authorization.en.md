# Authorization Requirements

[한국어](03-authorization.md) | [English](03-authorization.en.md) | [日本語](03-authorization.ja.md)

← Return to: [Blog Service Requirements](../03-requirements.en.md)

## Requirement List

- [`FR-AUTHZ-001` Ownership and Permission Restrictions](03-authorization/FR-AUTHZ-001.en.md)

## Permission Matrix

| Action | Visitor Not Signed In | Member Signed In | Ownership / State Condition |
| --- | --- | --- | --- |
| Sign-up | Allowed | Not applicable | Not applicable |
| Sign-in | Allowed | Not applicable | Not applicable |
| Sign-out | Prohibited | Allowed | Not applicable |
| View public Post list/details | Allowed | Allowed | Public state, regardless of ownership |
| Create Post | Prohibited | Allowed | Current Member becomes new Post author |
| Modify/permanently delete existing public Post | Prohibited | Conditionally allowed | Author, public state |
