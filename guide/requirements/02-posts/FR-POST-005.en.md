# `FR-POST-005` Delete Post

[한국어](FR-POST-005.md) | [English](FR-POST-005.en.md) | [日本語](FR-POST-005.ja.md)

← Return to: [Post Requirements](../02-posts.en.md)

| Item | Value |
| --- | --- |
| Input | Identification of existing public Post to delete |
| Output / State | Post permanently deleted so that no one can later view, modify, restore, or re-delete it |
| Validation | Only an existing public Post targeted |
| Permission | Only a signed-in author deleting an owned existing public Post |
| Failure / Rejection | Unauthorized attempts by a user not signed in or not the author, and attempts against unavailable targets, make no state change. Error representation is defined in Stage 08 |

| AC ID | Condition | Expected Result |
| --- | --- | --- |
| `AC-POST-005-01` | Signed-in author deletes an owned existing public Post | Permanently deleted, excluded from public list and details, and cannot later be viewed, modified, restored, or re-deleted |
| `AC-POST-005-02` | Deletion attempt by a Visitor not signed in or a Member not the author | Deletion unavailable; target content, public availability, and ownership unchanged |
| `AC-POST-005-03` | Re-deletion attempt against a permanently deleted Post that no longer exists | Re-deletion unavailable, no state change |
