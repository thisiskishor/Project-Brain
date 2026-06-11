# [Project Name] — Root AGENTS.md

> Root contracts for the entire repository. Read before touching anything. Update only when durable structure changes.
> Full decision history → DECISIONS.md (append-only, do not re-debate without explicit user sign-off)
> Full session history → CHANGELOG.md (append-only)
> Deferred features → ROADMAP.md

---

## Project Overview

[What this project is, its primary purpose, and the core business model in 2–3 sentences.]

---

## Locked Decisions (Summary)

Non-negotiable. Do not re-debate. Full history in DECISIONS.md.

- [Key decision]: [value] — [one-line reason]
- [Key decision]: [value] — [one-line reason]

---

## Phase-Gate Rules

Do not start Phase N+1 until Phase N is signed off by the user. Ask before crossing phase boundaries.

| Phase | Name | Status |
|-------|------|--------|
| 1 | [Name] | not started / in-progress / complete — [date if complete] |

---

## Tech Stack

```
Framework:   [name + version]
Language:    [name + version]
Styling:     [library]
State:       [client state] + [server state]
DB:          [provider + ORM if any]
Auth:        [provider + method]
Storage:     [provider]
Email:       [provider]
Payments:    [provider + status: active/feature-flagged]
Hosting:     [frontend host] + [backend/DB host]
```

---

## Security Invariants

Always enforce. No exceptions. Flag immediately if any of these would be violated.

- [e.g., Row-Level Security on every database table — no exceptions]
- [e.g., Commission calculations are server-side only — never trust client]
- [e.g., Payment provider secrets are server-side only — never in NEXT_PUBLIC_ or client bundles]
- [e.g., Webhook signatures validated before processing any event]

---

## Idempotency Requirements

- [e.g., Webhook handlers check for existing processed record before acting]
- [e.g., Payment order creation deduplicated by idempotency key]
- [e.g., Queue consumers must be idempotent — no side effects on duplicate delivery]

---

## Scope Guard — What NOT to Build

Flag immediately if any of these are proposed:

- [Feature that is explicitly out of scope — e.g., "No mobile app in MVP"]
- [Another out-of-scope feature]

---

## Folder Structure

```
[project-root]/
├── [folder/]    ← [one-line description]
├── [folder/]    ← [one-line description]
└── [folder/]    ← [one-line description]
```

---

## Domain Files Index

Files that exist for this project and when to load them:

| File | Load When |
|------|-----------|
| `DECISIONS.md` | Reviewing past decisions, L4 tasks, or when user asks about a past choice |
| `CHANGELOG.md` | Debugging a past mistake or tracing a historical change — never routinely |
| `ROADMAP.md` | Planning sessions, phase transitions, or when asked what's deferred |
| `ARCHITECTURE.md` | L2+ tasks — read relevant section only unless doing structural work |
| `SCHEMA.md` | DB migrations, schema questions, any table-level work |
| `API.md` | Building, auditing, or documenting API routes |
| `DEPLOYMENT.md` | Git operations, deploys, environment variable changes |

*Remove rows for files that don't exist in this project.*

---

## Child DOX Index

*(Only for Pattern B — multi-service projects with per-folder AGENTS.md files. Delete this section for Pattern A monorepos.)*

- `[folder/]` — [one-line description of what this domain owns]
