# Blog Service Scope

## In Scope

Required for the first release.

| Feature | Actor | Capability |
| --- | --- | --- |
| Registration | Visitor not logged in | Create an account with email and password |
| Login | Visitor not logged in | Log in with a registered account |
| Logout | Logged-in member | End login in the current usage context |
| Post creation | Logged-in member | Create their own post with a title and body |
| Public list | Member or visitor | View the list of public posts |
| Public details | Member or visitor | Read a public post's title and body |
| Post editing | Logged-in author | Edit their own post's title and body |
| Permanent deletion | Logged-in author | Permanently delete their own post |

## Out of Scope

Excluded from current design and implementation; no promise of future delivery.

- Comments
- Likes
- Following
- Search
- Categories and tags
- Media uploads, including images and videos
- Draft saving and scheduled publishing
- Administrative features
- Social login
- Password recovery
- Email verification
- Account deletion

## Scope Boundaries

- **Public visibility**: Public immediately on creation. No private posts. Members and visitors who are not logged in have the same public access.
- **Permissions**: Only logged-in members can create posts; only logged-in authors can edit or delete their own posts. Managing others' posts and ownership transfers are excluded.
- **Editing**: Title and body only. Author, public status, and original creation time remain unchanged.
- **Deletion**: Permanent; immediately removed from all public lists and details. No one, including the author, can view, edit, restore, or delete it again.
- **Logout**: Ends login only in the current usage context. Accounts, posts, and other members' login states remain unchanged.
- **External integration**: External identity providers and systems are excluded.

## Feature Behavior Rules

Authority for feature behavior and data. Common responsibilities and placement principles belong in [Internal Design](03-architecture/02-internal-design.en.md).

### Data Responsibilities

- Accounts/authentication owns member/session/input/result types and rules. Posts owns post/input/result types, input rules, and author permissions.
- Posts retain only the author member identifier. No direct account-storage/session lookups or copying of password/session information.
- Password protection takes password/stored-hash inputs for creation/verification. Hash format/algorithm stays within the implementation. No plaintext storage/logging. Security details finalize algorithm and cost.

### Authentication and Sessions

- Login: Account lookup/password verification → completed session persistence → identifier return/cookie response. No automatic login added on registration.
- Protected actions: Verify the session before passing the member identifier. Do not trust a body-supplied member identifier; reject invalid sessions without calling mutations. Public lists/details require no session.
- Logout: Persist current-session revocation → remove cookie/success response. Keep other sessions, accounts, and posts.

### Post Changes

- Check input/author permissions before mutation. Edit/delete storage contracts receive post/current-member identifiers; recheck existence/ownership during the actual mutation.
- Reads started after a success response reflect changes. The scope boundaries also prohibit reading, editing, restoring, or deleting a deleted post again.

### Storage and Errors

- Atomic storage units: Member creation, session creation, current-session revocation, post creation/editing/deletion, each separately. Follow Internal Design for the shared atomicity mechanism.
- Storage errors: Classify duplicates, missing targets, access denied, and storage failure. Map input/authentication/permission/target/processing failures to API responses. Subsequent API/database specifications finalize error precedence, status codes, and constraint mappings.

### Verification Conditions and Subsequent Decisions

- Request verification: Reject unauthenticated/others' post changes, allow public reads, logout only the current session, and reflect successful changes in subsequent reads.
- Database verification: Atomicity, duplicates, missing targets, ownership conditions, and error classification. No implementation, functional, or database integration verification performed.
- Unresolved: Concrete interfaces; field-level input validation, error precedence, status codes; database schema, constraint/error mappings, concurrent edits, isolation levels, duplicate requests; authentication/security settings; deployment/operations/deletion guarantees after restoration. Finalize in responsible detailed specifications and synchronize related documents.
