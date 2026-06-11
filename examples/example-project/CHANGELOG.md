# Acme Marketplace — Engineering Changelog

> THIS FILE IS APPEND-ONLY.
> No entry may be edited or deleted. Ever.
> One entry per session, appended at the end.
> Include MISTAKE/FIX entries honestly — these are the most valuable part.
> Future agents: read this file only when explicitly debugging history — never load it routinely.

---

## 2026-05-01 — Phase 1 Complete: Feature List + Locked Decisions

**What happened:**
- Full feature list reviewed and signed off with user
- All major architectural decisions locked (stack, payments, auth, commission model)
- Phase 1 declared complete — no open items

**Deferred:**
- Vendor ID verification deferred to post-launch (open question in progress file)
- Image upload size limit not yet decided

---

## 2026-05-03 — Repo Scaffold + Project Brain Setup

**What happened:**
- Initialized Next.js 14 App Router project with TypeScript, Tailwind, shadcn/ui
- Confirmed Next.js 14 over Vite with user (SEO requirement for product discovery)
- Created all Project Brain files: AGENTS.md, project-progress.md, DECISIONS.md, CHANGELOG.md, ROADMAP.md
- Wrote initial folder structure proposal for Phase 2 architecture review

**MISTAKE: Proposed Supabase Storage for vendor documents / FIX: Supabase Storage was correct, but the private bucket configuration was missing from the initial proposal. Added explicit private bucket spec for vendor verification documents.**

**Deferred:**
- Full ARCHITECTURE.md write — starting Phase 2 next session
- SQL migration file — pending architecture sign-off
