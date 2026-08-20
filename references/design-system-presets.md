# Complete Guide: Design System Presets (fallback when the user has no reference)

> **Called from**: `references/interview-guide.md`, sub-flow "UI/UX Reference" in Tier 3.
> **Contents of this file**: 4 ready-to-use design system presets (typography, color token, radius, shadow, best-for), used as selectable options via the user-question tool when the user has no design reference of their own.
> **Read this file BEFORE** showing preset options to the user or filling PRD Section 7 from the selected preset.

---

## When this file is used

In the Tier 3 interview, the agent asks whether the user has a design reference (Figma, mockup, brand kit, or an inspiration app). There are 2 branches:

- **User has a reference** → the user explains freely in chat (link, description, app name, screenshot). The agent extracts/infers tokens from that. **The presets in this file are NOT used** — skip directly to filling PRD Section 7 from the user's reference, and note in "Notes & Assumptions" which parts the agent inferred.
- **User has no reference** → the agent shows the **4 presets below** via the user-question tool (single_select, 4 options — fits the tool's limit), and the user picks one. The selected preset is mapped directly into PRD Section 7.

These presets are pure sensible defaults — they are NOT real brand research, so still let the user know they can request color/font changes at any time after the PRD is done.

---

## How to show the options to the user

Use the user-question tool with a question like:

> *"No design reference yet? I can suggest a few ready-to-use styles — which one best matches your product's vibe?"*

Options (short labels, because the tool usually only renders short labels — the full details are in the table below; the agent explains them in chat before/after the tool is called):

1. **Modern Minimalist** — clean, neutral, great for SaaS/dashboards
2. **Bold & Friendly** — bright colors, rounded, great for consumer/startup apps
3. **Corporate & Formal** — conservative, info-dense, great for enterprise/internal tools
4. **Dark Tech** — dark-mode-first, neon accents, great for developer tools/tech products

---

## 1. Modern Minimalist

**Best for**: SaaS, dashboards, productivity tools, admin panels.

| Token | Value |
| :--- | :--- |
| Typography (sans) | `Inter` or `Geist Sans` |
| Typography (mono, for numbers/code) | `JetBrains Mono` |
| Primary | `#18181B` (near-black) |
| Primary Foreground | `#FAFAFA` |
| Accent | `#3B82F6` (blue) |
| Success / Warning / Danger | `#10B981` / `#F59E0B` / `#EF4444` |
| Background / Muted | `#FFFFFF` / `#F4F4F5` |
| Radius | `rounded-md` (6px) for input/button, `rounded-lg` (8px) for card |
| Shadow | Subtle — `shadow-sm` for button/input, `shadow-md` for dropdown |
| Visual character | Generous whitespace, only 1 accent color, hierarchy through size & weight rather than color |

## 2. Bold & Friendly

**Best for**: Consumer apps, startups, products aimed at general/non-technical users.

| Token | Value |
| :--- | :--- |
| Typography (sans) | `Plus Jakarta Sans` or `Poppins` |
| Typography (mono) | `JetBrains Mono` (when precise numbers are needed) |
| Primary | `#7C3AED` (purple) or adjust to brand |
| Primary Foreground | `#FFFFFF` |
| Accent | `#F59E0B` (warm orange) |
| Success / Warning / Danger | `#22C55E` / `#F59E0B` / `#EF4444` |
| Background / Muted | `#FFFFFF` / `#F3F0FF` (tint of primary) |
| Radius | `rounded-xl` (12px) for almost everything, `rounded-full` for badges/avatars/pill buttons |
| Shadow | More present — `shadow-md` as default, `shadow-lg` for highlight cards |
| Visual character | Bold but only 1 primary + 1 accent, illustrations/emojis are fine, consistent rounded corners |

## 3. Corporate & Formal

**Best for**: Enterprise software, internal corporate tools, financial/legal/government applications.

| Token | Value |
| :--- | :--- |
| Typography (sans) | `IBM Plex Sans` or `Inter` |
| Typography (mono) | `IBM Plex Mono` |
| Primary | `#1E3A5F` (navy) |
| Primary Foreground | `#FFFFFF` |
| Accent | `#64748B` (slate, used minimally) |
| Success / Warning / Danger | `#15803D` / `#B45309` / `#B91C1C` (darker/muted versions of the usual) |
| Background / Muted | `#FFFFFF` / `#F1F5F9` |
| Radius | `rounded-sm` (4px) — sharper corners, formal feel |
| Shadow | Minimal — borders are preferred over shadows |
| Visual character | High information density (tables, long forms), very conservative color use, consistent and conservative typography |

## 4. Dark Tech

**Best for**: Developer tools, monitoring/observability dashboards, technical products for a technical audience.

| Token | Value |
| :--- | :--- |
| Typography (sans) | `Geist Sans` |
| Typography (mono, dominant) | `JetBrains Mono` or `Fira Code` |
| Primary (base background) | `#0A0A0B` (near-black) |
| Primary Foreground | `#E4E4E7` |
| Accent | `#22D3EE` (cyan) or `#A3E635` (lime) |
| Success / Warning / Danger | `#4ADE80` / `#FACC15` / `#F87171` (neon/bright versions for contrast on dark bg) |
| Background / Muted | `#0A0A0B` / `#18181B` |
| Radius | `rounded-md` (6px), consistent and not too rounded |
| Shadow | Replace with **subtle border glow** (`border` + `ring`) because shadows are less visible on dark mode |
| Visual character | Dark mode is the default (not an optional toggle), monospace used widely for numbers/logs/code, neon accents used very selectively (CTA, active status) |

---

## After the user picks a preset

1. Mention the chosen preset in the confirmation summary before Stage 1 (*"Okay, this PRD will use the [Preset Name] style"*).
2. Copy the tokens from the table above directly into PRD Section 7 (Design & Technical Constraints) — do not paraphrase into vague terms, keep the exact hex/font values.
3. Note in "Notes & Assumptions": *"Design system selected from the [Preset Name] preset because the user has no design reference of their own. The user can request color/font changes at any time."*
4. If the Implementation Prompt (Appendix B) later mentions styling, refer back to the same tokens in PRD Section 7 — do not create new, different tokens.
