---
name: prd-generator
description: 'Generate comprehensive Product Requirements Documents (PRD) following a standardized 7-section structure (Overview, Requirements, Core Features, User Flow, Architecture, Database Schema, Design & Technical Constraints) PLUS two companion deliverables — a 2-phase TODO List (Phase 1: frontend-only with mock data for a fast prototype, mandatory user-approval checkpoint, then Phase 2: backend implementation replacing the mock data) and a self-contained Implementation Prompt enforcing the same phase gate. BEFORE generating, runs a mandatory Pre-Planning Interview via the available structured question tool to resolve ambiguities (feature list, tech stack, roles, business rules). For UI projects, asks if the user has a design reference — if not, offers curated presets. Loops until unambiguous or the user opts out ("skip interview"). Trigger on /prd, /prd-generator, /generate-prd, or requests for a PRD, requirements document, product specs, or implementation prompt.'
---

# PRD Generator Skill

This skill guides AI Agents to produce a **highly structured, professional, and accurate Product Requirements Document (PRD)** in English, plus 2 ready-to-execute companion files.

**This file is the routing layer.** Full details for each stage live in the `references/` folder — read the relevant file right before you need it; do not generate from memory or guesswork.

```
prd-generator/
├── SKILL.md                                    (you are here)
└── references/
    ├── interview-guide.md                      (Stage 0 — full question tier list)
    ├── prd-format.md                           (Stage 1 — 7-section PRD template)
    ├── todo-template.md                        (Stage 2 — Appendix A)
    ├── implementation-prompt-template.md       (Stage 2 — Appendix B)
    └── design-system-presets.md                (Stage 0 — fallback UI/UX presets)
```

---

## Slash Commands & Triggers

This skill is automatically triggered when the user types any of the following slash commands or keywords:
- `/prd`, `/prd-generator`, `/generate-prd`, `/buat-prd`
- Phrases like *"generate PRD"*, *"create PRD"*, *"build a PRD"*, *"PRD document"*, *"requirements document"*, *"product specs"*, *"implementation prompt"*, etc.

---

## 📦 Output Deliverables (3 Mandatory Files)

Every time this skill is triggered, the agent MUST produce **3 markdown files** in the same folder:

| File | Contents | Required? |
| :--- | :--- | :--- |
| `[Project-Name]-PRD.md` | Product Requirements Document (7 sections) | ✅ Always |
| `[Project-Name]-TODO.md` | Task list **2 phases**: Phase 1 (Frontend-Only, mock data) → Approval checkpoint → Phase 2 (Backend) | ✅ Always |
| `[Project-Name]-IMPLEMENTATION-PROMPT.md` | Prompt ready to paste into a coding agent | ✅ For technical projects |

All three files are **SYNCHRONIZED** — task numbers in the TODO must match the references in the Implementation Prompt.

**Exceptions**:
- **Implementation Prompt**: Skip only if the project is non-technical (business process, SOP, content strategy) or the user explicitly asks for no prompt.
- **2-Phase Structure**: Skip (use regular priority grouping instead) only if the project has no UI, or the frontend consumes an existing external API (no custom backend to build).

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

## 🧩 Stage 2: Generate the 2 Companion Files

| Appendix | Output File | Reference |
| :--- | :--- | :--- |
| A — TODO List | `[Project-Name]-TODO.md` | `references/todo-template.md` |
| B — Implementation Prompt | `[Project-Name]-IMPLEMENTATION-PROMPT.md` | `references/implementation-prompt-template.md` |

Read the corresponding reference file **right before generating that appendix** — each has its own precise format, mandatory rules, and conditions for when it can be skipped (see "When NOT to..." in each file).

**Numbering consistency is mandatory**: Task numbers in the TODO = task numbers referenced in the Implementation Prompt. Sync them before the final output.

---

## Usage Instructions for the Agent

0. **🚦 Stage 0 is MANDATORY**: Do not generate anything before the interview finishes or the user opts out (see `references/interview-guide.md`). Exception — if the user already provided a super-complete request, you may skip straight to the assumptions summary + final confirmation.
1. **Input Flexibility**: Adapt the content of each section to the user's specific project details, but always keep the 7 PRD sections.
2. **Completeness**: All 7 sections must always exist and be filled in detail — never skip any.
3. **Mermaid Diagrams**: Use valid Mermaid syntax for Architecture (`graph TD`) and Database Schema (`erDiagram`).
4. **Typography Strictness**: Include typography rules exactly as specified in `references/prd-format.md`.
5. **Generate the 2 Companion Files (MANDATORY)**: After the PRD is done, always generate `[Project-Name]-TODO.md` and `[Project-Name]-IMPLEMENTATION-PROMPT.md` in the same folder — follow each reference file.
6. **Output Summary**: At the end, show the TODO item count + Phase 1/Phase 2 distribution, the task count in the Implementation Prompt, and notes on the prompt variations available.
7. **Implementation Prompt Exception**: Skip Appendix B ONLY if the project is non-technical OR the user explicitly asks for no prompt. Confirm with the user when in doubt.
8. **Numbering Consistency**: Sync task numbers between the TODO and Implementation Prompt before final output — numbers continue from Phase 1 into Phase 2, do not reset.
9. **UI/UX Reference (not a separate file)**: For projects with a UI, run the "UI/UX Reference" sub-flow in `references/interview-guide.md` — first ask if the user has a reference; if not, offer 4 design system presets from `references/design-system-presets.md`. The result goes into PRD Section 7, NOT a 4th file.
10. **2-Phase Structure (MANDATORY for fullstack projects)**: The TODO and Implementation Prompt MUST be split into Phase 1 (Frontend-Only, mock data, fast prototype) → User approval checkpoint (MANDATORY) → Phase 2 (Backend, replace mock data with real integration). The Implementation Prompt MUST include explicit "stop and wait for approval" instructions at the end of Phase 1 — see `references/implementation-prompt-template.md`. Skip this structure ONLY for projects without UI, or where the frontend consumes an existing external API (see the exception in `references/todo-template.md`).
11. **Default Execution: Full Autopilot**: The Implementation Prompt MUST explicitly state that by default the agent auto-continues task after task WITHOUT asking permission in between — progress reports are FYI, not confirmation requests. The only mandatory pause is the GATE at the end of Phase 1. Per-task confirmation mode (Pair Programming) is only active when the user explicitly opts in.

---

## 📝 Change History

- **v1.5**: Implementation Prompt now explicitly defaults to **Full Autopilot** — the agent auto-continues task after task without asking permission in between (progress reports = FYI, not confirmation requests). The only mandatory pause remains the GATE at the end of Phase 1. Per-task confirmation mode (Pair Programming) is now opt-in, not the default.
- **v1.4**: TODO List & Implementation Prompt restructured into **2 phases**: Phase 1 (Frontend-Only with mock data, for a fast prototype) → User approval checkpoint (MANDATORY) → Phase 2 (Backend, replace mock data with real integration). The Implementation Prompt now has an explicit GATE that forces the agent to stop & request approval at the end of Phase 1 before starting Phase 2.
- **v1.3**: UI/UX Reference brought back, but as an **interview sub-flow** (not a separate file). The user is asked whether they have a design reference; if not, the agent offers 4 design system presets (`references/design-system-presets.md`) for selection. The result goes into PRD Section 7.
- **v1.2**: UI/UX Reference Prompt (Appendix C) removed entirely per request — the skill now produces only 3 files (PRD, TODO, Implementation Prompt). The `references/uiux-prompt-template.md` file was removed from the package.
- **v1.1**: Restructured into progressive disclosure (concise SKILL.md + `references/`) to save context on trigger. Fixed 2 non-Indonesian character bugs that snuck in (in a Tier 3 interview example and the Appendix C intro, before Appendix C was removed). Max questions per call adjusted to match the actual user-question tool's schema (check before assuming fixed numbers).
- **v1.0**: Initial version, single monolithic file (~1000 lines).
