# Changelog

---

## [2.0.0] - 2026-06-11

### Added
- **Layer 4 — Domain Files**: expanded the 3-layer system to 4 layers with a dedicated domain file system
- **Core Set (always create)**: `DECISIONS.md` (append-only decision registry), `CHANGELOG.md` (append-only session history with MISTAKE/FIX pattern), `ROADMAP.md` (living backlog and scope guard)
- **Extended Set (create when complexity demands)**: `ARCHITECTURE.md`, `SCHEMA.md`, `API.md`, `DEPLOYMENT.md` — each with a dedicated template
- **File Load Map**: explicit L1/L2/L3/L4 load paths with specific file names per level; replaces the general "Lazy-Read Rule" with an actionable lookup table
- **Two Organization Patterns**: Pattern A (centralized `Project Brain/` folder for monorepos) vs. Pattern B (distributed child AGENTS.md for multi-service projects); documented when to use each
- **Domain File Update Rules**: explicit instructions for which files to update on CLOSE and how (DECISIONS append, CHANGELOG append, ROADMAP edit freely, ARCHITECTURE/SCHEMA/API/DEPLOYMENT update when domain changes)
- **Open Questions section** in `project-progress.md` template — blockers and unresolved decisions now have a formal home
- **`decisions-template.md`** — full template with format spec, section headers, and reversal/superseded patterns
- **`changelog-template.md`** — template with MISTAKE/FIX format and session entry structure
- **`roadmap-template.md`** — template with post-launch priorities, deferred integrations, explicit out-of-scope, technical debt, and open ideas sections
- **`architecture-template.md`** — 14-section template covering system overview, tech stack, folder structure, auth, DB, API, state, storage, email, payments, security, performance, and deployment
- **Updated example project**: added DECISIONS.md, CHANGELOG.md, ROADMAP.md populated examples; updated AGENTS.md and project-progress.md to match v2.0 format
- **Updated token-guide.md**: token estimates for all new files, updated load path budgets, new "File Size Budget Summary" table

### Changed
- `progress-file-template.md`: removed Locked Decisions section (canonical source is now DECISIONS.md); added Open Questions section; added pointers to DECISIONS.md, CHANGELOG.md, ROADMAP.md
- `root-agents-template.md`: added Domain Files Index section; replaced Locked Decisions table with summary list + pointer to DECISIONS.md; added Pattern A/B note on Child DOX Index
- `child-agents-template.md`: added note clarifying Pattern A vs. Pattern B applicability; minor structural cleanup
- `SKILL.md`: updated from 3-layer to 4-layer system; added Domain Files section; added File Load Map; updated INIT to scaffold core + extended sets; updated CLOSE to update domain files + session log; updated Templates table; updated token cost reference
- `README.md`: updated version badge to 2.0.0; added What Changed table; updated repository structure; updated "What's in SKILL.md" section; added Domain File System section; added File Load Map section
- `token-guide.md`: updated all estimates to reflect expanded file set; new L1 baseline includes AGENTS.md (not just progress file); added File Size Budget Summary

### Reasoning (why this version exists)
v1.x assumed a single AGENTS.md + progress file could hold everything worth knowing about a project. This is true for simple projects. For complex builds (20+ routes, 10+ tables, multi-project machines, payment splits, hard security invariants), a single AGENTS.md either violates its token budget or loses critical detail. The domain file system gives complex projects a place to store deep knowledge without bloating the files loaded on every session. The load map ensures that complexity is paid for only when the task actually requires it.

---

## [1.1.0] - 2026-06-10

### Added
- **Idempotency** as the 5th Invariable — ensures async/payment events can safely retry without duplicating data
- **Idempotency Red Line** — non-idempotent mutation on a retryable path (webhook, payment, queue) is a hard stop
- **Idempotency Requirements section** in Root AGENTS.md template — projects document webhook/payment retry rules upfront
- **Context Compaction Rule** — Session Log capped at 5 entries; older entries archived to `.project/history.md` to keep the active progress file lightweight
- **Lazy-Read Rule** — Session start reads root + target folder AGENTS.md only; mid-tier directories loaded only when task explicitly crosses their boundary
- **Deferred-Write Rule** — Session end updates only when structural contracts changed; local features and bugfixes within existing boundaries skip doc writes
- **Token efficiency pass** — 37% file size reduction through stripping conversational phrasing, merging redundant checklists, and compressing section headers
- **Full reference templates** — root AGENTS.md, child AGENTS.md, progress file, and history archive
- **Token guide** — explicit cost estimates and minimum viable load paths by task complexity
- **examples/** directory — fully populated example project showing a real-world AGENTS.md and progress file

### Changed
- Merged redundant Session Start / Pre-Code / Session End checklists into single compressed inline forms
- Removed "What Claude Does vs What You Provide" table (redundant with instructions)
- AGENTS.md Chain Protocol restructured around Lazy-Read + Deferred-Write strategy

---

## [1.0.0] - 2026-06-10

Initial release of Project-Brain.

### Core
- 3-Layer System: Reasoning Protocol (Layer 1) + Structural Contracts (Layer 2) + Living State (Layer 3)
- Entry Protocol with 4-level ambiguity classification (Trivial / Low / Medium / High)
- Topology Navigation — map data flows, call graphs, and invariants before every non-trivial edit
- The 4 Invariables — state ownership, observability, coupling/fragility, async/timing
- Verification Gate — 6-point pre-code checklist
- Red Lines — hard stops requiring explicit user acknowledgement
- AGENTS.md Chain Protocol — read before editing, update after meaningful changes
- Commit Decision Framework — Full Coherence / Pragmatic Partial / Hold + Clarify / User Override
- Execution Behavior — spawn and don't block, verify before claiming done, collaborate not execute
- Dialogue Discipline — constrained response length, answer-first discipline
- Layer 3 Progress File — cross-session state with current phase, locked decisions, active work, session log
- Root AGENTS.md template and child AGENTS.md shape
- Initialization guide for new projects
- Skill Boundaries — calibration guidance for when to apply full vs. lightweight protocol
