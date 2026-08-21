# `NFR-PERF-001` First-Release Capability Response Time

[한국어](NFR-PERF-001.md) | [English](NFR-PERF-001.en.md) | [日本語](NFR-PERF-001.ja.md)

← Return to: [Quality Requirements](../04-quality.en.md)

| Item | Value |
| --- | --- |
| Input / Target | Successful sign-up, sign-in, sign-out, Post creation, public-list viewing, public-detail viewing, modification, permanent deletion |
| Output / Criterion | For 100 concurrent users, p95 completion time of successful cases per action no more than 2 seconds |
| Measurement start | Service receives action request |
| Measurement end | Product-level result of that action becomes available for use |
| Measurement exclusions | Client network transmission, client rendering, user think time |
| Validation | Successful cases aggregated separately per target action; 95th-percentile completion time measured |
| Permission | Permission of each functional requirement applied |
| Failure / Rejection | Requirement unmet if successful-case p95 exceeds 2 seconds for any action |
| Stage 05 | Define action mix for 100 concurrent users, measurement environment, and load-estimation basis; numerical target cannot change |

| AC ID | Condition | Expected Result |
| --- | --- | --- |
| `AC-PERF-001-01` | Defined load condition, each action aggregated separately | 95th-percentile completion time of successful actions no more than 2 seconds |
