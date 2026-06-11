# Token Cost Guide

This guide gives explicit token estimates for each file and minimum viable load paths by task complexity. Use it to make deliberate decisions about what to load — not everything, not nothing.

---

## File Cost Estimates

| File | Typical Tokens | Growth Pattern | Load Rule |
|------|---------------|----------------|-----------|
| `SKILL.md` | ~2,200 | Stable | Load once per setup. Not every session. |
| `project-progress.md` | ~200–400 | Grows → compact at 5 entries | Always first. Keep under 400. |
| Root `AGENTS.md` | ~400–700 | Grows slowly | Always. Keep under 700. |
| `DECISIONS.md` | ~300–2,000+ | Grows as project matures | L4 and decision reviews only |
| `CHANGELOG.md` | ~400–3,000+ | Grows every session | Never load routinely — archive only |
| `ROADMAP.md` | ~200–600 | Grows then stabilizes | L4 and planning sessions only |
| `ARCHITECTURE.md` | ~800–3,000 | Stable after setup | L2+ — read section only unless doing structural work |
| `SCHEMA.md` | ~400–1,200 | Grows with tables | DB/migration work only |
| `API.md` | ~800–2,500 | Grows with routes | API work only |
| `DEPLOYMENT.md` | ~200–500 | Stable | Git/deploy work only |
| Child `AGENTS.md` (single) | ~150–300 | Stable | Immediate target folder only (Pattern B) |
| `.project/history.md` | ~500–5,000+ | Grows unbounded | Never load during normal work |

---

## Minimum Viable Load Paths

### L1 — Trivial (typo, rename, config, style)
```
project-progress.md    (~300 tokens)
AGENTS.md              (~500 tokens)
──────────────────────────────────────
Total: ~700–800 tokens
```

### L2 — Feature (new component, new endpoint, UI change)
```
project-progress.md        (~300 tokens)
AGENTS.md                  (~500 tokens)
ARCHITECTURE.md (section)  (~400 tokens)
──────────────────────────────────────────
Total: ~1,200–1,600 tokens
```

### L3 — Structural (DB migration, schema change, major refactor)
```
L2 files                   (~1,400 tokens)
SCHEMA.md                  (~600 tokens)    ← if DB work
ARCHITECTURE.md (full)     (~1,500 tokens)  ← if structural
──────────────────────────────────────────
Total: ~2,000–3,500 tokens
```

### L4 — Critical System (auth, payments, new phase, major architecture)
```
L3 files                   (~2,500 tokens)
ROADMAP.md                 (~400 tokens)
DECISIONS.md (section)     (~600 tokens)
──────────────────────────────────────────
Total: ~3,000–5,000 tokens
```

### API Work (building or auditing routes)
```
project-progress.md    (~300 tokens)
AGENTS.md              (~500 tokens)
API.md                 (~1,500 tokens)
──────────────────────────────────────
Total: ~2,000–3,500 tokens
```

### Deploy / Git Work
```
project-progress.md    (~300 tokens)
AGENTS.md              (~500 tokens)
DEPLOYMENT.md          (~350 tokens)
──────────────────────────────────────
Total: ~1,000–1,200 tokens
```

---

## Rules for Staying Lean

**1. Never load SKILL.md every session.**
It's a reference document — load it during INIT or re-orientation only. After that, the protocol lives in the agent's behavior.

**2. Keep `project-progress.md` under 400 tokens.**
The moment it grows past that, something is wrong: either the session log needs compaction, or locked decisions are being duplicated (they belong in DECISIONS.md, not here).

**3. Keep root `AGENTS.md` under 700 tokens.**
If it's growing, move domain-specific details into the appropriate domain file (ARCHITECTURE.md, SCHEMA.md, etc.).

**4. Never load `CHANGELOG.md` during normal work.**
It is a history archive. Load it only when explicitly debugging a past mistake or tracing how a specific decision was made. Its growth is unbounded — never make it a session-start file.

**5. Never load `.project/history.md` during normal work.**
It is the compacted overflow of the session log. Loading it is almost never necessary and always expensive.

**6. Read ARCHITECTURE.md in sections, not full-file.**
For an L2 feature task, read the relevant 2–3 sections. For L3/L4, read the full file. ARCHITECTURE.md at full load can be 1,500–3,000 tokens — read selectively.

**7. The Lazy-Read Rule saves 250–1,500 tokens per session.**
Skipping ARCHITECTURE.md on an L1 task, or skipping API.md on a non-API task, compounds across every session over a long project.

**8. DECISIONS.md grows — read sections, not the full file.**
For most sessions, you only need the section relevant to your task domain (Auth, Payments, Stack, etc.). Load the full file only on L4 tasks or explicit decision reviews.

---

## Context Compaction Trigger

Compact `project-progress.md` Session Log when it exceeds 5 entries.

**Before compaction** (7 entries, ~700 tokens in session log alone):
```
project-progress.md → ~900 tokens total
```

**After compaction** (5 entries kept, 2 archived to `.project/history.md`):
```
project-progress.md → ~600 tokens total
.project/history.md → archive, never loaded
```

Savings per compaction cycle: ~100–200 tokens on every future session start. Over a 50-session project, that's 5,000–10,000 tokens saved.

---

## File Size Budget Summary

| File | Target Budget | Action if Exceeded |
|------|--------------|-------------------|
| `project-progress.md` | ≤ 400 tokens | Compact session log; strip anything that belongs in DECISIONS.md |
| Root `AGENTS.md` | ≤ 700 tokens | Move domain details to appropriate domain file |
| `DECISIONS.md` section | ≤ 600 tokens per section | Split into sub-sections by domain |
| `ARCHITECTURE.md` section | ≤ 500 tokens per section | Split section; add sub-headers for selective loading |
| Session Log | ≤ 5 entries | Archive oldest entries to `.project/history.md` |
