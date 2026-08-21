# `FR-POST-002` View Public Post List

[한국어](FR-POST-002.md) | [English](FR-POST-002.en.md) | [日本語](FR-POST-002.ja.md)

← Return to: [Post Requirements](../02-posts.en.md)

| Item | Value |
| --- | --- |
| Input | Initial or subsequent public-list result request |
| Output / State | Only existing public Posts included |
| Validation | At most 20 per result, original creation time descending; if over 20, every public Post retrievable without omission through subsequent results |
| Permission | Visitors and signed-in Members allowed, same public scope |
| Failure / Rejection | Permanently deleted Posts are absent from all public-list results |

| AC ID | Condition | Expected Result |
| --- | --- | --- |
| `AC-POST-002-01` | Public Posts have different original creation times | Descending from newest original creation time |
| `AC-POST-002-02` | 21 or more public Posts | At most 20 per result; Posts absent from first result retrievable through subsequent results |
| `AC-POST-002-03` | Visitor or signed-in Member | Same public scope applied |
| `AC-POST-002-04` | Public Post permanently deleted | Absent from all public-list results immediately after deletion |
