# Account Requirements

[한국어](01-account.md) | [English](01-account.en.md) | [日本語](01-account.ja.md)

← Return to: [Blog Service Requirements](../03-requirements.en.md)

## Common Account Rules

| Item | Value |
| --- | --- |
| Account composition | Email and password combination |
| Email preprocessing | Trim leading and trailing whitespace, then use for validation and account representation |
| Email length | 1–255 Unicode code points after trimming leading and trailing whitespace; email format must also be satisfied, so a valid email has at least 3 code points |
| Email format | No whitespace, exactly 1 `@`, non-empty local and domain parts in `local@domain` form |
| Email comparison | Case-insensitive for both uniqueness and sign-in matching |
| Duplicate email | Duplicate if differing only by case or equal after trimming leading and trailing whitespace |
| Email display | Preserve representation from sign-up after trimming only leading and trailing whitespace; no arbitrary case changes |
| Password preprocessing | Use exact input; no trimming of leading or trailing whitespace or other normalization |
| Password characters | All Unicode characters allowed, including whitespace |
| Password length | 1–255 Unicode code points |
| Password comparison | Exact character-by-character match with sign-up input |

## Requirement List

- [`FR-AUTH-001` Sign-up](01-account/FR-AUTH-001.en.md)
- [`FR-AUTH-002` Sign-in](01-account/FR-AUTH-002.en.md)
- [`FR-AUTH-003` Sign-out](01-account/FR-AUTH-003.en.md)
