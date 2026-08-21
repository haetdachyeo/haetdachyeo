# `FR-POST-001` Create Post

[한국어](FR-POST-001.md) | [English](FR-POST-001.en.md) | [日本語](FR-POST-001.ja.md)

← Return to: [Post Requirements](../02-posts.en.md)

| Item | Value |
| --- | --- |
| Input | Title, body |
| Output / State | Exactly 1 new Post authored by current Member, immediately public, retaining original creation time |
| Validation | Common Post rules satisfied |
| Permission | Signed-in Member only |
| Failure / Rejection | No creation for invalid input or when not signed in |

| AC ID | Condition | Expected Result |
| --- | --- | --- |
| `AC-POST-001-01` | Signed-in Member, valid title and body | Exactly 1 own public Post created |
| `AC-POST-001-02` | Title or body empty after trimming leading and trailing whitespace | No Post created |
| `AC-POST-001-03` | After trimming, title exactly 1 / 255 and body exactly 1 / 100,000 code points; title 256 or body 100,001 | Former creatable / latter not creatable |
| `AC-POST-001-04` | Visitor not signed in, valid title and body | No Post created |
