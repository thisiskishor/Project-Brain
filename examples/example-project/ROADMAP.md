# Acme Marketplace — Roadmap

> Living document — editable and updated as features move between phases or are executed.
> Full locked decisions → DECISIONS.md (append-only)
> Session history → CHANGELOG.md (append-only)
> Current phase status → project-progress.md

---

## Post-Launch Priorities (Phase 2 — after launch metrics validate)

- [ ] **React Native mobile app** — iOS + Android; web MVP must hit traction first
- [ ] **Vendor ID verification** — identity check before listing; deferred from MVP (open question: required or optional?)
- [ ] **In-app messaging** — vendor ↔ buyer order questions; order email works for MVP
- [ ] **Phone OTP auth** — add if UK conversion data shows demand
- [ ] **Bulk vendor import** — CSV upload for vendors with existing product catalogs

---

## Deferred Integrations (Feature-Flagged Off at Launch)

| Feature | Flag | Why Deferred |
|---------|------|-------------|
| Vendor referral program | `REFERRAL_ENABLED=false` | Revenue model needs validation before discounting commission |
| Product promotions / boosting | `PROMOTIONS_ENABLED=false` | Adds UI complexity; not needed for MVP traction |

---

## Explicitly Out of Scope (MVP)

These are not deferred — they are explicitly excluded. Do not build unless user signs off.

- Secondary resale market — moderation burden, out of core scope
- Subscriptions or recurring billing — marketplace model is per-transaction
- Live streaming or virtual events — product marketplace, not content platform
- Crypto payments — not in UK buyer demand

---

## Technical Debt

Known issues to address post-launch:

- [ ] Generated Supabase types need a CI step to auto-regenerate on migration (currently manual)
- [ ] Image optimization pipeline not yet defined — add WebP conversion and resize-on-upload

---

## Open Ideas

- [ ] Vendor analytics dashboard (sales trends, top products, buyer geography) — raised 2026-05-03
- [ ] Bundle deals / multi-product discounts — raised by user as future consideration

---

## Completed

*(Nothing yet — project in Phase 2)*
