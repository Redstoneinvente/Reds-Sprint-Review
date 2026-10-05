# Reds Sprint Review

A lightweight sprint planning and flow health dashboard for The Storefront.

## Improvements

- Project calendar for task due dates, review dates, and milestones.
- Flow health signals for work in progress, blocked tasks, aging work, overdue tasks, and recent throughput.
- Task dependencies, blocker/next-action notes, creation/start/completion timestamps, search, and status filters.
- Firebase Realtime Database REST sync, with a local browser copy and portable JSON backups.

## Firebase setup

The app uses the Realtime Database REST endpoint (JSON `.json` resources) at the configured database URL. It stores one project snapshot at `projects/storefront` by default, using `GET` and `PUT`; it does not use Firestore or the Firebase Admin SDK. You can change the database path in Settings.

For protected data, configure Firebase Authentication and database Security Rules that restrict access to authorized users. An optional Firebase ID token can be pasted in Settings and is kept in that browser's local storage. Do not put a service account credential or database secret into this static app. A Firebase API key is not an authorization token. With unauthenticated locked rules, REST requests will fail, and the app will continue to retain the local copy.

Connect flow: Settings → Firebase Realtime Database → enter URL/path and optional ID token → Connect and sync. First connection uploads local data when the remote path is empty. When remote data exists, the app asks whether to load it or replace it with this browser's copy.

## What the flow signals mean

- **Work in progress:** tasks marked In progress or Blocked. A growing queue can indicate context switching or work started faster than it finishes.
- **Work item age:** elapsed calendar days since work started (or became blocked). Seven days is a review prompt, not a universal service-level target.
- **Throughput:** completed tasks in the last 14 days, with review records providing a longer trend. Compare similar periods and similar task sizes.
- **Dependencies and blockers:** visible waiting relationships and the stated next action to unblock work.

These are system-level prompts. They are not individual performance measures or delivery promises. The Scrum Guide emphasizes inspecting progress toward the Sprint Goal and adapting the plan; Kanban flow guidance calls out WIP, throughput, work item age, and cycle time. This app currently tracks calendar-day age and does not estimate cycle-time percentiles because historical status transitions were not present in the original data.

## Research references

- [The Scrum Guide (official)](https://scrumguides.org/scrum-guide.html) — Sprint Goal focus, inspecting progress, and adapting the Sprint Backlog.
- [The Kanban Guide (May 2025)](https://kanbanguides.org/the-kanban-guide/) — flow measures including WIP, throughput, work item age, and cycle time.
- [Firebase Realtime Database REST API](https://firebase.google.com/docs/reference/rest/database/) — append `.json` to HTTPS database paths and use REST requests.
- [Firebase Realtime Database Security Rules](https://firebase.google.com/docs/database/security) — server-enforced authorization and validation; locked rules deny access by default.

## Current database access note

As observed on 2026-10-05, the requested endpoint returned `null` at `projects/storefront` and allowed an unauthenticated REST write. This is a property of the current database rules, not a requirement of the app. The app has no service account secret and does not automatically upload any sample records. The first explicit connect uploads the browser's current project if the remote location is empty. Review and tighten Firebase rules and enable authentication before storing anything sensitive or inviting other users.
