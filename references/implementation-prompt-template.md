# Implementation Prompt — File Template

> **Called from**: `SKILL.md`, Stage 1.5.
> **Purpose**: Defines the exact structure of the Implementation Prompt that the agent saves as `implementation_prompt.md` beside the PRD. The prompt is self-contained, ready to copy-paste into a coding agent (Mavis, Claude Code, Cursor, etc.) to execute the PRD.
> **Default behavior**: After saving the PRD, the agent writes `implementation_prompt.md` with only the completed prompt — no save confirmation, usage tips, or surrounding explanation inside the file.
> **Skip this output ONLY** when the project is non-technical (SOP, content strategy) or the user explicitly asks for no prompt.

---

## File Output Rules (MANDATORY)

When the agent writes the Implementation Prompt, it MUST follow these rules:

1. **Use the exact filename `implementation_prompt.md`.** Save it in the same directory as the PRD.
2. **Prompt-only file contents.** Do not wrap the prompt in an outer code fence or add save confirmations, summaries, or usage tips.
3. **Prompt is self-contained.** The user can copy the file's contents and paste them directly into a new coding-agent chat — the executing agent has no prior context.
4. **Use standard Markdown inside the file.** Nested code fences that belong to the prompt itself are allowed.
5. **Chat confirmation is concise.** After writing both files, report their saved paths or links; do not duplicate the full prompt in chat unless requested.

The `Prompt Template` below is the complete file content. Do not include this reference material in the output file.

---

## Prompt Template

The agent fills in the placeholders (`[Project Name]`, `[tech]`, etc.) from the PRD and writes the result to `implementation_prompt.md`. The template uses standard markdown — no excessive emojis, no decorative formatting.

````
# Implementation Prompt — [Project Name]

You are a Senior Fullstack Engineer building **[Project Name]**.

## Context

- PRD: `[PRD-filename].md` (in the same folder as this prompt)
- Read the PRD before starting. All technical decisions must align with it.

## Tech Stack (LOCKED)

| Layer | Technology |
| :--- | :--- |
| Frontend | [tech, e.g. Next.js 14 + Tailwind] |
| Backend (Phase 2 only) | [tech, e.g. Express + Prisma] |
| Database (Phase 2 only) | [tech, e.g. PostgreSQL] |

## Mission: Build in 2 Phases

### Phase 1 — Production-Grade Frontend (Realistic Synthetic Data)

- Build the full UI per PRD Section 4 (User Flow) and Section 3 (Core Features).
- **Production-Grade Fidelity**: The frontend must look, feel, and function like a 100% finished website/application. It must NOT look like a wireframe, rough prototype, or half-baked demo.
- **High-Fidelity Synthetic Data**: Populate views with rich, domain-authentic synthetic records (realistic names, believable metrics, real-world dates, proper statuses, clean avatars/images). Do NOT use lazy placeholders like "Lorem ipsum", "Product 1", "John Doe", or 2-item arrays. The data must feel indistinguishable from a live database.
- **ZERO Demo Gimmicks**: STRICTLY FORBIDDEN to show "Demo", "Demo Mode", "Preview", "Prototype", "Mock Data", or "Database not connected" badges, banners, watermarks, alerts, or disclaimers anywhere in the UI.
- **Full Client-Side Interactivity**: Implement stateful client-side handling for core flows (adding an item updates the table, editing updates the record, deleting removes it, search filters live records, pagination navigates pages, modals open/close with real form state). Do NOT disable buttons with "Demo version" popups.
- **Swap-Ready Service Layer**: Write async service functions with signatures matching the future real API (e.g. `getItems()`, `createItem()`), returning synthetic data so Phase 2 is purely a backend swap, not a UI rewrite.
- Follow PRD Section 7 (Design System) exactly — colors, fonts, spacing, radius.

### Approval Gate (MANDATORY STOP)

After Phase 1 is fully built, tested, and committed:

1. STOP. Do not start Phase 2.
2. Report what was built, how to run it locally, and request user review.
3. Wait for explicit user approval before continuing.

### Phase 2 — Backend Implementation (After Approval Only)

- Build the backend per PRD Section 5 (Architecture) and Section 6 (Database Schema).
- Replace synthetic data resolvers in the service layer with real API calls / database queries.
- Connect authentication and persistent storage.
- Do NOT rewrite the Phase 1 UI from scratch.

## Execution Mode: Full Autopilot

- Work continuously within each phase. No permission-asking between steps.
- Progress reports are informational, not confirmation requests.
- The ONLY mandatory pause is the Approval Gate at the end of Phase 1.
- Only stop for real technical blockers — not for "I want to check with the user first."

## Working Principles

1. **Production-grade frontend first** — UI/UX must look and behave like a finished product from day one.
2. **Realistic synthetic data, swap-ready** — data looks completely real; service signatures match the future API for an easy Phase 2 swap.
3. **Zero demo embel-embel** — no demo badges, watermarks, or mock banners anywhere.
4. **Frontend complete before backend** — no Phase 2 work until Phase 1 is approved.
5. **Follow design system exactly** — only use tokens from PRD Section 7.
6. **Commit incrementally** — small focused commits, conventional messages (`feat:`, `fix:`, `chore:`, etc.).
7. [Project-specific principle, e.g. "Use server components by default in Next.js."]

## File Architecture

```
[project-name]/
├── [folder tree, mark which parts are added in Phase 2]
```

## First Steps

```bash
# 1. [Step 1]
[command]
# 2. [Step 2]
[command]
```

## Communication

After each meaningful step, report in this format:

```
Done: [step name]
- Files: [list of files created or changed]
- Commit: [hash]
- Next: [next step name]
- Blockers: [none / or explanation]
```

These reports are FYI, not permission requests. Default: auto-continue nonstop. Only pause at the Approval Gate.

## Hard Limits

- DO NOT add any "Demo", "Prototype", "Preview", or "Mock Data" badges, banners, watermarks, or disclaimer text anywhere in the UI.
- DO NOT use low-effort dummy placeholders ("Lorem ipsum", "Product 1", "John Doe", single-item lists). Use rich, domain-authentic synthetic records.
- DO NOT disable interactive actions with "Demo only" or "Database not connected" alerts. Implement functional stateful client-side handlers for core flows.
- DO NOT set up a database, backend, or real API before Phase 1 approval.
- DO NOT start Phase 2 before explicit user approval.
- DO NOT rewrite Phase 1 UI in Phase 2 — integrate the API into the existing components.
- DO NOT stop or ask for confirmation between steps in the same phase.
- DO NOT use design tokens that are not in PRD Section 7.

## Optional Variations

Add any of these at the very top of the prompt (before "Implementation Prompt — [Project Name]") if the user requests them.

- **Solo Mode**: `NOTE: Solo developer. Work step by step, not in parallel.`
- **Pair Programming Mode**: `NOTE: I want to review each commit. After 1 step, STOP and wait for approval.`
- **Fast-Track UI Mode**: `NOTE: Need Phase 1 UI delivered quickly. Prioritize [feature list] first.`
- **Skip Frontend Gate** (advanced, higher risk): `NOTE: Skip the frontend-first gate. Build full stack in one pass.`

Begin Phase 1 now.
````

---

## Agent Reference (NOT part of the saved prompt)

This section is for the agent's understanding. It is NEVER included in `implementation_prompt.md`.

### Why this prompt is structured this way

- **Self-contained**: The executing agent has no prior context, so the prompt includes the tech stack table, working principles, file architecture, first steps, and hard limits — everything needed to start without follow-up questions.
- **Single approval gate**: Phase 1 must end with a hard stop. This is the only intentional pause. Everything else is auto-continue.
- **No emojis inside the prompt**: The prompt is meant to be copy-pasted into a coding agent. Excessive emojis (`🚀`, `✅`, `🛑`, etc.) look messy when copied and add no functional value. The single `🛑` before the Approval Gate is a deliberate visual marker for the one mandatory stop.
- **Clean markdown**: The prompt uses standard markdown (headings, tables, code fences) that any coding agent can render and understand.
- **Explicit placeholders**: `[Project Name]`, `[tech]`, etc. are easy to find and replace. The agent fills them in from the PRD's Overview, Requirements, and Design & Technical Constraints sections.

### What the agent extracts from the PRD to fill the prompt

| Placeholder | Source in PRD |
| :--- | :--- |
| `[Project Name]` | Top of PRD (`# PRD — Product Requirements Document: [Name]`) |
| `[PRD-filename].md` | The actual filename the agent is saving |
| `[tech]` (Frontend, Backend, Database) | PRD Section 7 (Design & Technical Constraints → High-Level Technology) + Tier 3 interview answers |
| `[project-name]` (folder) | Derived from project name in `kebab-case` |
| `[folder tree]` | Project structure derived from tech stack (e.g. Next.js App Router, Express src/ structure) |
| First steps commands | Standard project initialization for the chosen tech stack |
| `[Project-specific principle]` | Any non-obvious principle from the interview or PRD (e.g. "use server components", "RBAC is owner-only", etc.) |

### What the agent must NOT do

- DO NOT add intro text (e.g. "Below is the prompt...") or save confirmations to `implementation_prompt.md`.
- DO NOT include the "Why this prompt is structured this way" or "What the agent extracts" sections in the saved prompt — those are for the agent's own reasoning.
- DO NOT include usage tips, variations explanations, mandatory rules, or "when to skip" sections in the saved prompt.
- DO NOT rely on chat output as the deliverable; save the completed prompt to `implementation_prompt.md`.

---

## Mandatory Rules for the Agent

1. **MUST save the prompt as `implementation_prompt.md`** in the same directory as the PRD.
2. **MUST NOT** add intro phrases like "Below is the prompt" or "Copy this" inside the file.
3. **MUST NOT** add outro phrases like "Now paste it into a new chat" or "Tip: you can also use..." inside the file.
4. **MUST fill all placeholders** (`[Project Name]`, `[tech]`, `[project-name]`, etc.) from the PRD before output. No `[TBD]` or `[fill in later]` left in the prompt.
5. **MUST include the Tech Stack table** with all three layers (Frontend, Backend, Database), even if the user only mentioned the frontend.
6. **MUST include the Approval Gate** as a hard stop after Phase 1 (Production-Grade Frontend). The prompt must explicitly say "STOP", "Do not start Phase 2", and "wait for explicit user approval".
7. **MUST include the Full Autopilot mode** explicitly. Many coding agents by default pause after each step unless told otherwise.
8. **MUST include the Hard Limits** as a numbered list — at least the 8 critical ones (including the zero-demo and realistic synthetic data rules).
9. **MUST keep the prompt as standard markdown** — no proprietary syntax, no chat-platform-specific formatting.
10. **MUST keep placeholders bracketed** with `[...]` so the user can see what was filled in vs. what is custom.
11. **MUST enforce Production-Grade Frontend & Realistic Synthetic Data in Phase 1**: The generated prompt must instruct the coding agent to build a finished-quality website/app with rich, domain-authentic data, strictly forbidding any "Demo", "Prototype", "Preview", or "Mock" badges, watermarks, or lazy dummy placeholders.

## When You Do NOT Need an Implementation Prompt

- PRDs for **non-technical products** (e.g. business processes, SOPs, content strategy) — there is nothing to code.
- PRDs intended as **reference documents** only, not to be executed.
- The user **explicitly asks for only the PRD**.

Confirm with the user when in doubt about whether the prompt is needed.
