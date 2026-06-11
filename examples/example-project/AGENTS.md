# Acme Marketplace — Root AGENTS.md

> Root contracts for the entire repository. Read before touching anything. Update only when durable structure changes.
> Full decision history → DECISIONS.md (append-only)
> Full session history → CHANGELOG.md (append-only)
> Deferred features → ROADMAP.md

---

## Project Overview

Acme is a two-sided marketplace where vendors list products and buyers purchase them. Vendors keep 85% of each sale; the platform takes 15% via Stripe Connect server-side splits. Launch market is the UK, with global support from day one.

---

## Locked Decisions (Summary)

Non-negotiable. Do not re-debate. Full history in DECISIONS.md.

- Commission split: 15% platform / 85% vendor — server-side only, never trust client
- Payment provider: Stripe Connect only — UK bank support + automated split payout API
- Database: Supabase PostgreSQL, RLS on every table — no exceptions
- Auth: Supabase Auth — Email/password + Google OAuth
- Frontend: Next.js 14 + Tailwind + Zustand — locked
- Hosting: Vercel (frontend) + Supabase (everything else) — locked

---

## Phase-Gate Rules

Do not start Phase N+1 until Phase N is signed off by the user.

| Phase | Name | Status |
|-------|------|--------|
| 1 | Feature Finalization | complete — 2026-05-01 |
| 2 | Platform Architecture | in-progress |
| 3 | Design System | not started |
| 4 | Backend + Database | not started |
| 5 | Frontend Build | not started |
| 6 | Launch Prep | not started |

---

## Tech Stack

```
Framework:   Next.js 14 (App Router, TypeScript)
Styling:     Tailwind CSS + shadcn/ui
State:       TanStack Query v5 (server) + Zustand (client/UI)
DB:          Supabase PostgreSQL (RLS on everything)
Auth:        Supabase Auth (email/password + Google OAuth)
Storage:     Supabase Storage (product images, vendor docs)
Email:       Resend via Supabase Edge Functions
Payments:    Stripe Connect (server-side splits, automated payouts)
Hosting:     Vercel (frontend) + Supabase (DB, auth, edge functions)
```

---

## Security Invariants

Always enforce. No exceptions. Flag immediately if any of these would be violated.

- RLS on every database table — zero exceptions
- Commission split (15%/85%) calculated server-side only — never expose calculation to client
- `STRIPE_SECRET_KEY` is server-side only — never in `NEXT_PUBLIC_` env vars or client bundles
- Validate Stripe webhook signatures on every handler before processing any event
- All vendor payouts go through Stripe Connect — no manual transfers
- Admin routes: server-side role check on every request — never trust client claims

---

## Idempotency Requirements

- Stripe webhook handlers check for an existing processed payment before acting
- All order creation endpoints use Stripe idempotency keys
- Payout triggers deduplicated by period — one payout per vendor per billing period

---

## Scope Guard — What NOT to Build

Flag immediately if any of these are proposed:

- No secondary resale market for products
- No subscriptions or recurring billing
- No in-app messaging between buyers and vendors (MVP)
- No mobile app (MVP — post-launch Phase 2)
- No live streaming or virtual events

---

## Folder Structure

```
acme-marketplace/
├── Project Brain/         ← all planning and architecture documents
│   ├── AGENTS.md          ← this file
│   ├── project-progress.md
│   ├── DECISIONS.md
│   ├── CHANGELOG.md
│   ├── ROADMAP.md
│   └── ARCHITECTURE.md
├── src/
│   ├── app/               ← Next.js App Router pages + API routes
│   ├── components/        ← shared UI components
│   ├── lib/               ← utilities, DB client, payment helpers
│   └── types/             ← TypeScript types including generated DB types
├── supabase/
│   └── migrations/        ← SQL migrations with RLS policies
└── public/                ← static assets
```

---

## Domain Files Index

| File | Load When |
|------|-----------|
| `DECISIONS.md` | Reviewing past decisions, L4 tasks, or when user asks about a past choice |
| `CHANGELOG.md` | Debugging a past mistake — never load routinely |
| `ROADMAP.md` | Planning sessions, phase transitions, asking what's deferred |
| `ARCHITECTURE.md` | L2+ tasks — read relevant section; full file for L3+ structural work |
