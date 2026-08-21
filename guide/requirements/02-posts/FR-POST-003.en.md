# `FR-POST-003` View Public Post Details

[한국어](FR-POST-003.md) | [English](FR-POST-003.en.md) | [日本語](FR-POST-003.ja.md)

← Return to: [Post Requirements](../02-posts.en.md)

| Item | Value |
| --- | --- |
| Input | Information identifying a specific Post |
| Output / State | Existing public Post details including title and body |
| Validation | Only existing public Posts targeted |
| Permission | Visitors and signed-in Members allowed |
| Failure / Rejection | Details of permanently deleted Posts unavailable to everyone, including the author |

| AC ID | Condition | Expected Result |
| --- | --- | --- |
| `AC-POST-003-01` | Public Post, Visitor or signed-in Member | Details viewable |
| `AC-POST-003-02` | Public Post permanently deleted | Details unavailable to everyone, including the author, immediately after deletion |
