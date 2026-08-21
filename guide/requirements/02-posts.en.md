# Post Requirements

[한국어](02-posts.md) | [English](02-posts.en.md) | [日本語](02-posts.ja.md)

← Return to: [Blog Service Requirements](../03-requirements.en.md)

## Common Post Rules

| Item | Value |
| --- | --- |
| Title value | Use input after trimming leading and trailing whitespace |
| Title validation | Required, 1–255 Unicode code points after trimming leading and trailing whitespace |
| Body value | Use input after trimming leading and trailing whitespace |
| Body validation | Required, 1–100,000 Unicode code points after trimming leading and trailing whitespace |
| Validation failure | No new Post creation; no changes to existing Post values |

## Requirement List

- [`FR-POST-001` Create Post](02-posts/FR-POST-001.en.md)
- [`FR-POST-002` View Public Post List](02-posts/FR-POST-002.en.md)
- [`FR-POST-003` View Public Post Details](02-posts/FR-POST-003.en.md)
- [`FR-POST-004` Modify Post](02-posts/FR-POST-004.en.md)
- [`FR-POST-005` Delete Post](02-posts/FR-POST-005.en.md)
