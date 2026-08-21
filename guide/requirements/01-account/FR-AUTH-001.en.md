# `FR-AUTH-001` Sign-up

[한국어](FR-AUTH-001.md) | [English](FR-AUTH-001.en.md) | [日本語](FR-AUTH-001.ja.md)

← Return to: [Account Requirements](../01-account.en.md)

| Item | Value |
| --- | --- |
| Input | Email, password |
| Output / State | Exactly 1 unique Member account created; subsequent sign-in possible with that email and password |
| Validation | Common account rules satisfied; email unique among existing Member emails under case-insensitive comparison |
| Permission | Visitor not signed in |
| Failure / Rejection | No account created for invalid input or duplicate email. Error representation decided in Stage 08 |

| AC ID | Condition | Expected Result |
| --- | --- | --- |
| `AC-AUTH-001-01` | Unused valid email, password of 1–255 Unicode code points | Exactly 1 Member account created |
| `AC-AUTH-001-02` | Only case changed or leading/trailing whitespace added to a registered email | Rejected as duplicate, no new account created |
| `AC-AUTH-001-03` | After trimming, email is exactly 3 / 255 code points and satisfies the format; 0 / 256 code points, or 1–255 but fails `local@domain` format | Former available for sign-up / latter unavailable for sign-up |
| `AC-AUTH-001-04` | Password of 0 / 256 code points; exactly 1 / 255 code points with all other common account rules satisfied | Former unavailable for sign-up / latter available for sign-up |
| `AC-AUTH-001-05` | Password meeting length rule and containing whitespace or Unicode characters | Available for sign-up without changing input value |
