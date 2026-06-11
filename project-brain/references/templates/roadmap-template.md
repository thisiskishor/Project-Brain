# [Project Name] — Roadmap

> Living document — editable and updated as features move between phases or are executed.
> Full locked decisions → DECISIONS.md (append-only)
> Session history → CHANGELOG.md (append-only)
> Current phase status → project-progress.md
>
> HOW TO USE:
> - Feature gets scheduled or built → move it from Deferred to Completed
> - New idea comes up but isn't decided → add to Open Ideas
> - Decision is locked → add to DECISIONS.md AND remove from Open Ideas here
> - Agent: update this file when feature scope changes across sessions

---

## Post-Launch Priorities

Confirmed to build after launch metrics validate direction:

- [ ] [Feature name] — [one-line description and why it's post-launch]
- [ ] [Feature name] — [one-line description]

---

## Deferred Integrations

Feature-flagged off at launch. Infrastructure is ready; activation is a config change.

| Feature | Flag | Why Deferred |
|---------|------|-------------|
| [Integration name] | `FEATURE_FLAG=false` | [reason — e.g., "complexity, adding post-launch"] |

---

## Explicitly Out of Scope (MVP)

These are not deferred — they are explicitly excluded. Do not build unless user signs off.

- [Feature] — [reason it was excluded]
- [Feature] — [reason]

---

## Technical Debt

Known issues to address post-launch:

- [ ] [Debt item] — [impact and suggested fix]

---

## Open Ideas

Not decided yet. Add here when a good idea comes up mid-session. Move to DECISIONS.md when locked.

- [ ] [Idea] — [brief context, raised YYYY-MM-DD]

---

## Completed (moved from Active)

- [YYYY-MM-DD]: [Feature name] — shipped in [phase/session]
