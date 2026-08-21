# `FR-POST-004` Modify Post

[한국어](FR-POST-004.md) | [English](FR-POST-004.en.md) | [日本語](FR-POST-004.ja.md)

← Return to: [Post Requirements](../02-posts.en.md)

| Item | Value |
| --- | --- |
| Input | Identification of existing public Post to modify, new title, new body |
| Output / State | Title and body changed, reflected in public views, original creation time retained |
| Validation | New title and body satisfy common Post rules; target is an existing public Post |
| Permission | Signed-in author only |
| Failure / Rejection | Existing title and body retained for invalid input. Modification unavailable when not signed in, not the author, or the target has been permanently deleted and no longer exists |

| AC ID | Condition | Expected Result |
| --- | --- | --- |
| `AC-POST-004-01` | Author enters valid title and body for own public Post | Modification available, changes reflected in public list and details |
| `AC-POST-004-02` | Invalid title or body | Existing title and body retained |
| `AC-POST-004-03` | Visitor not signed in or Member not author | Modification unavailable |
| `AC-POST-004-04` | Permanently deleted Post, including the author's modification attempt | Modification unavailable |
