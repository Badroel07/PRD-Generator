---
name: prd-generator
description: 'Generate a comprehensive Product Requirements Document (PRD) following a standardized 7-section structure (Overview, Requirements, Core Features, User Flow, Architecture, Database Schema, Design & Technical Constraints), plus a self-contained implementation_prompt.md ready to paste into a coding agent. BEFORE generating, runs a mandatory Pre-Planning Interview via the available structured question tool to resolve ambiguities (feature list, tech stack, roles, business rules). For UI projects, asks if the user has a design reference — if not, offers curated presets. Loops until unambiguous or the user opts out ("skip interview"). Trigger on /prd, /prd-generator, /generate-prd, or requests for a PRD, requirements document, or product specs.'
---

# PRD Generator Skill

This skill guides AI Agents to produce a **highly structured, professional, and accurate Product Requirements Document (PRD)** in English, plus a ready-to-paste `implementation_prompt.md` file.

**This file is the routing layer.** Full details for each stage live in the `references/` folder — read the relevant file right before you need it; do not generate from memory or guesswork.

```
prd-generator/
├── SKILL.md                                    (you are here)
└── references/
    ├── interview-guide.md                      (Stage 0 — full question tier list)
    ├── prd-format.md                           (Stage 1 — 7-section PRD template)
    ├── implementation-prompt-template.md       (Stage 1.5 — implementation prompt file template)
    └── design-system-presets.md                (Stage 0 — fallback UI/UX presets)
```

---

## Slash Commands & Triggers

This skill is automatically triggered when the user types any of the following slash commands or keywords:
- `/prd`, `/prd-generator`, `/generate-prd`, `/buat-prd`
- Phrases like *"generate PRD"*, *"create PRD"*, *"build a PRD"*, *"PRD document"*, *"requirements document"*, *"product specs"*, *"implementation prompt"*, etc.

---

## 📦 Output Deliverables (2 Files)

Every time this skill is triggered, the agent MUST produce:

| Output | Type | Contents | Required? |
| :--- | :--- | :--- | :--- |
| `[Project-Name]-PRD.md` | **File** (saved to disk) | Product Requirements Document (7 sections) | ✅ Always |
| `implementation_prompt.md` | **File** (saved beside the PRD) | Self-contained prompt ready to paste into a coding agent | ✅ For technical projects |

> The Implementation Prompt is always saved with the exact filename `implementation_prompt.md`, in the same directory as the PRD. Its contents must be ready to copy and paste into another chat or coding agent to execute the PRD.

**Exceptions**:
- **Implementation Prompt**: Skip only if the project is non-technical (business process, SOP, content strategy) or the user explicitly asks for no prompt.

> This skill **does not produce a separate UI/UX Reference Prompt file**. For projects with a UI, the interview asks whether the user has a design reference — if yes, it's used; if not, the agent offers several ready-to-use design system presets. The result goes directly into PRD Section 7 ("Design & Technical Constraints"). The sub-flow details are in `references/interview-guide.md`; the preset list is in `references/design-system-presets.md`.

---

## 🚦 Stage 0: Pre-Planning Interview (MANDATORY BEFORE GENERATION)

> 📖 **Read `references/interview-guide.md` before starting** — it contains the full question list per tier (1–4), the user-question tool usage rules, the loop & termination logic, and a sample end-to-end flow.

Before writing the PRD, the agent MUST run a **Pre-Planning Interview** to eliminate ambiguity. The interview loops continuously (no round limit) until one of these 2 conditions is met:

1. **No critical ambiguities remain** → proceed to Stage 1.
2. **User explicit opt-out** (*"skip interview"*, *"enough"*, *"just generate"*, *"use defaults"*) → document all assumptions in the PRD's **"Notes & Assumptions"** section, then proceed to Stage 1.

> **Hard rule**: The agent **MUST NOT** stop just because the info gathered is "mostly there." Keep asking — a PRD full of wrong assumptions is worse than a slightly longer interview.

### What counts as "ambiguous" (mandatory clarification)?

Missing any of the following would force the agent to make decisions that **materially change** the scope, tech stack, architecture, or feature set:

- Product/app name · Industry domain · Target platform (web/mobile/desktop/CLI/API) · Users & roles/permissions
- MVP feature list · Critical tech stack · Special workflow/business rules
- External integrations · Deployment target · Compliance/security (when industry requires it) · PRD output language · Auth method

What does **NOT** need to be asked (agent may use defaults):
- Minor typography details, icon libraries, sprint timeline details, naming conventions, state management library, test framework — the agent decides all of these based on framework best practices.

### How to Ask

- **MUST use the structured question tool** available in the environment (e.g. `ask_user_input_v0`) — **NOT** plain text chat questions.
- **Check the tool's schema/limits first** before generating a batch (max questions per call & max options per question may vary by tool/environment — do not assume fixed numbers).
- 2–4 concrete, mutually exclusive options per question, all at the same abstraction level.
- If the tool has no free-text "Other" button, the user can still type a custom answer in their next reply — the agent MUST accept that as a valid answer.
- **Loop until clear**: re-evaluate after each batch. If critical ambiguity remains → next batch.
- **DO NOT** repeat questions already answered, and **DO NOT** ask cosmetic details.
- Ask **per tier** (Tier 1 → 4); skip tiers that are already clear, or re-batch across tiers for efficiency.

### Before generating: show summary + final confirmation

Once no critical ambiguity remains (or the user opts out), show a **summary of confirmed assumptions** and ask one last (binary) question: *"Generate with this info?"* — only then move to Stage 1.

---

## 🧱 Stage 1: Generate the PRD (7 Sections)

> 📖 **Read `references/prd-format.md` before generating** — it contains the precise markdown template, sample Mermaid diagrams (`graph TD` for Architecture, `erDiagram` for Database Schema), and typography rules.

Every PRD MUST have these 7 sections, fully filled out — do not skip or shorten any section:

1. **Overview** — problem background + main product goals
2. **Requirements** — accessibility, users, input data, notifications
3. **Core Features** — list of MVP features
4. **User Flow** — step-by-step user workflow
5. **Architecture** — Mermaid `graph TD` diagram + component descriptions
6. **Database Schema** — Mermaid `erDiagram` + table summary
7. **Design & Technical Constraints** — tech stack, typography rules, UI/layout rules

Adapt the content of each section to the user's project context (product name, business domain), but **keep the exact 7-section structure** as defined in `prd-format.md`.

---

## 🧩 Stage 1.5: Write the Implementation Prompt File

> 📖 **Read `references/implementation-prompt-template.md` before generating** — it contains the precise format for `implementation_prompt.md`.

After the PRD file is saved, the agent MUST write a **self-contained Implementation Prompt** to `implementation_prompt.md` beside the PRD. The user can copy this file's contents into a coding agent (Mavis, Claude Code, Cursor, Cody, or a human engineer) to execute the PRD.

**Output rules (MANDATORY):**
- **Exact filename:** use `implementation_prompt.md`; do not derive a project-specific filename.
- **Same directory:** save it next to `[Project-Name]-PRD.md` so the prompt can reliably refer to that PRD using a relative path.
- **Prompt-only contents:** the file contains only the ready-to-paste prompt, with no explanation, save confirmation, or usage tips around it.

Key characteristics of the prompt itself:
- **Saved as a file**: written to `implementation_prompt.md`, not merely shown inline in chat.
- **Self-contained**: the executing agent has no prior context, so the prompt must reference the PRD file path and include all important info (tech stack, working principles, execution mode, file architecture, first steps, hard limits).
- **Default execution mode: Full Autopilot**: the agent auto-continues task after task without asking permission in between — progress reports are FYI, not confirmation requests.
- **Mandatory approval gate after Phase 1 (Production-Grade Frontend)**: the prompt instructs the agent to build a production-grade frontend with rich, realistic synthetic data (zero demo badges/watermarks/placeholders), stop, and wait for user approval before touching any backend work.

> Skip this stage only if the project is non-technical OR the user explicitly asks for no prompt.

---

## Usage Instructions for the Agent

0. **🚦 Stage 0 is MANDATORY**: Do not generate anything before the interview finishes or the user opts out (see `references/interview-guide.md`). Exception — if the user already provided a super-complete request, you may skip straight to the assumptions summary + final confirmation.
1. **Input Flexibility**: Adapt the content of each section to the user's specific project details, but always keep the 7 PRD sections.
2. **Completeness**: All 7 sections must always exist and be filled in detail — never skip any.
3. **Mermaid Diagrams**: Use valid Mermaid syntax for Architecture (`graph TD`) and Database Schema (`erDiagram`).
4. **Typography Strictness**: Include typography rules exactly as specified in `references/prd-format.md`.
5. **Save 2 Files**: Save `[Project-Name]-PRD.md` and `implementation_prompt.md` in the same directory. The prompt file must contain only the completed, ready-to-paste implementation prompt.
6. **Report Saved Files Clearly**: After writing the deliverables, give a concise confirmation that names or links both saved files. Do not duplicate the prompt inline unless the user explicitly requests it.
7. **Implementation Prompt Exception**: Skip Stage 1.5 ONLY if the project is non-technical OR the user explicitly asks for no prompt. Confirm with the user when in doubt.
8. **UI/UX Reference (not a separate file)**: For projects with a UI, run the "UI/UX Reference" sub-flow in `references/interview-guide.md` — first ask if the user has a reference; if not, offer 4 design system presets from `references/design-system-presets.md`. The result goes into PRD Section 7, NOT a separate file.
9. **Default Execution: Full Autopilot**: The Implementation Prompt MUST explicitly state that by default the agent auto-continues task after task WITHOUT asking permission in between — progress reports are FYI, not confirmation requests. The only mandatory pause is the GATE after the production-grade frontend is ready for review. Per-task confirmation mode (Pair Programming) is only active when the user explicitly opts in.
10. **Production-Grade Frontend & Approval Gate (Phase 1)**: The Implementation Prompt MUST instruct the agent to build a production-grade frontend with high-fidelity realistic synthetic data first — complete visual polish, authentic domain-accurate data, full client-side state interactivity, and ZERO "demo/prototype" gimmicks. After Phase 1 is built and committed, the agent MUST stop and wait for explicit user approval before starting Phase 2 backend work.
11. **Zero Demo Embel-Embel & High-Fidelity Synthetic Data (MANDATORY)**: Prompts generated for coding agents must strictly ban any "Demo", "Demo Mode", "Preview", "Prototype", "Mock Data", or "Database not connected" badges, banners, alerts, or watermarks. The UI must look and feel 100% like a live production website connected to a database. Low-effort dummy placeholders (e.g. "Lorem ipsum", "John Doe", "Product 1", 2-item tables) are strictly forbidden; views must be populated with rich, domain-authentic synthetic records.
12. **Mermaid Syntax Validation (MANDATORY before saving the PRD)**: Before saving the PRD, the agent MUST validate every raw `erDiagram` relationship line against the exact shape `ENTITY1 ||--o{ ENTITY2 : "label"`. The line must contain exactly two entity names, exactly one cardinality token, and one label at the end. Labels must never appear between entities. Each relationship must occupy one physical line; never concatenate or wrap two relationships into one line. If two foreign keys point to the same entity, emit two separate lines with role-specific labels (for example, `"from unit"` and `"to unit"`). Do not place malformed anti-patterns inside a `mermaid` code fence. Same strictness applies to Architecture `graph TD` blocks.
13. **Mermaid Literal Scan (MANDATORY before file write)**: After drafting the Mermaid blocks, inspect the raw text between each diagram fence. Reject and rewrite any relationship line that contains a label before a second entity, more than one cardinality token, a second relationship on the same physical line, an undefined entity, or an entity name with spaces/reserved keywords. Do not save or present the PRD until this scan passes.
14. **Implementation Prompt is a Required File**: For technical projects, the Implementation Prompt MUST be written as `implementation_prompt.md` beside the PRD. It is not sufficient to show it only in chat.

---

## 📝 Change History

- **v2.1**: Eliminated prototype/demo framing and enforced **Production-Grade Frontend with Realistic Synthetic Data**.
  - Renamed Phase 1 in the Implementation Prompt from "Frontend Prototype (Mock Data)" to "Production-Grade Frontend (Realistic Synthetic Data)".
  - Added strict zero-demo policy: banned all "Demo", "Demo Mode", "Preview", "Prototype", and "Mock Data" badges, banners, alerts, and watermarks.
  - Mandated high-fidelity, domain-authentic synthetic records (paving over lazy placeholders like "Lorem ipsum" or "Product 1") and full client-side state interactivity (CRUD, search, filter, pagination).
  - Added Hard Rule #11 in `SKILL.md` enforcing the zero-demo and realistic synthetic data standard.

- **v2.0**: Changed the Implementation Prompt deliverable from an inline chat code block to a required `implementation_prompt.md` file saved alongside the PRD. The prompt remains self-contained and ready to copy into a coding agent.

- **v1.9**: Hardened Mermaid validation after a repeated `erDiagram` parse failure. Malformed relationship examples are no longer placed inside Mermaid fences, one physical line per relationship is mandatory, same-entity dual-FK relationships require separate role-labeled lines, and a literal pre-save scan is required.
- **v1.8**: Restructured the Implementation Prompt for clarity and copy-paste readiness, and enforced a strict no-chatter output rule. The new prompt template uses standard markdown (no excessive emojis inside the prompt), clear bracketed placeholders, and 8 distinct sections (Context, Tech Stack, Mission, Execution Mode, Working Principles, File Architecture, First Steps, Communication, Hard Limits, Optional Variations). The chat output rule now mandates: single code block, NO intro line, NO outro line, NO summary, NO usage tips. Optionally, a one-line PRD save confirmation may appear before the code block; nothing after it. New hard rule #6 in `SKILL.md` codifies this, and new rule #12 explicitly forbids saving the prompt as a file. `references/implementation-prompt-template.md` now includes a "Why this prompt is structured this way" agent reference section, a placeholder-to-source mapping table, and 10 mandatory rules.
- **v1.7**: Added mandatory Mermaid `erDiagram` syntax validation. New section in `references/prd-format.md` covers the exact `ENTITY1 ||--o{ ENTITY2 : "label"` format, 5 common parse errors (label-between-entities, many-to-many without bridge table, entity names with spaces, reserved keywords, ghost entities), and a 7-point self-check checklist the agent MUST run before saving the PRD. New hard rule #11 in `SKILL.md` enforces this self-check.
- **v1.6**: Simplified output — only 1 file (`[Project-Name]-PRD.md`) + 1 chat output (the Implementation Prompt as an inline code block, NOT a file). Removed the TODO List file entirely. The Implementation Prompt is now self-contained and no longer needs to sync task numbers with a separate TODO file. The frontend-first approval gate is preserved as a principle inside the prompt.
- **v1.5**: Implementation Prompt explicitly defaults to **Full Autopilot** — auto-continues task after task without asking permission in between (progress reports = FYI, not confirmation requests). The only mandatory pause is the GATE at the end of Phase 1. Per-task confirmation (Pair Programming) is opt-in.
- **v1.4**: TODO List & Implementation Prompt restructured into **2 phases** (Phase 1 Frontend-Only with mock data → User approval checkpoint → Phase 2 Backend). The Implementation Prompt had an explicit GATE forcing the agent to stop & request approval at the end of Phase 1 before starting Phase 2.
- **v1.3**: UI/UX Reference brought back as an **interview sub-flow** (not a separate file). The user is asked whether they have a design reference; if not, 4 design system presets are offered. The result goes into PRD Section 7.
- **v1.2**: UI/UX Reference Prompt (Appendix C) removed entirely — the skill produced 3 files (PRD, TODO, Implementation Prompt). The `uiux-prompt-template.md` was removed from the package.
- **v1.1**: Restructured into progressive disclosure (concise SKILL.md + `references/`) to save context on trigger. Fixed 2 non-Indonesian character bugs. Max questions per call adjusted to match the user-question tool's actual schema.
- **v1.0**: Initial monolithic version, single file (~1000 lines).
