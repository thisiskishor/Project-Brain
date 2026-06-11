---
name: project-brain
description: "Unified AI engineering protocol for complex multi-phase platform builds — topology-first reasoning, hierarchical structural contracts (AGENTS.md), cross-session state persistence (project-progress.md), and a domain file system (DECISIONS, CHANGELOG, ROADMAP, ARCHITECTURE, and more). Use when: starting a multi-phase project from scratch; joining an existing codebase; working across multiple domains (frontend + backend + database + payments); any build where locked architectural decisions must survive multiple sessions."
license: MIT
metadata:
  author: Kishor Gartaula
  version: 2.0.0
  github: https://github.com/thisiskishor/Project-Brain
---

# Project-Brain v2.0 — Unified Engineering Protocol

> Topology-first reasoning · Structural contracts · Cross-session state · Domain file system
> Built for complex multi-phase platform builds. Works with Claude, Cursor, Codex, Windsurf, Copilot, Cline.

---

## When to Use This Skill

- Starting any non-trivial project from scratch
- Joining an existing codebase for the first time
- Beginning a new phase on a multi-phase build
- Any session where the agent must understand "where we are" before touching code
- Projects with locked architectural decisions that must survive multiple sessions
- Multi-domain builds (frontend + backend + database + payments + auth)

**Skip for**: single-file scripts, throwaway prototypes, or single-session tasks with no follow-on work.

---

## The 4-Layer System

```
Layer 1 — Reasoning Protocol   →  How to think before touching code     (SKILL.md)
Layer 2 — Structural Contracts →  Domain rules + locked decisions        (AGENTS.md hierarchy)
Layer 3 — Living State         →  Phase, what's done, what's next        (project-progress.md)
Layer 4 — Domain Files         →  Specialized truth sources per concern  (see below)
```

Layer 1 governs reasoning. Layer 2 holds contracts. Layer 3 holds current state. Layer 4 holds deep domain knowledge that would bloat Layers 2 and 3 if kept there.

---

## Layer 4 — Domain Files

Domain files capture knowledge too deep for AGENTS.md but too important to lose. They are **lazy-loaded** — only read when the current task actually requires them.

### Core Set (create at INIT for every project)

| File | Purpose | Mutability |
|------|---------|------------|
| `DECISIONS.md` | Append-only decision registry. All locked decisions, with date, reason, and reversal tracking. | Append-only — never edit or delete entries |
| `CHANGELOG.md` | Append-only session history. One entry per session. Includes `MISTAKE: X / FIX: Y` pattern for honest error tracking. | Append-only — never edit or delete entries |
| `ROADMAP.md` | Living backlog. Deferred features, future scope, open ideas. Things decided NOT to build yet. | Living — editable |

### Extended Set (create when complexity demands)

| File | Create When | Purpose |
|------|-------------|---------|
| `ARCHITECTURE.md` | Stack has > trivial complexity (multiple services, non-obvious patterns) | Single source of truth for all technical architecture decisions. Other files defer to this one. |
| `SCHEMA.md` | DB has > ~10 tables | Human-readable ER map, table relationships, key invariants. Full DDL stays in migrations. |
| `API.md` | Routes > ~20 | Living catalog of all API routes with method, auth, status (`[ ]` planned / `[x]` built / `[✓]` tested). |
| `DEPLOYMENT.md` | Multi-project machine or non-trivial deploy config | Git identity rules, deployment pipeline, credentials guide. Critical for multi-project isolation. |

### File Organization

Two valid patterns — choose based on project structure:

**Pattern A — Centralized (monorepos, Next.js, single-codebase projects):**
```
project-root/
├── Project Brain/          ← all domain files here
│   ├── AGENTS.md
│   ├── project-progress.md
│   ├── DECISIONS.md
│   ├── CHANGELOG.md
│   ├── ROADMAP.md
│   └── [extended files as needed]
```

**Pattern B — Distributed (multi-repo, separated frontend/backend/services):**
```
project-root/
├── AGENTS.md               ← root contracts
├── project-progress.md     ← living state
├── DECISIONS.md            ← decision registry
├── CHANGELOG.md            ← session history
├── ROADMAP.md              ← backlog
├── frontend/
│   └── AGENTS.md           ← frontend-specific contracts
├── backend/
│   └── AGENTS.md           ← backend-specific contracts
└── payments/
    └── AGENTS.md           ← payments-specific contracts
```

Use Pattern A when your codebase is a single Next.js/Rails/Django app. Use Pattern B when you have distinct top-level service directories that teams work in independently.

---

## File Load Map

**The most important operational rule in this skill.** Do not load every file on every session. Load by task.

```
ALWAYS (every session, no exceptions):
  project-progress.md   + AGENTS.md

THEN stop at the right level:

  L1 — Trivial (typo, rename, config tweak)
       Nothing else. Proceed directly.

  L2 — Feature (new component, new endpoint, UI change)
       + ARCHITECTURE.md (relevant section only, not full file)

  L3 — Structural (DB migration, schema change, major refactor)
       + ARCHITECTURE.md (full) + SCHEMA.md

  L4 — Critical System (auth, payments, new phase, major architecture)
       + ARCHITECTURE.md + ROADMAP.md + DECISIONS.md (relevant section)

  API work (building or auditing routes):
       + API.md

  Deploy / git work:
       + DEPLOYMENT.md

  Decision review (user asks about past choices):
       + DECISIONS.md

  History debugging (tracing a past mistake):
       + CHANGELOG.md  ← NEVER load routinely; expensive and rarely needed
```

**Rules:**
- Never load `.project/history.md` during normal work — it is an archive
- Never load CHANGELOG.md unless explicitly debugging history
- A task with "multiple angles" is not permission to load everything — classify the primary concern and load for that level
- If uncertain which level: pick one lower, not one higher

---

## How to Invoke

| Trigger | Action |
|---------|--------|
| `/project-brain init` | Scaffold all files for a new project |
| `/project-brain start` | Begin session: read state, classify task, map topology |
| `/project-brain close` | End session: update files, compact log |
| `/project-brain status` | Read-only: summarize current phase and open questions |
| `"Initialize project-brain"` | Natural language — same as `/project-brain init` |
| `"Apply project-brain"` | Natural language — same as `/project-brain start` |

---

## INIT — Scaffold a New Project

When `/project-brain init` is triggered:

1. Detect project stack (check package.json, go.mod, requirements.txt, pyproject.toml, etc.)
2. Determine file organization pattern (A = monorepo/single-codebase, B = multi-service)
3. Create **always-required files:**
   - `AGENTS.md` (root) — from `references/templates/root-agents-template.md`
   - `project-progress.md` — from `references/templates/progress-file-template.md`
   - `DECISIONS.md` — from `references/templates/decisions-template.md`
   - `CHANGELOG.md` — from `references/templates/changelog-template.md`
   - `ROADMAP.md` — from `references/templates/roadmap-template.md`
4. Assess complexity and create **extended files as warranted:**
   - `ARCHITECTURE.md` — if stack has multiple services, non-obvious patterns, or > 3 layers
   - `SCHEMA.md` — if DB will have > ~10 tables
   - `API.md` — if project will have > ~20 API routes
   - `DEPLOYMENT.md` — if running on a multi-project machine or complex deploy pipeline
5. If Pattern B: identify top-level domain folders, create child `AGENTS.md` stubs
6. Report what was scaffolded and confirm with user before proceeding

---

## START — Session Begin

When `/project-brain start` is triggered:

1. Read `project-progress.md` — **before anything else**
2. Read root `AGENTS.md`
3. Classify the task (see Ambiguity Classification below)
4. Apply the Load Map — load only what the task level requires
5. Describe the relevant topology before writing any code

---

## Ambiguity Classification

| Level | Trigger | Action |
|-------|---------|--------|
| **Trivial** | Typo, rename, single obvious fix | Proceed directly |
| **Low** | Clear, specific, narrow scope | Quick verify, proceed |
| **Medium** | Multi-file, gaps in stated intent | Ask targeted questions on gaps only |
| **High** | Vague, architectural, cross-cutting | Full question sequence — no code until resolved |

Confirm detected tensions back to the user. Only proceed when confidence is high and no unresolved tensions exist.

---

## Topology Navigation (Before Every Non-Trivial Edit)

1. Identify entry points, core modules, high-centrality components
2. Map data flows, call graphs, architectural layers
3. Discover key abstractions, contracts, interfaces, invariants
4. Note tech stack, patterns, conventions in scope
5. **Describe the relevant topology to the user before writing code**

**Stay in lane.** Flag out-of-scope dependencies and stop. Ask before crossing the boundary.

---

## The 5 Invariables

Apply to every non-trivial change:

| Question | Maps To | Why |
|---|---|---|
| Where does state live? | Ownership & source of truth | Consistency, blast radius |
| Where does feedback live? | Observability | Debugging, monitoring |
| What breaks if I delete this? | Coupling & fragility | Safe refactoring |
| When does timing work? | Async & ordering | Race conditions, correctness |
| Is this operation idempotent? | Retry safety | Webhooks, payments, queued jobs must not duplicate data on retry |

---

## Verification Gate (Before Writing Code)

Non-trivial work must pass all:

- [ ] State ownership clear?
- [ ] Blast radius understood?
- [ ] Timing & ordering safe?
- [ ] Idempotent where retries are possible (webhooks, payments, queued jobs)?
- [ ] Observability in place?
- [ ] Follows existing patterns (or intentionally deviates with a stated reason)?
- [ ] Security risks addressed?

Any unclear → flag, ask, or defer. Never write code to resolve an unclear invariant.

---

## Red Lines (Stop and Flag — No Exceptions)

Never proceed past any of these without explicit user acknowledgement:

- Unclear state ownership
- Unknown blast radius on a non-trivial change
- Race condition or timing hazard
- Non-idempotent mutation on a retryable path (webhook, payment, queue)
- Security vulnerability (injection, auth bypass, secrets in client, missing RLS)
- Significant complexity debt
- Unknown unknowns on non-trivial changes

---

## AGENTS.md Chain Protocol

### Read (Lazy — Session Start or Before Edit)

1. Always read: root `AGENTS.md` + `project-progress.md`
2. For Pattern B: read immediate target folder's `AGENTS.md` only
3. Pull mid-tier `AGENTS.md` files **only if** the task crosses that folder's boundary
4. Closer doc controls local details; no child doc may override root-level rules

### Write (Deferred — Session End)

Update AGENTS.md files **only when structural contracts changed**.
Skip entirely if the session only added local features or fixed bugs within existing boundaries.

**Triggers that require a write:**
- Purpose, scope, ownership, or responsibilities changed
- A new contract, invariant, API shape, or security rule was introduced
- A folder was created, deleted, moved, or renamed
- Phase-gate status changed

Remove stale or contradictory text on write. No history entries in AGENTS.md files — history belongs in CHANGELOG.md.

---

## Domain File Update Rules

### DECISIONS.md
Append a new entry when a decision is locked during a session. Format:
```
[YYYY-MM-DD] LOCKED | [Decision] | [Reason]
[YYYY-MM-DD] REVERSED | [Decision] | [Why reversed] | Supersedes: [original date]
```
Never edit or delete existing entries. Reversals are new entries, not overwrites.

### CHANGELOG.md
Append one entry per session at close. Always include:
- What was built or changed
- Any `MISTAKE: [what went wrong] / FIX: [what fixed it]` entries — honest error tracking is the point
- What was deferred

Never edit or delete existing entries.

### ROADMAP.md
Update when features move between phases or are explicitly deferred or completed. This is a living document — edit freely.

### ARCHITECTURE.md / SCHEMA.md / API.md / DEPLOYMENT.md
Update when the domain they cover changes. These are living documents — edit freely, but keep them accurate. Never leave stale data.

---

## Commit Decision

| State | Action |
|-------|--------|
| Full Coherence | Ship complete solution |
| Pragmatic Partial | Ship core + flag deferred items explicitly |
| Hold + Clarify | Gaps remain — ask before proceeding |
| User Override | "Ship it" = proceed with risks flagged |

---

## Execution Behavior

**Spawn and Don't Block** — Fork subagents for parallel work. Integrate results when ready.

**Verify Before Claiming Done** — Compile/run? Tests pass? Edge cases covered?
- PASS = confident it works
- PARTIAL = mostly works, known gaps flagged
- FAIL = rework needed

**Collaborate, Don't Execute** — Flag misconceptions and adjacent bugs proactively. Disagree honestly. State assumptions.

**Security** — Race conditions, duplicated logic, insecure data flows, OWASP Top 10, missing RLS, secrets in client.

---

## Dialogue Discipline

- Responses ≤100 words unless task demands more; text between tool calls ≤25 words
- State assumptions explicitly; come back with answers, not just questions
- Never write code you cannot trace invariants for

---

## CLOSE — Session End

When `/project-brain close` is triggered:

1. **Update `project-progress.md`:**
   - Move completed items with date
   - Rewrite Active to reflect current state
   - Update Next
   - Resolve or carry forward Open Questions
   - Add Session Log entry: `[YYYY-MM-DD]: [what happened, decisions made, what changed]`

2. **Compact session log if needed:**
   - If Session Log has > 5 entries → archive all but the 5 most recent to `.project/history.md` (append-only, never overwrite)
   - Never load `.project/history.md` during normal sessions

3. **Update domain files:**
   - DECISIONS.md: append any decisions locked this session
   - CHANGELOG.md: append this session's entry (include MISTAKE/FIX if applicable)
   - ROADMAP.md: update if features moved phases or were completed
   - ARCHITECTURE.md / SCHEMA.md / API.md / DEPLOYMENT.md: update if their domain changed

4. **Update AGENTS.md if structural contracts changed** (see AGENTS.md Chain Protocol above)

5. Report what was updated.

---

## STATUS — Read-Only Check

When `/project-brain status` is triggered:

Read `project-progress.md` only. Output a 6-line summary:
```
Phase:     [current phase + status]
Active:    [what's being built]
Next:      [what follows]
Blocked:   [open questions or blockers, or "none"]
Sessions:  [number of log entries in active file]
Last:      [date of most recent session log entry]
```

---

## Session Complexity Levels

| Level | Type | Load Map | Protocol |
|-------|------|----------|---------|
| **L1** | Typo, config, style tweak | progress + AGENTS only | Proceed directly, no docs update |
| **L2** | New feature, component, endpoint | + ARCHITECTURE (section) | Topology check, verification gate, close with update |
| **L3** | Schema change, major refactor | + ARCHITECTURE (full) + SCHEMA | Full topology + verification gate + AGENTS.md update if structural |
| **L4** | Auth, payments, new phase, major arch | + ROADMAP + DECISIONS | Full + Red Lines + phase-gate check + user confirmation |

---

## Skill Boundaries

**Built for:**
- Multi-phase builds (3+ phases, multi-week or multi-month)
- Multi-domain projects (frontend + backend + database + payments + auth)
- Projects with critical invariants (payment splits, RLS policies, commission logic)
- Projects where wrong architectural decisions are expensive to reverse
- Any build where AI agents must maintain continuity across sessions

**Overkill for:** Single-file scripts, prototypes, one-session tasks.

**Calibrate:** L1 tasks skip everything. L4 tasks use the full protocol. The domain files exist to make complex sessions cheaper — not to make simple sessions heavier. Only load what the task actually requires.

---

## Templates (Load When Needed)

All templates are in `references/templates/`. Load only what the current task requires:

| Template | When to use |
|----------|-------------|
| `root-agents-template.md` | INIT: creating root AGENTS.md |
| `child-agents-template.md` | INIT: creating child AGENTS.md for domain folder (Pattern B) |
| `progress-file-template.md` | INIT: creating project-progress.md |
| `decisions-template.md` | INIT: creating DECISIONS.md |
| `changelog-template.md` | INIT: creating CHANGELOG.md |
| `roadmap-template.md` | INIT: creating ROADMAP.md |
| `architecture-template.md` | INIT: creating ARCHITECTURE.md (extended set) |
| `history-archive-template.md` | CLOSE: compacting session log to .project/history.md |

---

## Token Cost Reference

See `references/token-guide.md` for full estimates. Quick reference:

| Task | Files loaded | Est. tokens |
|------|-------------|-------------|
| L1 quick fix | progress + AGENTS | ~700 |
| L2 feature | + ARCHITECTURE section | ~1,200–1,600 |
| L3 schema/refactor | + ARCHITECTURE full + SCHEMA | ~2,000–3,500 |
| L4 major system | + ROADMAP + DECISIONS section | ~3,000–5,000 |
| API route work | + API.md | ~2,000–4,000 |
| Deploy/git work | + DEPLOYMENT.md | ~900–1,200 |

---

## Related Skills

- [code-brain](https://github.com/thisiskishor/code-brain) — persistent session memory for Claude; pairs well with project-brain
