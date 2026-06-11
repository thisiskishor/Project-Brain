# [Project Name] — Architecture

> Single source of truth for all technical architecture decisions.
> All other files defer to this one on architecture questions.
> Update when — and only when — architecture changes. Never edit for cosmetic reasons.
> Cross-reference: DECISIONS.md for the WHY. AGENTS.md for security contracts.

---

## Table of Contents

1. [System Overview](#1-system-overview)
2. [Tech Stack](#2-tech-stack)
3. [Folder Structure](#3-folder-structure)
4. [Application Layers](#4-application-layers)
5. [Authentication Architecture](#5-authentication-architecture)
6. [Database Architecture](#6-database-architecture)
7. [API Architecture](#7-api-architecture)
8. [State Management](#8-state-management)
9. [Storage Architecture](#9-storage-architecture)
10. [Email Architecture](#10-email-architecture)
11. [Payment Architecture](#11-payment-architecture)
12. [Security Architecture](#12-security-architecture)
13. [Performance & Caching](#13-performance--caching)
14. [Deployment Architecture](#14-deployment-architecture)

*Remove sections that don't apply. Add sections that are missing for your stack.*

---

## 1. System Overview

[What this system is and how its core pieces fit together. 3–5 sentences max. Include a simple diagram if the topology isn't obvious.]

**Core user flows:**
- [Flow 1: e.g., "Guest discovers event → RSVPs → pays → attends"]
- [Flow 2]

**Platform constraint:** [e.g., "Strictly 18+. Age gate at signup and server-side at every RSVP."]

---

## 2. Tech Stack

### Frontend

| Tool | Version | Role | Why |
|------|---------|------|-----|
| [Framework] | [version] | [role] | [reason chosen] |
| [Styling] | [version] | [role] | [reason] |
| [State management] | [version] | [role] | [reason] |

### Backend / Infrastructure

| Tool | Version | Role | Why |
|------|---------|------|-----|
| [DB] | [version] | [role] | [reason] |
| [Auth] | [version] | [role] | [reason] |
| [Hosting] | — | [role] | [reason] |
| [Storage] | — | [role] | [reason] |
| [Email] | — | [role] | [reason] |

### Payment Providers

| Provider | Status | Notes |
|----------|--------|-------|
| [Provider] | Active / Feature-flagged off | [notes] |

---

## 3. Folder Structure

```
[project-root]/
├── [top-level folder/]    ← [description]
│   ├── [subfolder/]       ← [description]
│   └── [subfolder/]       ← [description]
└── [top-level folder/]    ← [description]
```

---

## 4. Application Layers

[Describe the layers of your system: e.g., client → API routes → service layer → DB. Show what lives at each layer and what crosses boundaries.]

```
[Client Layer]    →  [Server Layer]   →  [Data Layer]
[What lives here]    [What lives here]   [What lives here]
```

---

## 5. Authentication Architecture

[How auth works end-to-end. Session lifecycle, token handling, cookie management. What happens on signup vs. login vs. token refresh.]

**Key invariants:**
- [e.g., "Session cookies are httpOnly and secure — never accessible from JS"]
- [e.g., "Server-side session validation on every protected route"]

---

## 6. Database Architecture

[Overview of the data model. Key table groups and their relationships. RLS strategy. Migration approach.]

**RLS policy:** [e.g., "Every table has RLS enabled. Policies are defined in the migration file alongside table creation."]

**Key relationships:**
- [Table A] → [Table B]: [relationship type and cardinality]

---

## 7. API Architecture

[How API routes are structured. Auth middleware. Rate limiting. Error response format. Pagination approach.]

**Response format:**
```json
{
  "data": {},
  "error": null
}
```

**Auth levels:** [e.g., public / auth (signed-in) / owner / host / admin]

**Rate limiting:** [e.g., "Upstash Redis sliding window. Limits defined per-route in the rate limiter config."]

---

## 8. State Management

[Client state (UI) vs. server state (data fetching). What lives where and why.]

- **Server state:** [e.g., "TanStack Query — caching, deduplication, pagination"]
- **Client/UI state:** [e.g., "Zustand — modals, sidebars, ephemeral UI"]
- **Forms:** [e.g., "React Hook Form + Zod — uncontrolled inputs, schema validation"]

---

## 9. Storage Architecture

[File storage provider, bucket strategy, access control (public vs. private buckets), upload flow.]

**Upload flow:** [e.g., "Client requests presigned PUT URL → uploads directly → server records the key"]

**Buckets:**
| Bucket | Access | Contents |
|--------|--------|----------|
| [name] | public / private | [what goes here] |

---

## 10. Email Architecture

[Email provider, send queue mechanism, template approach, transactional vs. marketing distinction.]

**Transactional emails** (always sent, cannot unsubscribe):
- [e.g., OTP verification, payment confirmation]

**Platform emails** (respects unsubscribe):
- [e.g., event reminders, host newsletters]

---

## 11. Payment Architecture

[Payment flow end-to-end. Commission split logic (must be server-side). Webhook handling and idempotency. Feature flag state for each provider.]

**Commission split:** [e.g., "Calculated server-side only via calculateSplit(). Host payout is 80% of full ticket price — never reduced by discounts."]

**Webhook idempotency:** [e.g., "Check provider_payment_id before processing. If already processed, return 200 and skip."]

---

## 12. Security Architecture

[How security is layered across the system. RLS, server-side checks, secret management, input sanitization.]

**Defense layers:**
1. [e.g., "RLS on every DB table — first line"]
2. [e.g., "Server-side role checks on every protected route — second line"]
3. [e.g., "Input validation with Zod at API boundaries — third line"]
4. [e.g., "Output sanitization (DOMPurify) before storing rich text — fourth line"]

---

## 13. Performance & Caching

[Caching strategy: SSR vs. SSG vs. ISR. CDN, edge caching, DB query optimization.]

- **Static pages:** [e.g., "ISR with 60s revalidation for event detail pages"]
- **Dynamic pages:** [e.g., "SSR for user-specific pages (profile, dashboard)"]
- **Images:** [e.g., "Next.js Image with R2 remote pattern — CDN-cached at edge"]

---

## 14. Deployment Architecture

[How the system deploys. CI/CD pipeline. Environment variable strategy. Preview vs. production environments.]

**Environments:**
- Production: [URL + platform]
- Preview: [URL pattern + platform]

**Deploy trigger:** [e.g., "Push to main → Vercel auto-deploy"]

**Secrets:** [e.g., "All secrets in Vercel dashboard env vars. .env.local for local dev (gitignored). Never commit secrets."]
