# `NFR-AVAIL-001` First-Release Capability Monthly Availability

[한국어](NFR-AVAIL-001.md) | [English](NFR-AVAIL-001.en.md) | [日本語](NFR-AVAIL-001.ja.md)

← Return to: [Quality Requirements](../04-quality.en.md)

| Item | Value |
| --- | --- |
| Input / Target | Sign-up, sign-in, sign-out, Post creation, public-list viewing, public-detail viewing, modification, permanent deletion |
| Output / Criterion | Monthly availability at least 99.5% per target action |
| Availability definition | Product-level result in this document can be provided for valid, authorized request |
| Unavailability definition | Any time a target action cannot provide its defined product-level result is downtime for that action |
| Validation | Available time / total measurement time measured per target action over one month |
| Permission | Permission of each functional requirement applied |
| Failure / Rejection | Requirement unmet if monthly availability is below 99.5% for any target action |

| AC ID | Condition | Expected Result |
| --- | --- | --- |
| `AC-AVAIL-001-01` | One month, measured per target action | Available time / total measurement time at least 99.5% |
