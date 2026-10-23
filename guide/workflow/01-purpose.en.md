# Blog Service Purpose and Description

## Purpose

Build a basic blog service where members can write and manage their posts in one place, and both members and visitors can read public posts.

## Background and Problem

This service is for members who write and manage posts and visitors who read public posts. Authors must be able to manage their own posts, while other users must be able to read the public content.

Existing usage practices, their limitations, and the specific reason for creating this service are `undecided`.

## Target Users

- **Members**: Users who want to write and publish their own posts, and edit or permanently delete existing posts.
- **Visitors**: Users who want to browse public post lists and read post details without registering or logging in. Logged-in members can read the same public posts.

## Expected Usage Scale

The blog service's architecture and code structure are determined based on the following expected usage scale.

- **Forecast point**: Three months after the first release.
- **Expected total users**: 1,000, including members who write posts and visitors who read them.
- **Expected concurrent users**: 50.

## Value to Users

- **Members**: They can write and manage their posts in one place and make them available to others. Only the author can edit or delete their own posts.
- **Visitors**: They can read public posts without creating an account or logging in.

Improvements over existing practices must be described once the background and problem are more concrete. This document alone does not establish any specific improvement.

## Success Criteria

Functional verification for the first release must confirm all of the following:

1. Users can register, log in, and log out.
2. When a logged-in member creates a post, it becomes public immediately and can be viewed in the list and detail views.
3. Members and visitors who are not logged in can read the same set of public posts.
4. When an author edits their own post, public views reflect the changes.
5. When an author permanently deletes their own post, no one can view or restore it afterward.
6. Users who are not logged in cannot create, edit, or delete posts, and other members cannot edit or delete the author's posts.

Verification consists of carrying out normal member and visitor journeys and checking that modification attempts by users who are not logged in and by other members are rejected.

For the first release, the following quality criteria apply separately to each of eight actions: registration, login, logout, post creation, public list retrieval, public detail retrieval, editing, and permanent deletion.

- **Response time**: With 100 concurrent users, the p95 completion time must be no more than 2 seconds when successful requests are aggregated separately for each action. Measure from the time the service receives a request until its result becomes available, excluding client-side network transfer, rendering, and user think time.
- **Monthly availability**: Each action must achieve at least 99.5% monthly availability. Evaluate each action by dividing the time during which it can provide normal results for valid, authorized requests by the total measurement time over one month. Correctly rejecting invalid or unauthorized requests does not count as unavailability.

The measurement environment, action mix for the 100 concurrent users, and basis for estimating the load will be finalized during design.

The metrics and evaluation timing for determining whether users have actually received the intended value are `undecided`. Meeting the functional and quality criteria above alone does not establish that user value has been validated.
