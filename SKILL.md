---
name: prd-generator
description: 'Generate a comprehensive Product Requirements Document (PRD) following a standardized 9-section Product Management structure (Document Control, Executive Summary & KPIs, Scope & Assumptions, User Personas & RBAC, Functional Requirements, Edge Cases, NFRs, User Flows, Technical & Architecture Specs Appendix). BEFORE generating, runs a mandatory Pre-Planning Interview via the available structured question tool to resolve ambiguities (problem statement, metrics, scope boundaries, feature list, tech stack, roles, business rules, edge cases). For UI projects, asks if the user has a design reference — if not, offers curated presets. Loops until unambiguous or the user opts out ("skip interview"). Trigger on /prd, /prd-generator, /generate-prd, or requests for a PRD, requirements document, or product specs.'
---

# PRD Generator Skill

This skill guides AI Agents to produce a **highly structured, professional, and accurate Product Requirements Document (PRD)** following industry-standard Product Management methodology.

**This file is the routing layer.** Full details for each stage live in the `references/` folder — read the relevant file right before you need it; do not generate from memory or guesswork.

```
prd-generator/
├── SKILL.md                                    (you are here)
└── references/
    ├── interview-guide.md                      (Stage 0 — full question tier list)
    ├── prd-format.md                           (Stage 1 — 9-section PM PRD template)
    └── design-system-presets.md                (Stage 0 — fallback UI/UX presets)
```

---

## Slash Commands & Triggers

This skill is automatically triggered when the user types any of the following slash commands or keywords:
- `/prd`, `/prd-generator`, `/generate-prd`, `/buat-prd`
- Phrases like *"generate PRD"*, *"create PRD"*, *"build a PRD"*, *"PRD document"*, *"requirements document"*, *"product specs"*, etc.

---

## 📦 Output Deliverable (1 File)

Every time this skill is triggered, the agent MUST produce:

| Output | Type | Contents | Required? |
| :--- | :--- | :--- | :--- |
| `[Project-Name]-PRD.md` | **File** (saved to disk) | Product Requirements Document (9 sections PM standard) | ✅ Always |

> This skill **focuses exclusively on the PRD document**. It does not generate secondary implementation prompts or code scaffolding templates.
> For projects with a UI, the interview asks whether the user has a design reference — if yes, it's used; if not, the agent offers several ready-to-use design system presets. The result goes directly into PRD Section 9.4 ("Design System Tokens"). The sub-flow details are in `references/interview-guide.md`; the preset list is in `references/design-system-presets.md`.

---

## 🚦 Stage 0: Pre-Planning Interview (MANDATORY BEFORE GENERATION)

> 📖 **Read `references/interview-guide.md` before starting** — it contains the full question list per tier (1–4), the user-question tool usage rules, the loop & termination logic, and a sample end-to-end flow.

Before writing the PRD, the agent MUST run a **Pre-Planning Interview** to eliminate ambiguity. The interview loops continuously (no round limit) until one of these 2 conditions is met:

1. **No critical ambiguities remain** → proceed to Stage 1.
2. **User explicit opt-out** (*"skip interview"*, *"enough"*, *"just generate"*, *"use defaults"*) → document all assumptions in the PRD's **"Scope & Assumptions" (Section 3)**, then proceed to Stage 1.

> **Hard rule**: The agent **MUST NOT** stop just because the info gathered is "mostly there." Keep asking — a PRD full of wrong assumptions is worse than a slightly longer interview.

### What counts as "ambiguous" (mandatory clarification)?

Missing any of the following would force the agent to make decisions that **materially change** the scope, tech stack, architecture, or feature set:

- Product/app name · Industry domain · Core problem & success metrics (KPIs) · Target platform (web/mobile/desktop/CLI/API) · Users & roles/permissions (RBAC)
- MVP feature engines · Out-of-scope (non-goals) · Critical tech stack · Special workflow/business rules · Critical edge cases
- External integrations · Deployment target · Compliance/security (when industry requires it) · PRD output language · Auth method

What does **NOT** need to be asked (agent may use defaults):
- Minor typography details, icon libraries, sprint timeline details, naming conventions, state management library, test framework — the agent decides all of these based on framework best practices.

### How to Ask

- **MUST use the structured question tool** available in the environment (e.g. `ask_question` / `ask_user`) — **NOT** plain text chat questions.
- **Check the tool's schema/limits first** before generating a batch.
- 2–4 concrete, mutually exclusive options per question, all at the same abstraction level.
- Accept typed free-form answers when the user provides them outside the offered buttons.
- **Loop until clear**: re-evaluate after each batch. If critical ambiguity remains → next batch.
- **DO NOT** repeat questions already answered, and **DO NOT** ask cosmetic details.
- Ask **per tier** (Tier 1 → 4); skip tiers that are already clear, or re-batch across tiers for efficiency.

### Before generating: show summary + final confirmation

Once no critical ambiguity remains (or the user opts out), show a **summary of confirmed scope & assumptions** and ask one last (binary) question: *"Generate with this info?"* — only then move to Stage 1.

---

## 🧱 Stage 1: Generate the PRD (9 Sections PM Standard)

> 📖 **Read `references/prd-format.md` before generating** — it contains the precise markdown template, table definitions, sample Mermaid diagrams (`graph TD` for Architecture, `erDiagram` for Database Schema), and typography/color rules.

Every PRD MUST have these 9 sections, fully filled out — do not skip or shorten any section:

1. **Document Control & Metadata** — document version, status (Draft/Review/Approved), author, reviewer, last updated, target release.
2. **Executive Summary & Problem Statement** — background & problem statement, vision & objectives, measurable KPIs & target metrics.
3. **Scope & Assumptions** — core assumptions, in-scope MVP capabilities, explicit out-of-scope (non-goals).
4. **User Personas & Permissions** — target personas, user roles, RBAC permissions matrix.
5. **Functional Requirements** — core capability engines/epics, user stories, acceptance criteria, business logic (no micro-CSS or premature styling).
6. **Edge Cases & Exception Handling** — failure modes, network disruptions, GPS drift, hardware failure, user feedback & recovery.
7. **Non-Functional Requirements (NFR)** — usability & accessibility, performance & latency budgets, security & data integrity, reliability.
8. **User Flows & Visual Diagrams** — end-to-end user journeys (ASCII diagrams & Mermaid flowcharts/state diagrams).
9. **Technical & Architecture Specs (Appendix / TRD Bridge)** — system architecture diagram (`graph TD`), data model & ERD (`erDiagram`), tech stack recommendations, design system tokens.

Adapt the content of each section to the user's project context (product name, business domain), but **keep the exact 9-section structure** as defined in `prd-format.md`.

---

## Usage Instructions for the Agent

0. **🚦 Stage 0 is MANDATORY**: Do not generate anything before the interview finishes or the user opts out (see `references/interview-guide.md`). Exception — if the user already provided a super-complete request, you may skip straight to the assumptions summary + final confirmation.
1. **Input Flexibility**: Adapt the content of each section to the user's specific project details, but always keep the 9 PRD sections.
2. **Completeness**: All 9 sections must always exist and be filled in detail — never skip any.
3. **Mermaid Diagrams**: Use valid Mermaid syntax for Architecture (`graph TD`), States (`stateDiagram-v2`), and Database Schema (`erDiagram`).
4. **Design & Styling Separation (WHAT/WHY vs HOW)**: Do NOT put Tailwind classes or hex codes into Section 5 (Functional Requirements). Place styling tokens strictly in Section 9.4 (Design System Tokens) and layout constraints in Section 7.1 (Usability & Accessibility).
5. **Save 1 File**: Save `[Project-Name]-PRD.md` to disk. The skill produces strictly the PRD document without secondary prompt files.
6. **Report Saved File Clearly**: After writing the PRD, give a concise confirmation that names or links the saved PRD file.
7. **UI/UX Reference (not a separate file)**: For projects with a UI, run the "UI/UX Reference" sub-flow in `references/interview-guide.md` — first ask if the user has a reference; if not, offer 4 design system presets from `references/design-system-presets.md`. The result goes into PRD Section 9.4, NOT a separate file.
8. **Mermaid Syntax Validation (MANDATORY before saving the PRD)**: Before saving the PRD, the agent MUST validate every raw `erDiagram` relationship line against the exact shape `ENTITY1 ||--o{ ENTITY2 : "label"`. The line must contain exactly two entity names, exactly one cardinality token, and one label at the end. Labels must never appear between entities. Each relationship must occupy one physical line; never concatenate or wrap two relationships into one line. If two foreign keys point to the same entity, emit two separate lines with role-specific labels (for example, `"from unit"` and `"to unit"`). Do not place malformed anti-patterns inside a `mermaid` code fence. Same strictness applies to Architecture `graph TD` blocks.
9. **Mermaid Literal Scan (MANDATORY before file write)**: After drafting the Mermaid blocks, inspect the raw text between each diagram fence. Reject and rewrite any relationship line that contains a label before a second entity, more than one cardinality token, a second relationship on the same physical line, an undefined entity, or an entity name with spaces/reserved keywords. Do not save or present the PRD until this scan passes.

---

## 📝 Change History

- **v3.0**: **Pure Product Management PRD Standard (Single Deliverable)**.
  - Eliminated `implementation_prompt.md` deliverable and Stage 1.5. Output is strictly 1 file: `[Project-Name]-PRD.md`.
  - Removed all artificial coding-agent execution constraints (Phase 1/Phase 2, Approval Gate, Production-Grade Frontend, Realistic Synthetic Data mandates, and Zero-Demo rules).
  - Streamlined the skill to focus 100% on standard, comprehensive, and professional Product Requirements Documents.
- **v2.2**: Upgraded to **9-Section Product Management Standard PRD Structure**.
  - Restructured PRD into 9 standard PM sections: Document Control, Executive Summary & KPIs, Scope & Assumptions, User Personas & Permissions, Functional Requirements, Edge Cases & Exception Handling, Non-Functional Requirements, User Flows & Visual Diagrams, and Technical & Architecture Specs (Appendix / TRD Bridge).
  - Eliminated redundancy between former Section 2 (Requirements) and Section 3 (Core Features) by establishing consolidated **User Personas & Roles** (Section 4) and **Functional Requirements** (Section 5) organized by Core Engines / Epics.
  - Enforced strict separation of **WHAT/WHY vs HOW**: banned premature styling (Tailwind CSS classes, hex colors) from functional specifications, encapsulating design tokens cleanly in Section 9.4 and layout constraints in Section 7.1.
  - Added **Document Control & Metadata** (Section 1) with semver, author, status, and milestone tracking.
  - Added **Measurable Success Metrics & KPIs** (Section 2.3) with baseline vs. target benchmarks.
  - Placed **Scope & Assumptions** (Section 3) upfront with explicit **Out-of-Scope (Non-Goals)** to prevent scope creep from day one.
  - Added dedicated **Edge Cases & Exception Handling** (Section 6) covering network disruption, GPS drift, hardware failure, and user feedback recovery.
  - Relegated architecture diagrams (`graph TD`), data model (`erDiagram`), tech stack, and design tokens into **Technical & Architecture Specs** (Section 9) as a clean TRD bridge.
- **v2.1**: Eliminated prototype/demo framing and enforced Production-Grade Frontend with Realistic Synthetic Data.
- **v2.0**: Changed the Implementation Prompt deliverable from an inline chat code block to a required `implementation_prompt.md` file saved alongside the PRD.
- **v1.9**: Hardened Mermaid validation after a repeated `erDiagram` parse failure.
- **v1.8**: Restructured the Implementation Prompt for clarity and copy-paste readiness.
- **v1.7**: Added mandatory Mermaid `erDiagram` syntax validation.
- **v1.6**: Simplified output to 1 file + 1 chat output.
- **v1.5**: Implementation Prompt explicitly defaults to Full Autopilot.
- **v1.4**: 2 phases structure.
- **v1.3**: UI/UX Reference interview sub-flow.
- **v1.2**: Removed separate UI/UX prompt file.
- **v1.1**: Restructured into progressive disclosure (`SKILL.md` + `references/`).
- **v1.0**: Initial monolithic version.
