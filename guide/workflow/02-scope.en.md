# Blog Service Scope

## In Scope

The first release provides all of the following features.

- **Registration**: Visitors who are not logged in can create an account with an email address and password.
- **Login**: Visitors who are not logged in can log in with a registered account.
- **Logout**: Logged-in members can log out of their current usage context.
- **Post creation**: Logged-in members can create their own posts with a title and body.
- **Public post list**: Members and visitors can view a list of public posts.
- **Public post details**: Members and visitors can read the title and body of public posts.
- **Post editing**: Logged-in authors can edit the title and body of their own posts.
- **Permanent post deletion**: Logged-in authors can permanently delete their own posts.

## Out of Scope

The following features are not included in the current design or implementation. Their inclusion here is not a commitment to provide them in the future.

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

- **Public visibility**: Posts become public immediately upon creation. Private posts are not supported, and members and visitors who are not logged in can read the same set of public posts.
- **Creation and management permissions**: Only logged-in members can create posts. Only the logged-in author can edit or delete an existing post. Managing other members' posts and transferring ownership are not supported.
- **Editing**: A post's title and body can be edited. Its author, public status, and original creation time remain unchanged.
- **Deletion**: Deletion is permanent. Immediately after deletion, the post is excluded from all public lists and detail views. No one, including the author, can view, edit, restore, or delete it again.
- **Logout**: Logout ends the login state in the current usage context. It does not delete accounts or posts or change other members' login states.
- **External integration**: Integration with external identity providers or external systems is not included.
