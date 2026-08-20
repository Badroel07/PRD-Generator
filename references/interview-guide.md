# Complete Guide: Stage 0 — Pre-Planning Interview

> **Called from**: `SKILL.md`, Stage 0.
> **Contents of this file**: The full question list per tier (1–4), the user-question tool usage rules, the loop & termination logic, and a sample end-to-end flow.
> **Read this file BEFORE starting the interview** — do not generate questions from memory or guesswork.

---

## 🚦 Stage 0: Pre-Planning Interview — Ambiguity Resolution (MANDATORY BEFORE GENERATION)

Before the agent starts writing the PRD, the agent MUST run a **Pre-Planning Interview** to eliminate ambiguity. **THERE IS NO QUESTION LIMIT** — the agent keeps asking (looping) until **the plan is no longer ambiguous** or the user explicitly opts out.

### When the Interview Stops (Only 2 Conditions)

1. **No critical ambiguities remain** → the agent moves to Stage 1 (generate the PRD file).
2. **User explicit opt-out** with keywords such as *"skip interview"*, *"enough"*, *"just generate"*, *"use defaults"*, *"that's enough"*, *"proceed"* → the agent documents all assumptions in the PRD's **"Notes & Assumptions"** section, then moves to Stage 1.

> **Hard rule**: The agent **MUST NOT** stop just because the info gathered is "mostly there." Keep looping until the plan is clear or the user stops. Keep asking = better than a PRD full of wrong assumptions.

### Definition: What is "Ambiguous"?

A user input is called **ambiguous** (mandatory clarification) when its absence would force the agent to make decisions that **materially change** the scope, tech stack, architecture, or feature set of the PRD. Examples of ambiguity:

- ❌ No **product/app name**
- ❌ No **industry domain** (retail, F&B, B2B SaaS, education, healthcare, logistics, etc.)
- ❌ No **target platform** info (web app / native mobile / mobile web / desktop / CLI / API-SDK / IoT)
- ❌ No **user/role/permission** info
- ❌ No **MVP feature** list (just "build app X" with no feature spec)
- ❌ No **UI/UX reference** (Figma, mockup, brand kit, design system, sample app)
- ❌ **Tech stack** not mentioned even though it's critical to implementation choices
- ❌ **Workflow / business rule** not clear (FIFO/LIFO stock, multi-step approval, recalculation, etc.)
- ❌ No **external integration** info (payment gateway, SSO, third-party API, email service, cloud storage)
- ❌ No **deployment target** info (Vercel / self-host Docker / on-premise / specific cloud)
- ❌ **Compliance/security** unclear when the industry requires it (GDPR, HIPAA, PCI-DSS)
- ❌ **PRD output language** not specified (English / Indonesian)
- ❌ **Auth method** unclear for projects with login

What is **NOT ambiguous** (agent may use defaults without asking):

- ✅ Minor typography details (font size, line-height) → use Section 7 defaults
- ✅ Icon library (lucide-react default)
- ✅ Sprint timeline details → agent estimates based on complexity
- ✅ Quick wins → agent decides based on best practice
- ✅ File/folder naming convention → agent picks the idiomatic choice per framework
- ✅ State management library per framework → use community default
- ✅ Test framework per language → default (Jest/Vitest for JS, pytest for Python, etc.)

### How to Ask: MUST Use `ask_user`

The agent **MUST** use the structured question tool `ask_user` — **NOT** plain text chat questions. Key rules:

- **Maximum 3 questions per call, maximum 4 options per question** — these are the limits of the Claude `ask_user_input_v0` user-question tool as of August 2026. **Check the schema of the available tool in your environment before generating a batch** — limits may differ across platforms/tools.
- **2–4 concrete options per question**, mutually exclusive, all at the same abstraction level.
- **No built-in "Other" button** in this tool — if the user wants to answer outside the offered options, they can simply type a free-form answer in their next reply. The agent MUST accept and process free-form answers as valid; do not force the user to pick one of the buttons.
- **Smart batching**: re-batch across tiers when more efficient (e.g. 2 from Tier 2 + 2 from Tier 3 in one call).
- **Loop until clear**: after each batch of answers, re-evaluate. If critical ambiguity remains, next batch → ask again → evaluate → repeat.
- **DO NOT repeat questions** that have already been answered (check chat history first).
- **DO NOT ask cosmetic details** that can be assumed from PRD Section 7.

### Question Tiers (Progressive, Not Bombardment)

The agent asks **per tier**, starting at Tier 1, and **MAY skip a tier** when the info is already clear from the initial request or previous tier answers.

#### Tier 1 — Identity & Scope (MANDATORY unless already clear from the request)

1. **Product/app name** — if the user hasn't given one.
2. **Industry domain** — retail, F&B, B2B/B2C SaaS, education, healthcare, logistics, finance, manufacturing, etc.
3. **Target platform** — web app, native mobile (iOS/Android), mobile web/PWA, desktop (Electron/Tauri), CLI, API/SDK, embedded/IoT, or a combination.
4. **Target users & roles** — who is the primary user, what roles exist, whether multi-level RBAC is needed.
5. **Whether a PRD/reference document already exists** — for consistency (if yes, ask for the file location).
6. **PRD output language** — English or Indonesian (default: English, matching the user's language).

#### Tier 2 — Features & Workflow (Asked when the user's features are still vague)

1. **Required MVP features** — list of core features (1–3 sentences each), or pick a preset by domain.
2. **What is NOT in MVP** — explicit out-of-scope (not built in the initial phase).
3. **Special workflow / business rules** — approval flow, FIFO/LIFO, auto-numbering, multi-step form, specific validations, etc.
4. **Main data entities** — list of entities (User, Product, Order, etc.) or preset by domain.
5. **Reporting / analytics** — which reports must exist in MVP (sales, stock, finance, etc.).

#### Tier 3 — Tech & Design (Asked only when not yet specified & critical)

1. **Tech stack preference** — framework, language, database, ORM (or *"pick the best for this use case"* → agent decides).
2. **UI/UX reference** — see the dedicated sub-flow below ("UI/UX Reference — Mandatory Sub-Flow"); do NOT just ask via a generic question.
3. **Color/typography preference** — usually already answered by sub-flow point 2 above. Ask again ONLY if the user wants to override part of the preset/reference they already chose.
4. **Auth method** — email/password, OAuth (Google/GitHub/Apple), magic link, enterprise SSO, or a combination.
5. **External integrations** — payment gateway, email service, third-party API, cloud storage, CDN, search engine.
6. **Deployment target** — Vercel/Netlify, AWS/GCP/Azure, self-host Docker, on-premise, or *"don't know yet"*.

##### UI/UX Reference — Mandatory Sub-Flow (for projects with a UI)

> 📖 **Read `references/design-system-presets.md`** before running this sub-flow — it contains 4 ready-to-use presets with tokens.

For projects that have a user interface, DO NOT just assume or generate a design system from scratch. Run this sequence:

1. **Ask first** (via the user-question tool, single_select, 2 options): *"Do you have a design reference for this project (Figma, mockup, brand kit, or an app that serves as inspiration)?"*
   - Options: **"Yes, I do"** / **"No, not yet"**
2. **If "Yes, I do"** → ask the user to explain freely in chat (link, description, app name, screenshot, etc. — no need to go through the structured tool, a regular message is fine). The agent then extracts/infers design tokens (colors, fonts, radius, etc.) from that description. If the reference is just an app name without technical details (e.g. *"like Notion"*), you may reasonably infer but MUST note it as an assumption in "Notes & Assumptions".
3. **If "No, not yet"** → present the **4 design system presets** from `references/design-system-presets.md` via the user-question tool (single_select, 4 options: Modern Minimalist / Bold & Friendly / Corporate & Formal / Dark Tech). Briefly explain each preset in chat before/after the tool is called so the user understands the differences, then let them pick one.
4. **The result from step 2 or 3** is mapped directly into PRD Section 7 (Design & Technical Constraints) — exact tokens (hex, font name, radius), not vague descriptions.
5. The user can still request token changes at any time after the PRD is done — this is not a permanent decision.

#### Tier 4 — Non-Functional (Asked ONLY when material to scope)

1. **Performance / scale** — expected user count, data volume, peak load, response time target.
2. **Compliance / security** — GDPR, HIPAA, PCI-DSS, ISO 27001, SOC 2, or local standards (e.g. Indonesia's UU PDP, etc.).
3. **Multi-tenancy** — whether multi-tenant (one app for many organizations) or single-org.
4. **i18n / multi-language** — whether multiple locale support (i18n) is needed or single language is enough.
5. **Notification channels** — email, push notification, in-app, SMS, WhatsApp, webhook.
6. **File upload / media handling** — need to upload files? What formats? Max size? (images, documents, video).
7. **Offline / PWA** — whether the app must work without internet (PWA, sync queue).
8. **Audit log** — whether all data changes need to be tracked for compliance/debug.

### Interview Opening Message Template

When entering interview mode, the agent MUST open with a message (or equivalent) to set expectations:

```
Alright, before I generate the PRD, I need to clarify a few things so the
output hits the mark and we don't end up with off-target assumptions.

I'll ask via question cards — answer as you like, or pick the option that
fits best. If you already have an answer in mind, just type it via the
"Other" option.

If you're tired of it and want to skip the interview, just say
"skip interview" or "just generate with defaults" — I'll record all the
assumptions I'm making in the PRD's "Notes & Assumptions" section.

Starting with the first question...
```

### Loop & Termination Logic

After each `ask_user` batch is answered, the agent runs this loop:

1. **Parse the user's answers** from all questions in the batch.
2. **Check for remaining ambiguity** (refer to "Definition: What is Ambiguous?" above).
3. **If critical ambiguity remains** → next batch of questions (next tier or previously skipped questions) → ask again via `ask_user` → go to step 1.
4. **If no critical ambiguity remains** → show the user a **summary of confirmed assumptions**, ask for final confirmation: *"Generate with this info?"* (1 last question, binary) → move to Stage 1.
5. **If the user says stop/opt-out in the middle** ("enough", "proceed", "done") → agent does a quick evaluation, documents assumptions for what's not yet answered, **still shows the summary + asks for final confirmation**, then moves to Stage 1.

### Important Notes for the Agent

- **DO NOT** ask via plain text — always use `ask_user` (except the opening message & summary).
- **DO NOT** ask cosmetic details that can be assumed (minor typography, spacing, icon library).
- **DO NOT** repeat questions that have already been answered.
- **MAY skip tiers** when the info in that tier is already clear from the initial request.
- **MAY re-batch** questions across tiers (e.g. 2 from Tier 2 + 2 from Tier 3 in one call) for efficiency.
- **ALWAYS** document assumptions in the PRD's **"Notes & Assumptions"** section when the user opts out of specific questions.
- **If the user says "just get it done" / "keep it simple"** — the interview still runs, but the agent compresses it into 1–2 batches with only the most critical questions (identity + MVP features + tech stack).
- **If the user provides a super-complete request up front** (e.g. *"build a PRD for a multi-outlet cafe POS, Next.js + Supabase, Figma already at link X, target 50 outlets within 6 months"*) — the agent may skip the interview, go straight to the assumptions summary + final confirmation, then generate.

### Sample Flow

**User request**: *"generate a PRD for a cafe POS app"*

**Agent — Opening**: interview opening message (template above).

**Agent — Batch 1** (Tier 1, 4 questions via `ask_user`):
- Q1: PRD output language? → English / Indonesian
- Q2: Target deployment? → Vercel / Self-host Docker / Cloud (AWS/GCP) / Don't know yet
- Q3: Auth method? → Email-password / OAuth (Google) / Enterprise SSO
- Q4: Figma/design system reference? → Yes (specify) / No (generate default)

*(user answers batch 1)*

**Agent — Evaluation**: Cafe POS context is already clear, but payment & receipt printer are still ambiguous → continue batch 2.

**Agent — Batch 2** (Tier 3, 3 questions via `ask_user`):
- Q1: Payment method? → Cash only / Cash + QRIS / Multi-payment (cash + QRIS + debit + e-wallet)
- Q2: Receipt printer integration? → Bluetooth thermal / Network printer / No printer
- Q3: Multi-outlet or single outlet? → Single outlet / Multi-outlet with consolidation / Multi-outlet independent (no consolidation)

*(user answers batch 2)*

**Agent — Evaluation**: No critical ambiguity remains → show assumptions summary + final confirmation.

**Agent — Summary**:
```
Confirmed assumptions:
- Cafe POS, single outlet, Vercel deployment, email-password auth
- Payment: cash + QRIS, Bluetooth receipt printer, default design system
- Output language: English

Generate with this info? (Yes / No, want to change something)
```

*(user answers Yes)*

**Agent → Stage 1**: Generate 3 files (PRD, TODO, Implementation Prompt) + a "Notes & Assumptions" section in the PRD listing all the assumptions above.

---
