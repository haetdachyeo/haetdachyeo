# `FR-AUTH-003` Sign-out

[한국어](FR-AUTH-003.md) | [English](FR-AUTH-003.en.md) | [日本語](FR-AUTH-003.ja.md)

← Return to: [Account Requirements](../01-account.en.md)

| Item | Value |
| --- | --- |
| Input | Sign-out request from current signed-in Member |
| Output / State | Current Member identification ended in that usage state |
| Validation | Current signed-in Member |
| Permission | Signed-in Member only |
| Failure / Rejection | Prohibited when not signed in |

| AC ID | Condition | Expected Result |
| --- | --- | --- |
| `AC-AUTH-003-01` | Signed-in Member signs out | Member-only actions unavailable afterward in same usage state |
| `AC-AUTH-003-02` | One Member signs out | No effect on another Member's signed-in state or data |
