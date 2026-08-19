# PRD Generator Skill

> Generate comprehensive Product Requirements Documents (PRD) with a structured 7-section format, plus companion TODO list and Implementation Prompt.

## What it does

This skill produces **3 synchronized markdown files** from a brief product idea:

| File | Purpose |
| :--- | :--- |
| `[Nama-Proyek]-PRD.md` | The main 7-section PRD (Overview, Requirements, Core Features, User Flow, Architecture, Database Schema, Design & Technical Constraints) |
| `[Nama-Proyek]-TODO.md` | Sprint-based task list with checkboxes (15–30 items across 4 phases) |
| `[Nama-Proyek]-IMPLEMENTATION-PROMPT.md` | Self-contained prompt for coding agents (Mavis, Claude Code, Cursor) to execute the PRD |

## Pre-Planning Interview (mandatory)

Before generating, the skill runs a **Pre-Planning Interview** to resolve planning ambiguities — missing UI reference, incomplete feature list, unspecified tech stack, undefined user roles, unclear business rules, etc. The interview uses structured question prompts (`ask_user`) and keeps looping until planning is unambiguous or the user opts out. **No question limit.**

See the `Tahap 0` section in `SKILL.md` for full specification.

## Trigger

The skill activates when you type any of:
- `/prd` · `/prd-generator` · `/buat-prd` · `/generate-prd`
- "buatkan PRD" · "generate PRD" · "PRD document"
- "implementation prompt" · "todo list" · "task list"

## Language

Output in Bahasa Indonesia by default. Code identifiers, paths, and CLI commands stay in English.

## Usage

In a Mavis / Claude Code session, just type `/prd` followed by a brief description of what you want to build. The skill will:

1. Run the Pre-Planning Interview (1–4 questions per batch, looping until clear)
2. Generate the 3 markdown files
3. Show a summary of assumptions + sprint distribution

## File map

```
prd-generator/
├── SKILL.md    # Full skill specification (read by the agent)
└── README.md   # This file
```

## Source

Maintained as a Mavis skill in `~/.agents/skills/prd-generator/`.
