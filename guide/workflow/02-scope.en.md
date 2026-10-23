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
