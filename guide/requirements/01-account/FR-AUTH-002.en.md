# `FR-AUTH-002` Sign-in

[한국어](FR-AUTH-002.md) | [English](FR-AUTH-002.en.md) | [日本語](FR-AUTH-002.ja.md)

← Return to: [Account Requirements](../01-account.en.md)

| Item | Value |
| --- | --- |
| Input | Email, password |
| Output / State | Member identified as current signed-in Member; Member-only actions available |
| Validation | Email matched case-insensitively after trimming leading and trailing whitespace; password exact match without preprocessing or normalization |
| Permission | Visitor not signed in |
| Failure / Rejection | No signed-in state created for nonexistent account or password mismatch. Error representation decided in Stage 08 |

| AC ID | Condition | Expected Result |
| --- | --- | --- |
| `AC-AUTH-002-01` | Registered email with case changed or leading/trailing whitespace added, exact password | Signed in as same Member |
| `AC-AUTH-002-02` | Any one-character change to registered password, including removing or adding leading/trailing whitespace | Sign-in unavailable |
| `AC-AUTH-002-03` | Unregistered email or incorrect password | No signed-in state enabling Member-only actions created |
