# Blog Service Purpose and Description

## Purpose

A basic blog service for members to write and manage posts, and for members and visitors to read public posts.

## Background and Problem

- Need: Authors manage their posts; others read public posts.
- `Undecided`: Existing practices, limitations, and the specific reason for creating this service.

## Target Users

- **Members**: Write, publish, edit, and permanently delete their own posts.
- **Visitors**: Browse public lists and details without registration or login. Logged-in members have the same public access.

## Expected Usage Scale

Design baseline for architecture and code structure.

| Item | Forecast |
| --- | --- |
| Point in time | 3 months after the first release |
| Total users | 1,000 members and visitors combined |
| Concurrent users | 50 |

## Value to Users

- **Members**: Write, manage, and publish posts in one place; only the author can edit or delete them.
- **Visitors**: Read public posts without registration or login.

Improvements over existing practices: specify once the background and problem are concrete. No improvement is established yet.

## Success Criteria

First-release functional verification must meet all conditions:

1. Registration, login, and logout work.
2. Posts created by logged-in members are public immediately, with list and detail access.
3. Members and visitors who are not logged in have the same public access.
4. Author edits appear in public views.
5. After permanent deletion, no one can view or restore the post.
6. Reject creation, editing, and deletion by users who are not logged in, and editing or deletion by other members.

Verification: Exercise normal member and visitor journeys; confirm rejection of changes by users who are not logged in or are not the author.

Quality targets apply separately to 8 actions: registration, login, logout, post creation, public list retrieval, public detail retrieval, editing, and permanent deletion.

| Item | Criterion |
| --- | --- |
| Response time | 100 concurrent users; successful requests per action: p95 ≤ 2 seconds |
| Measurement interval | Service request receipt to result availability |
| Exclusions | Client-side network transfer, rendering, and user think time |
| Monthly availability | ≥ 99.5% per action. Time able to provide normal results for valid, authorized requests / total measurement time over one month |
| Normal rejection | Correct rejection of invalid or unauthorized requests excluded from unavailability |

Measurement environment, action mix for 100 concurrent users, and load estimate basis: finalize during design.
User-value metrics and evaluation timing: `undecided`. Functional and quality compliance does not establish user-value validation.
