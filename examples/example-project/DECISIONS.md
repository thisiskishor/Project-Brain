# Acme Marketplace — Decision Registry

> THIS FILE IS APPEND-ONLY.
> No entry may be edited or deleted. Ever.
> If a decision is reversed or superseded, add a NEW entry with status REVERSED or SUPERSEDED.
> Never remove the original entry — the full history must be preserved.
> Future agents: read this file before proposing any architectural change.

---

## Product & Business Model

[2026-05-01] LOCKED | Commission split: 15% platform / 85% vendor | Server-side only via Stripe Connect — never trust client for financial calculations
[2026-05-01] LOCKED | No secondary resale market for products | Out of scope for MVP — moderation complexity without clear revenue
[2026-05-01] LOCKED | No subscriptions or recurring billing | Marketplace model; per-transaction commission is sufficient
[2026-05-01] LOCKED | Launch market: UK first, global architecture from day one | UK has strong e-commerce maturity; Stripe Connect UK support is mature

---

## Authentication

[2026-05-01] LOCKED | Auth: Supabase Auth — email/password + Google OAuth | Covers the majority of UK buyer/vendor preferences; phone OTP post-launch
[2026-05-01] LOCKED | No phone OTP at launch | Complexity vs. value tradeoff — add post-launch if UK conversion data supports it

---

## Stack & Architecture

[2026-05-01] LOCKED | Framework: Next.js 14 App Router | SSR required for product pages (SEO, Google Shopping indexing)
[2026-05-01] LOCKED | Not Vite | Vite has no SSR story for the discovery/SEO use case
[2026-05-01] LOCKED | Database: Supabase PostgreSQL with RLS on every table | Single platform for DB + Auth + Realtime + Edge Functions reduces infra complexity
[2026-05-01] LOCKED | Hosting: Vercel + Supabase | Next.js-native deploy + zero-config previews; Supabase collocates with DB

---

## Payments

[2026-05-01] LOCKED | Payment provider: Stripe Connect only | Stripe Connect supports automated split payouts to UK bank accounts; Razorpay/Khalti not needed for UK market
[2026-05-01] LOCKED | Commission calculation: server-side in Edge Function only | Client cannot be trusted for financial calculations — invariant, no exceptions

---

## Scope Guard

[2026-05-01] LOCKED | No in-app messaging (MVP) | Moderation burden without clear MVP value; vendor contact via order email is sufficient
[2026-05-01] LOCKED | No mobile app (MVP) | Web MVP first — build mobile post-launch if metrics validate
[2026-05-01] LOCKED | No live streaming or virtual events | Out of scope for a product marketplace
