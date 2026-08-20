# Complete Guide: Appendix B — Implementation Prompt Template

> **Called from**: `SKILL.md`, Stage 2.
> **Contents of this file**: The format for the `[Project-Name]-IMPLEMENTATION-PROMPT.md` file ready to paste into a coding agent (Claude Code, Cursor, etc.), structured for **2 work phases** (Phase 1: Frontend-Only Mock Data, Phase 2: Backend Implementation). Default execution is **Full Autopilot** (auto-continue without asking permission between tasks) with one explicit approval gate between Phase 1 and Phase 2.
> **Skip this appendix ONLY** when the project is non-technical (SOP, content strategy) or the user explicitly asks for no prompt — see "When You Do NOT Need an Implementation Prompt" below.

---

Every PRD output MUST be accompanied by a separate **Implementation Prompt** as a ready-to-paste markdown file for a coding agent (Mavis, Claude Code, Cursor, Cody, or a human engineer) to execute the PRD + TODO. This appendix **must not be skipped** — the Implementation Prompt is the "execution blueprint" that turns the PRD + TODO into a ready-to-use command.

### Why This File Matters

- **Self-contained**: The executing agent has no prior context, so the prompt must include all important info (tech stack, working principles, task order, active phase).
- **Reusable**: The same prompt is used for both Phase 1 and Phase 2 — just swap the `CURRENT_PHASE` block.
- **Default full-autopilot**: The agent keeps going task after task **without stopping to ask permission** in between — the only mandatory stop is the GATE at the end of Phase 1 (and real technical blockers). This MUST be stated explicitly in the prompt, because many coding agents by default tend to pause and wait for confirmation after each task unless instructed otherwise.
- **Enforces the approval gate**: The Phase 1 prompt explicitly instructs the agent to **stop** after ALL Phase 1 tasks are done and wait for user review — not auto-proceed to the backend. This is the only intentional pause; outside of it the agent must not stop.

### Core Principle: Aligned with the 2-Phase TODO Structure

This prompt MUST align with the 2-phase structure in `references/todo-template.md`:

- **Phase 1 — Frontend-Only (Mock Data)**: The agent ONLY builds the interface + mock data layer. **FORBIDDEN** to set up a real database/backend/API in this phase.
- **Mandatory gate at the end of Phase 1**: After all Phase 1 tasks are done, the agent **MUST STOP**, report that the prototype is ready for review, and **WAIT** for an explicit approval sentence from the user before touching any Phase 2 task — even if the user said "proceed" generically elsewhere, the agent must first confirm this is approval for Phase 1.
- **Phase 2 — Backend Implementation**: The agent builds the backend per the Architecture + Database Schema, then **replaces the contents of the Phase 1 mock service functions** with real API calls — **FORBIDDEN** to rewrite the UI from scratch unless there is a new requirement.

> If the project does NOT use the 2-phase structure (see the exception in `todo-template.md`), this prompt can still be used but with a single `CURRENT_PHASE` (e.g. "Single Phase — Fullstack") — remove the approval gate section.

### Implementation Prompt File Format

Save as a separate file: `[Project-Name]-IMPLEMENTATION-PROMPT.md` in the same folder as the PRD and TODO. Use the following structure precisely:

```markdown
# IMPLEMENTATION PROMPT — [Project Name]

> **How to use**: Copy the entire content from `--- PROMPT START` to `--- PROMPT END`, paste into a new chat / coding agent as the first message.
> **Required reference files** (place in the working folder):
> - `[PRD-filename].md`
> - `[TODO-filename].md`
>
> **Replace the `CURRENT_PHASE` block** below to match the active phase — start with Phase 1.

---

## PROMPT START — COPY FROM HERE

\`\`\`
You are a **Senior Fullstack Engineer** tasked with building **[Project Name]** in 2 phases: a Frontend prototype first (mock data), then the Backend after user approval.

## 📂 Project Context

Read and understand the reference documents before starting to code:
1. `[PRD-filename].md` — Product Requirements Document
2. `[TODO-filename].md` — Task list per phase (total [N] items: [N1] Phase 1, [N2] Phase 2)

Do not skip reading — all technical decisions must align with the PRD.

## 🎯 Mission

Implement **incrementally per task**, starting from Task #1. After each task is done:
1. Commit with a conventional commit message (feat:, fix:, chore:, etc.).
2. Update the status in `[TODO-filename].md` (change `- [ ]` to `- [x]`).
3. Report progress briefly: what's done, blockers, next task — this report is FYI, **NOT** a request for permission. Do not stop and wait for a user reply, just continue to the next task.

**DO NOT skip around** between tasks unless the previous task is already committed + verified working.

### 🚀 Execution Mode: Full Autopilot (DEFAULT — MUST BE FOLLOWED)

- Work on task #1, #2, #3, etc. **back-to-back without pause** — no need to wait for "ok continue" from the user between tasks.
- Reports after each task are informational, not questions. If there is no real blocker, immediately move on to the next task in the same response or the next response without waiting for a reply.
- **The only mandatory stop in all of Phase 1** is the **GATE at the end of Phase 1** (see below) — not between any earlier tasks.
- If there is a real technical blocker (not just "I want to check with the user first"), then stop and explain the blocker.
- This mode can be overridden if the user explicitly requests per-task confirmation (see Variation B — Pair Programming Mode) — but that is not the default, it must be explicitly requested.

## 🛠 Tech Stack (LOCKED — do not change)

| Layer | Technology |
| :--- | :--- |
| Frontend | [tech, e.g. Next.js + Tailwind] |
| Backend (used starting Phase 2) | [tech, e.g. Express + Prisma] |
| Database (used starting Phase 2) | [tech] |

## 🚦 CURRENT_PHASE

**PHASE 1 — Frontend-Only (Mock Data)**

Tasks: #1 to #[Y] from `[TODO-filename].md` (all tasks in groups 1a–1d).
Target: [Phase 1 estimate].

### 🚧 Working Mode for This Phase (MUST BE FOLLOWED)
- You are **ONLY** building the frontend. **FORBIDDEN** to set up a database, backend server, or real API in this phase.
- All data uses **mock/dummy** (see the mock data layer task in the TODO) — the mock data shape MUST match the Database Schema in PRD Section 6 exactly, so it's easy to swap later.
- Service functions (e.g. `getProducts()`) that call mock data MUST have the SAME signature as the ones that will call the real API — so Phase 2 only swaps the implementation.
- All pages/screens in the User Flow (PRD Section 4) must be **clickable end-to-end** even with dummy data — including loading/empty/error states (use simulated delay to feel real).
- Follow the Design System in PRD Section 7 **exactly** (color, font, spacing, radius) — do not improvise new tokens.

### 🛑 MANDATORY GATE — Stop at the End of Phase 1
After ALL Phase 1 tasks (groups 1a–1d) are done and committed:
1. **STOP COMPLETELY.** Do not start Phase 2 tasks (groups 2a–2d) even if you have time/tokens left.
2. Report to the user: a summary of the pages built, how to run the prototype locally, and ask the user to **review + explicitly approve**.
3. **Wait for the user's reply.** Generic sentences like "proceed" are not necessarily Phase 1 approval — first make sure the user has actually tried the prototype. If in doubt, ask back.
4. Only after explicit approval is received, replace the `CURRENT_PHASE` block above with **PHASE 2** (see Variation D below) and continue to task #[X next].

## 📐 Working Principles (MUST BE FOLLOWED)

1. **Mock-first, swap-later** — all Phase 1 data layers are designed to be easy to swap with the real API, not rewritten.
2. **[Principle 2]** — [Short explanation]
3. **[Principle 3]** — [Short explanation]
4. ... (5–10 principles depending on complexity)

## 🏗 File Architecture (if Next.js / specific framework)

\`\`\`
[project-name]/
├── [short folder tree, mark which parts change in Phase 2]
\`\`\`

## 📋 Task Execution Order

**Phase 1 — Frontend-Only (Mock Data)**:
- #1 [Descriptive task + specific tech]
- #2 [Descriptive task + specific tech]
- ...
- #[Y] [Last Phase 1 task]

**⛔ CHECKPOINT — wait for user approval here before continuing to the line below ⛔**

**Phase 2 — Backend Implementation** (only worked on after approval):
- #[Y+1] [Descriptive task + specific tech]
- ...

## 🎬 Execute Now

Start with **Task #1** (Phase 1).

**Concrete steps for Task #1**:
\`\`\`bash
# 1. [Step 1]
[command]
# 2. [Step 2]
[command]
\`\`\`

After Task #1 is done, update `[TODO-filename].md`:
\`\`\`diff
- [ ] **#1** [Task name] ...
+ [x] **#1** [Task name] ...
\`\`\`

Commit:
\`\`\`bash
git add -A
git commit -m "[type]: [message] (#1)"
\`\`\`

**Report back** using this format:
\`\`\`
✅ Task #[X] done: [Task name].
- Files created: [list]
- Commit: [hash]
- Next: Task #[X+1] — [Name].
- Blockers: [none / or explanation]
\`\`\`

## ⚠️ Hard Limits

- **DO NOT** set up a database/backend/real API before entering Phase 2.
- **DO NOT** start Phase 2 tasks before the user explicitly approves Phase 1 — this is a hard limit, not a suggestion.
- **DO NOT** rewrite the Phase 1 UI from scratch when entering Phase 2 — integrate the API into the existing components.
- **DO NOT** stop or ask for confirmation between tasks in the same phase — auto-continue (see Execution Mode: Full Autopilot). The only mandatory pause is the GATE at the end of Phase 1.
- **DO NOT** [additional forbidden action per project context].
- ... (total 5–8 critical hard limits)

## 📞 Communication

Report after each task, but this is FYI — **NOT** a request for permission to continue. Default is to auto-continue nonstop until all active-phase tasks are done or a real blocker is hit. Mandatory pause ONLY at the end of Phase 1 (GATE) for user approval. If there's a blocker > 30 minutes or an architecture question not covered in the PRD, then stop and ask — don't assume, but also don't stop without a concrete reason.

Starting now, work on task #1 through #[Y] back-to-back without stopping to ask permission. First task: **[First Phase 1 task name]**.
\`\`\`

## PROMPT END — COPY UNTIL HERE

---

## 💡 Usage Tips

| Situation | What to do |
| :--- | :--- |
| Start Phase 1 | Paste the prompt as-is — `CURRENT_PHASE` is already set to Phase 1. |
| Phase 1 done, user has approved | Replace the `CURRENT_PHASE` block with Phase 2 (see Variation D), paste the prompt again. |
| Continuing mid-task | Paste the prompt + add: "Continue from Task #[X], previous status: [condition]". |
| Moving machines / fresh chat | Paste the prompt + make sure the PRD + TODO are in the workspace. |
| Delegating to a coder agent | The prompt is ready — paste it directly into the coder subagent. |

## 🔄 Prompt Variations (Optional)

### Variation A — Solo Developer Mode
Add at the top of the prompt:
\`\`\`
NOTE: I am a solo developer. Work in task-number order, not in parallel.
\`\`\`

### Variation B — Pair Programming Mode (Optional — overrides the default Autopilot)
The default for this prompt is **Full Autopilot** (auto-continue without pause). If you actually WANT to review each commit, add this at the top of the prompt:
\`\`\`
NOTE: Override execution mode — I want to review each commit. After completing
1 task, STOP and wait for my approval before starting the next task.
Do not auto-continue even though that's the default.
\`\`\`

### Variation C — Demo-First Mode (for pitches)
\`\`\`
NOTE: I need a Phase 1 demo in [X] days. Prioritize the Quick Wins first:
1. [Quick win 1] — [Y] days
2. [Quick win 2] — [Y] days
After that, continue in the regular Phase 1 order.
\`\`\`

### Variation D — Phase 1 → Phase 2 Transition (MUST be used after approval)
After the user approves the prototype, replace the `CURRENT_PHASE` block in the prompt with:
\`\`\`
## 🚦 CURRENT_PHASE

**PHASE 2 — Backend Implementation**

Tasks: #[Y+1] to #[N] from `[TODO-filename].md` (all tasks in groups 2a–2d).
Target: [Phase 2 estimate].
Assumption: Phase 1 was approved by the user on [date/short description of approval].

### 🚧 Working Mode for This Phase (MUST BE FOLLOWED)
- Build the backend per the Architecture (PRD Section 5) & Database Schema (PRD Section 6).
- Replace the contents of the Phase 1 mock service functions with real API calls — function signatures STAY THE SAME.
- DO NOT rewrite Phase 1 UI components unless the user has new requirements.
- Remove the simulated delay used for the mock loading state in Phase 1.
- Execution mode remains **Full Autopilot**: continue task after task without asking permission, unless the user overrides to Pair Programming Mode (Variation B).
\`
Paste the full prompt (with this new CURRENT_PHASE) into a new chat / coding agent.
```

### Mandatory Rules for the Implementation Prompt

1. **MUST be inside a ` ``` ` code block** with `--- PROMPT START` / `--- PROMPT END` markers so it's easy to copy.
2. **MUST be self-contained**: the executing agent must NOT need any context outside the PRD + TODO + this prompt.
3. **MUST include `CURRENT_PHASE`** with 2 standard values: **PHASE 1 — Frontend-Only (Mock Data)** and **PHASE 2 — Backend Implementation** (unless the project uses a single-phase structure, see note above).
4. **MUST include a "Working Mode for This Phase" block** that explicitly states what is ALLOWED and FORBIDDEN in that phase.
5. **MUST include an explicit GATE at the end of Phase 1**: instructions to stop completely, report, and wait for approval — not auto-continue.
6. **MUST include the "Execution Mode: Full Autopilot" instruction** explicitly: by default the agent keeps going task after task without asking permission in between, progress reports are FYI not confirmation requests. Do not leave it ambiguous — many coding agents by default like to pause and wait for permission after each task unless explicitly told otherwise.
7. **MUST include concrete steps for the first task**: bash command + diff + commit template for task #1.
8. **MUST include a report format**: standard communication after each task (done / blockers / next) — explicitly marked as FYI, not a permission request.
9. **MUST include Hard Limits** explicitly forbidding starting Phase 2 before approval, forbidding UI rewrites when entering Phase 2, and forbidding stopping/asking for confirmation between tasks.
10. **MUST include Variation D (Phase 1 → Phase 2 Transition)** with the new `CURRENT_PHASE` block ready to paste.
11. **MUST include at least 2 other variations**: Solo Mode + at least 1 more (Pair Programming as opt-in / Demo-First).
12. **Language**: Same as the PRD and TODO language (default: English). Code commands & identifiers stay in English.
13. **Task numbering must sync with the TODO**: task numbers in the prompt = task numbers in the TODO file, continuous from Phase 1 to Phase 2.

### When You Do NOT Need an Implementation Prompt

- PRDs for **non-technical products** (e.g. business processes, SOPs, content strategy) — there is nothing to code.
- PRDs intended as **reference documents** only, not to be executed.
- The user **explicitly asks for only the PRD** or **only the TODO**.

Confirm with the user when in doubt about whether the prompt is needed.
