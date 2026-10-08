# Emergent Repository Intelligence (stuff.md)

> Cross-commit patterns, architectural hotspots, and Gemini-powered insights.

## Deterministic patterns

**Most-touched files (Architectural hotspots):**
- `server.ts` — 3 commits
- `app.py` — 3 commits
- `config.ts` — 1 commits
- `docs/AUTH.md` — 1 commits

**Files with iterative wrong->correct cycles (Hard-won lessons):**
- `server.ts` — 1 failed-and-fixed cycles
- `app.py` — 1 failed-and-fixed cycles

**Recurring themes in commit subjects (sampled across 8 total commits, deduplicated per commit):**
- `middleware` — 3 commits (38% of total corpus)
- `auth` — 2 commits (25% of total corpus)
- `route` — 2 commits (25% of total corpus)
- `feat` — 2 commits (25% of total corpus)
- `config` — 1 commits (13% of total corpus)
- `loader` — 1 commits (13% of total corpus)
- `document` — 1 commits (13% of total corpus)
- `flow` — 1 commits (13% of total corpus)

### 🎯 Semantic Retrieval & Embedding Priority Index (Firestore Vector Targets)
> High-churn / high-recovery files prioritized for vector embedding into DARLEK semantic retrieval storage. These files yield the highest ROI for 'have I broken this before' similarity queries.

1. `server.ts` — **Priority P0 (CRITICAL - Highest Fail/Fix Volume)** (1 fail-and-fixed recovery cycles)
2. `app.py` — **Priority P1 (HIGH - Frequent Recovery Cycles)** (1 fail-and-fixed recovery cycles)

--- 

## LLM-surfaced patterns (Gemini)

> *Sample Archaeological Corpus Intelligence (Pre-analyzed)*

- **Architectural Hotspots Identified.** High commit churn observed in `app.py` (authentication validation logic) and `server.ts` (middleware registration sequence).
- **Failure Modes & Recovery.** Inline token parsing was refactored into modular `require_auth` decorator middleware. Logger middleware order was fixed to prevent blocking static file serving.
- **Code Evolution.** Rapid stabilization observed across structural scaffolding, auth guards, and documentation deliverables.