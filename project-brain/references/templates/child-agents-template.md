# [Folder Name] — Local Contracts

> Local contracts for this domain. Read before touching anything in this folder.
> Update only when local contracts change — never for cosmetic reasons.
> Root-level rules always take precedence. No child doc may override root AGENTS.md.
>
> NOTE: This file is for Pattern B (multi-service/multi-repo projects with distinct top-level domains).
> For Pattern A monorepos (Next.js, Rails, Django single apps), use the centralized Project Brain/ folder instead.

---

## Purpose

[What this folder owns and is responsible for. One clear sentence.]

---

## Ownership

[What this folder must NOT touch outside its own boundary. Be explicit about cross-boundary rules.]

---

## Local Contracts

Rules specific to this domain. These take precedence over general conventions but cannot override root-level rules.

- [Data shape or API contract: e.g., "All payment amounts stored in smallest currency unit (paise/cents)"]
- [Security invariant: e.g., "All functions in this folder run server-side only — no client exposure"]
- [Pattern: e.g., "Named exports only — no default exports in this domain"]

---

## Work Guidance

Standards for working in this folder:

- [Naming convention]
- [Testing requirement: e.g., "Unit tests required for all commission calculation functions"]
- [Error handling pattern]
- [Logging expectation]

---

## Verification

How to confirm this area is working correctly:

```bash
# [Command to run tests for this domain]
# [Command to check types]
# [Command to verify a specific invariant]
```

---

## Child DOX Index

*(Only if this folder has its own subdomains with separate AGENTS.md files)*

- `[subfolder/]` — [one-line description of what lives there]
