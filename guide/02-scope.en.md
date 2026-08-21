# Blog Service Scope and Priorities

[한국어](02-scope.md) | [English](02-scope.en.md) | [日本語](02-scope.ja.md)

← Previous: [01. Blog Service Overview](01-overview.en.md)
Next: [03. Blog Service Requirements](03-requirements.en.md) →

## Minimum Scope for the First Release

The first release provides a basic blog where users can sign up, write and manage their own Posts, and Members and Visitors can read public Posts. Sign-in and sign-out are also included to associate ownership of their Posts with the current Member.

All first-release items below are mandatory, not optional.

- Sign-up
- Sign-in and sign-out
- Creating Posts and modifying or permanently deleting the Member's own existing public Posts
- Viewing public Posts by Members and Visitors
- Preventing Members from modifying or deleting other Members' Posts

## In Scope

- Basic user flows required for Member account creation and current Member identification
- Creating Posts and modifying or permanently deleting the Member's own existing public Posts
- Deletion of an existing public Post owned by the Member is permanent; after deletion, no one can view, modify, restore, or re-delete it
- Viewing public Posts
- Limiting the Posts a Member can manage to those they authored

## Out of Scope

The following capabilities are excluded from the current scope because they are not confirmed requirements. This list does not mean that providing these capabilities in the future is planned or promised.

- Comments
- Likes
- Following
- Search
- Categories and tags
- Media uploads
- Drafts and scheduled publishing
- Administrator capabilities
- Social sign-in
- Password recovery
- Email verification
- Account deletion

## Priorities

- Mandatory: All items specified in the minimum scope for the first release
- Out of scope: All items specified in the out-of-scope section

There are no optional sub-priorities among the first-release items.

## Deferred Decisions and Items

The following details are not decided in Stage 02 and will be defined in the corresponding subsequent stages.

- Detailed functional requirements and priorities, inputs, outputs, and validation rules: Stage 03
- Normal, alternative, and failure flows and boundary cases: Stage 04
- API and external interface contracts: Stage 08
- Architecture and module responsibilities: Stage 09
- Database and storage structures: Stage 10
- Infrastructure and deployment method: Stage 11
- Authentication, session, authorization, and security mechanisms: Stage 12

## External Dependencies

No external identity provider, external system, or external organization has been selected or is required for the first release.

## Rationale for the Scope Decision

The first release is limited to the capabilities needed to deliver the core value established in Stage 01 and enforce Post ownership. Post deletion is permanent so that recovery and retention scope is not added, and the scope does not otherwise expand to capabilities whose subsequent requirements and designs have not been confirmed.
