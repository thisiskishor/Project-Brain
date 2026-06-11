# Acme Marketplace — Progress

> Read this first on every session start. Update on every session close.
> Keep this file lean — it is the fast-load state file, not the history file.
> Full decision history: see DECISIONS.md (append-only)
> Full session history: see CHANGELOG.md (append-only)
> Deferred features and future scope: see ROADMAP.md

---

## Current Phase

Phase 2 — Platform Architecture — in-progress

---

## Completed

- 2026-05-01: Phase 1 complete — full feature list signed off, all locked decisions confirmed
- 2026-05-03: Initial repo scaffolded, environment variables defined, README written
- 2026-05-03: Project Brain files created (AGENTS.md, project-progress.md, DECISIONS.md, CHANGELOG.md, ROADMAP.md)

---

## Active

Writing the Phase 2 architecture document:
- Folder structure for frontend + backend + supabase
- API endpoint catalog (method, path, auth, table)
- SQL migration file with RLS policies
- Stripe Connect integration flow
- Email trigger map

---

## Next

- User reviews and signs off on Phase 2 architecture document
- Begin Phase 3: Design System (colors, typography, component specs)

---

## Open Questions

- [ ] Should vendor onboarding require ID verification before they can list? (affects auth flow complexity — raised 2026-05-03)
- [ ] Max image upload size for product listings — Supabase Storage default is 50MB, is that acceptable?

---

## Session Log

<!-- Keep to 5 most recent entries. Archive older entries to .project/history.md -->

- 2026-05-03: Scaffolded repo, confirmed Next.js 14 decision with user, wrote initial folder structure proposal, created all Project Brain files
- 2026-05-01: Phase 1 signed off. Locked: Stripe Connect, Supabase, 15/85 split, Next.js 14
