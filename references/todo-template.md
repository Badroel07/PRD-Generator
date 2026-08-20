# Complete Guide: Appendix A — TODO List Template

> **Called from**: `SKILL.md`, Stage 2 (MANDATORY — must not be skipped).
> **Contents of this file**: The format for the `[Project-Name]-TODO.md` file with a **2-Phase** structure (Frontend-only mock data first, Backend after approval), priority guidance, granularity, and mandatory rules.
> **Read this file BEFORE generating the TODO list.**

---

## Appendix A: Implementation TODO List (MANDATORY)

Every PRD output MUST be accompanied by a separate **TODO List** as a ready-to-execute markdown file for the dev team. This appendix **must not be skipped** — the TODO List is the concrete set of action items that breaks the PRD down into checkable tasks.

### 2-Phase Structure (DEFAULT — mandatory for fullstack projects)

The TODO List is split into **2 major phases**, not regular sprint priorities:

1. **Phase 1 — Frontend-Only (Mock Data)**: Build the ENTIRE interface (all pages/screens per the User Flow in PRD Section 4) using dummy/mock data — **NO connection to a real backend/database** in this phase. Goal: give the user an end-to-end prototype as fast as possible to validate UX/flow BEFORE backend effort is spent.
2. **Checkpoint — User Review & Approval**: Demo Phase 1 to the user, collect feedback, revise if needed. **MUST stop here** — do not proceed to Phase 2 without explicit user approval.
3. **Phase 2 — Backend Implementation**: Build the backend per the Architecture (PRD Section 5) + Database Schema (PRD Section 6), then **replace the Phase 1 mock data with real API integration** — not rebuild the UI from scratch.

**When NOT to use the 2-phase structure** (use regular single-phase grouping by HIGH/MEDIUM/LOW priority instead):
- Projects **without UI** (API service, CLI tool, backend-only, data pipeline).
- Projects where the frontend consumes an **existing external API** (no custom backend being built by the user).
- The user explicitly requests a different structure.

If any of the above applies, document the reason in the TODO's "Important Notes" section.

### TODO List File Format

Save as a separate file: `[Project-Name]-TODO.md` in the same folder as the PRD. Use the following structure precisely:

```markdown
# TODO — [Project Name from PRD]

> **Source**: PRD v1.0 — `[prd-file-name].md`
> **Total**: [N] items ([N1] Phase 1 · [N2] Phase 2)
> **Estimate**: Phase 1 [X–Y days/weeks] → Checkpoint Review → Phase 2 [X–Y weeks]
> **Structure**: 2 Phases — Frontend prototype (mock data) first, Backend follows after user approval.

---

## 🎨 PHASE 1 — Frontend-Only (Mock Data / Fast Prototype)
> Target: [X–Y days/weeks]. All data is mock/dummy — NO real backend/database connection in this phase. Goal: user can try the main flow via the UI as fast as possible.

### 1a. Setup & Foundation
- [ ] **#1** Set up the frontend project ([framework]), routing, folder structure
- [ ] **#2** Set up design system tokens (typography, color, spacing — from PRD Section 7)

### 1b. Mock Data Layer
- [ ] **#3** Create mock data/fixtures for [main entity per PRD Section 6 Database Schema] — field shape MUST match the real schema exactly so it's easy to swap later
- [ ] **#4** Create mock service functions (e.g. `getProducts()`, `createOrder()`) that return mock data — function signatures must be IDENTICAL to the ones that will call the real API in Phase 2

### 1c. Pages & Components (per User Flow in PRD Section 4)
- [ ] **#5** [Page/screen 1 — descriptive]
- [ ] **#6** [Page/screen 2 — descriptive]
- [ ] **#N** ...

### 1d. Interactions & Client-side Logic
- [ ] **#N** Form validation (client-side), state management, loading/empty/error states (use simulated delay to feel real)

---

## 🚦 CHECKPOINT — User Review & Approval (MUST STOP HERE)
> **DO NOT proceed to Phase 2 before the user explicitly approves the Phase 1 prototype.**

- [ ] Demo the prototype to the user — all main flows in the User Flow (PRD Section 4) can be tried end-to-end with mock data
- [ ] Collect UI/UX feedback, revise if any
- [ ] User explicitly approves → only then this checklist is closed and Phase 2 starts

---

## ⚙️ PHASE 2 — Backend Implementation (After Phase 1 is Approved)
> Target: [X–Y weeks]. Build the backend per the Architecture (PRD Section 5) & Database Schema (PRD Section 6), then replace the Phase 1 mock data with real API integration — DO NOT rebuild the UI from scratch.

### 2a. Setup & Database
- [ ] **#N** Set up the backend project, database, migrations per the Database Schema in PRD Section 6

### 2b. API & Business Logic
- [ ] **#N** [Endpoint/business logic 1]
- [ ] **#N** [Endpoint/business logic 2]

### 2c. Frontend ↔ Backend Integration
- [ ] **#N** Replace mock service functions (#4) with real API calls — function signatures stay the same, only the internal implementation changes
- [ ] **#N** Real auth (if any), error handling from real responses, remove simulated delay

### 2d. Testing & Polish
- [ ] **#N** [Testing / edge cases / polish]

---

## 👥 Suggested Work Split (N People)

### Phase 1 (Frontend) — Split per Page/Screen
[Table distributing pages/screens across team members — everyone can work in parallel in Phase 1 since there are no backend dependencies yet]

### Phase 2 (Backend) — Split by Stream
[Table distributing Stream A (API/business logic) and Stream B (integration + testing) per team member]

---

## 🏃 Quick Wins (Show Progress Fast)
1. ⏱ [Most important page/flow — built first in Phase 1 so the demo is quick to show]
2. 🌱 [Early visual task]
3. 🧾 [Demo-friendly task]

---

## 📌 Important Notes
- [Critical notes about non-obvious decisions]
- [If the project does NOT use the 2-phase structure, explain the reason here]
- [Technical trade-offs to remember]
```

### Item Priority Guidance (used WITHIN each phase)

Priority now becomes sub-ordering inside Phase 1 / Phase 2, not the main grouping anymore (the main grouping = Phase). Mark inline if needed, e.g. `**#5** [HIGH] ...`.

| Priority | When to use | Example |
|---|---|---|
| **HIGH** | Required so the main flow can be tried/demoed | Phase 1: core pages (checkout, main dashboard). Phase 2: core API, core integration. |
| **MEDIUM** | Adds value but the prototype/product runs without it | Phase 1: secondary pages. Phase 2: supporting backend features (advanced reporting, etc.). |
| **LOW (Polish)** | Nice-to-have, optimization | Phase 1: micro-interactions. Phase 2: caching, rate limiting, audit log. |

### Granularity & Item Count Guidance

- **1 item = 1 task** completable in 0.5–2 days by 1 developer.
- **Ideal total**: 15–30 items.
- **Distribution**: Phase 1 usually has more items (~55–65% of total) because every page/component becomes a granular item. Phase 2 ~35–45%.
- **Grouping**: Items that depend on each other (e.g. "Set up Drizzle" + "Initial migration" + "Seed data") MUST be merged into 1 larger item, or numbered sequentially so the order is clear.

### Mandatory Rules for the TODO List

1. **MUST use markdown checkboxes** `- [ ]` (compatible with GitHub Projects, Notion, Obsidian, VS Code).
2. **MUST have a sequence number** `**#N**` on every item, continuing from Phase 1 into Phase 2 (do not reset numbers in Phase 2).
3. **MUST use the 2-phase structure** (Phase 1 Frontend mock data → Checkpoint → Phase 2 Backend) for fullstack projects — EXCEPT for the conditions listed in "When NOT to use the 2-phase structure" above.
4. **The Checkpoint MUST be its own explicit section** between Phase 1 and Phase 2, with clear instructions to "not proceed before approval".
5. **The mock data layer in Phase 1 MUST be "swap-able"**: service function signatures (e.g. `getProducts()`) must match the ones that will call the real API — so Phase 2 just swaps the implementation, not a total rewrite.
6. **MUST have an estimate** for each phase (not just the total).
7. **MUST include suggested work split** if the user mentions team size (default: assume 1–2 people).
8. **MUST include the Quick Wins section** — 2–3 of the earliest Phase 1 tasks with big visual/demo impact.
9. **MUST use emoji headers**: 🎨 (Phase 1), 🚦 (checkpoint), ⚙️ (Phase 2), 👥 (team), 🏃 (quick wins), 📌 (notes).
10. **Language**: Same as the PRD language (default: English). Code identifiers stay in their native form.
