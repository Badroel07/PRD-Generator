---
name: prd-generator
description: Generate comprehensive Product Requirements Documents (PRD) following a standardized 7-section structure (Overview, Requirements, Core Features, User Flow, Architecture, Database Schema, Design & Technical Constraints) PLUS three companion deliverables — a sprint-based TODO List, a self-contained Implementation Prompt for coding agents, and a UI/UX Reference Prompt for design specs. BEFORE generating, runs a mandatory Pre-Planning Interview using the available structured question tool to resolve ambiguities (missing UI reference, incomplete feature list, unspecified tech stack, undefined roles, unclear business rules) — loops with no fixed round limit until planning is unambiguous or the user opts out ("skip interview" / "langsung generate"). Trigger on /prd, /prd-generator, /buat-prd, /generate-prd, or any request for a PRD, product requirements document, detailed product specs, implementation prompt, UI/UX prompt, or design reference.
---

# PRD Generator Skill

Skill ini memandu AI Agent untuk membuat **Product Requirements Document (PRD)** yang sangat terstruktur, profesional, dan akurat dalam Bahasa Indonesia, plus 3 file pendamping siap-eksekusi.

**File ini adalah peta jalan (routing layer).** Detail lengkap tiap tahap ada di folder `references/` — baca file yang relevan tepat sebelum kamu butuh, jangan generate dari ingatan/tebakan.

```
prd-generator/
├── SKILL.md                                    (kamu di sini)
└── references/
    ├── interview-guide.md                      (Tahap 0 — tier pertanyaan lengkap)
    ├── prd-format.md                            (Tahap 1 — template 7-bagian PRD)
    ├── todo-template.md                         (Tahap 2 — Lampiran A)
    ├── implementation-prompt-template.md        (Tahap 2 — Lampiran B)
    └── uiux-prompt-template.md                  (Tahap 2 — Lampiran C)
```

---

## Slash Commands & Trigger

Skill ini otomatis dipicu ketika user mengetik perintah slash atau kata kunci berikut:
- `/prd`, `/prd-generator`, `/buat-prd`, `/generate-prd`
- Frasa seperti *"buatkan PRD"*, *"buat PRD"*, *"generate PRD"*, *"PRD document"*, *"implementation prompt"*, *"prompt implementasi"*, *"UI/UX prompt"*, *"design prompt"*, *"prompt desain"*, dll.

---

## 📦 Output Deliverables (4 File Wajib)

Setiap kali skill ini dipicu, agent WAJIB menghasilkan **4 file markdown** di folder yang sama:

| File | Isi | Wajib? |
| :--- | :--- | :--- |
| `[Nama-Proyek]-PRD.md` | Product Requirements Document (7 section) | ✅ Selalu |
| `[Nama-Proyek]-TODO.md` | Task list sprint-based dengan checkbox | ✅ Selalu |
| `[Nama-Proyek]-IMPLEMENTATION-PROMPT.md` | Prompt siap-paste ke coding agent | ✅ Untuk proyek teknis |
| `[Nama-Proyek]-UIUX-PROMPT.md` | Prompt referensi UI/UX untuk design spec | ✅ Untuk proyek dengan UI |

Keempat file **SINKRON** — nomor task di TODO harus sama dengan referensi di Implementation Prompt dan UI/UX Prompt.

**Pengecualian**:
- **Implementation Prompt**: Lewati hanya jika proyek non-teknis (proses bisnis, SOP, content strategy) atau user minta tanpa prompt.
- **UI/UX Prompt**: Lewati hanya jika proyek **tanpa UI** (API service, CLI tool, backend-only, data pipeline) atau user minta tanpa prompt UI/UX.

---

## 🚦 Tahap 0: Pre-Planning Interview (WAJIB SEBELUM GENERATE)

> 📖 **Baca `references/interview-guide.md` sebelum mulai** — berisi daftar lengkap pertanyaan per tier (1–4), template pesan pembuka, dan contoh flow end-to-end.

Sebelum mulai nulis PRD, agent WAJIB menjalani **Pre-Planning Interview** untuk ngilangin ambiguitas. Interview loop terus (tidak ada batas jumlah *ronde*) sampai salah satu dari 2 kondisi berikut terpenuhi:

1. **Tidak ada lagi ambiguitas kritis** yang tersisa → lanjut ke Tahap 1.
2. **User explicit opt-out** (*"skip interview"*, *"cukup"*, *"langsung generate"*, *"pakai default saja"*) → dokumentasikan semua asumsi di section **"Catatan & Asumsi"** PRD, lalu lanjut ke Tahap 1.

> **Hard rule**: Agent **TIDAK BOLEH** berhenti hanya karena info yang dikumpulkan "sebagian besar sudah ada". Bertanya terus lebih baik daripada PRD yang isinya banyak asumsi keliru.

### Apa yang dianggap "ambigu" (wajib diklarifikasi)?

Ketiadaan info berikut akan memaksa agent mengambil keputusan yang **materially mengubah** scope/tech stack/architecture/feature set:

- Nama produk/aplikasi · Domain industri · Platform target (web/mobile/desktop/CLI/API) · User & role/permission
- List fitur MVP · Referensi UI/UX (Figma, mockup, brand kit) · Tech stack krusial · Workflow/business rule khusus
- Integrasi eksternal · Deployment target · Compliance/security (kalau industri mengharuskan) · Bahasa output PRD · Auth method

Yang **TIDAK** perlu ditanya (boleh pakai default): typography minor, icon library, sprint timeline detail, naming convention, state management library, test framework — semua ini agent putuskan sendiri sesuai best practice framework.

### Cara Bertanya

- **WAJIB pakai structured question tool** yang tersedia di environment (mis. `ask_user_input_v0`) — **BUKAN** tanya di plain text chat.
- **Cek dulu skema/batas tool tersebut** sebelum generate batch pertanyaan (jumlah pertanyaan per call & jumlah opsi per pertanyaan bisa beda-beda tergantung tool/environment — jangan asumsikan angka tetap).
- 2–4 opsi konkret per pertanyaan, mutually exclusive, level abstraksi sama.
- Kalau tool tidak punya tombol opsi bebas ("Lainnya"), user tetap bisa ngetik jawaban custom di pesan balasan — agent WAJIB terima itu sebagai jawaban valid.
- **Loop sampai clear**: tiap batch jawaban dievaluasi ulang. Kalau masih ada ambiguitas kritis → batch berikutnya.
- **JANGAN** ulangi pertanyaan yang sudah terjawab, dan **JANGAN** tanya detail kosmetik.
- Bertanya **per tier** (Tier 1 → 4), boleh skip tier kalau sudah jelas, boleh re-batch lintas tier untuk efisiensi.

### Sebelum generate: tampilkan ringkasan + konfirmasi final

Begitu tidak ada ambiguitas kritis tersisa (atau user opt-out), tampilkan **ringkasan asumsi terkonfirmasi** dan tanya 1 pertanyaan terakhir (binary): *"Lanjut generate dengan info ini?"* — baru masuk ke Tahap 1.

---

## 🧱 Tahap 1: Generate PRD (7 Bagian)

> 📖 **Baca `references/prd-format.md` sebelum generate** — berisi template markdown presisi, contoh Mermaid diagram (`graph TD` untuk Architecture, `erDiagram` untuk Database Schema), dan aturan Typography.

Setiap PRD HARUS punya 7 bagian ini, lengkap dan terisi detail — jangan mengurangi atau melompati bagian manapun:

1. **Overview** — latar belakang masalah + tujuan utama aplikasi
2. **Requirements** — aksesibilitas, pengguna, data input, notifikasi
3. **Core Features** — daftar fitur MVP
4. **User Flow** — langkah demi langkah alur kerja pengguna
5. **Architecture** — diagram Mermaid `graph TD` + deskripsi komponen
6. **Database Schema** — diagram Mermaid `erDiagram` + ringkasan tabel
7. **Design & Technical Constraints** — tech stack, typography rules, UI/layout rules

Sesuaikan isi tiap bagian dengan konteks proyek user (nama produk, domain bisnis), tapi **tetap pertahankan struktur 7 bagian** persis seperti di `prd-format.md`.

---

## 🧩 Tahap 2: Generate 3 File Pendamping

| Lampiran | File output | Reference |
| :--- | :--- | :--- |
| A — TODO List | `[Nama-Proyek]-TODO.md` | `references/todo-template.md` |
| B — Implementation Prompt | `[Nama-Proyek]-IMPLEMENTATION-PROMPT.md` | `references/implementation-prompt-template.md` |
| C — UI/UX Reference Prompt | `[Nama-Proyek]-UIUX-PROMPT.md` | `references/uiux-prompt-template.md` |

Baca file reference yang sesuai **tepat sebelum generate lampiran itu** — masing-masing punya format presisi, aturan wajib, dan kapan boleh di-skip (lihat section "Kapan TIDAK Perlu..." di tiap file).

**Konsistensi penomoran wajib**: Nomor task di TODO = nomor task yang dirujuk di Implementation Prompt = nomor task UI di UI/UX Prompt. Sinkronkan sebelum output final.

---

## Petunjuk Penggunaan untuk Agent

0. **🚦 WAJIB Tahap 0**: Jangan generate apa pun sebelum interview selesai/di-opt-out (lihat `references/interview-guide.md`). Kecuali user sudah kasih info super lengkap di request awal — dalam hal itu boleh skip langsung ke ringkasan asumsi + konfirmasi final.
1. **Fleksibilitas Input**: Sesuaikan isi tiap bagian dengan detail spesifik proyek user, tapi tetap pertahankan 7 bagian PRD.
2. **Kelengkapan**: Semua 7 bagian harus selalu ada dan terisi detail — jangan skip bagian manapun.
3. **Mermaid Diagrams**: Gunakan sintaks Mermaid yang sah untuk Architecture (`graph TD`) dan Database Schema (`erDiagram`).
4. **Typography Strictness**: Sertakan aturan Typography persis seperti spesifikasi di `references/prd-format.md`.
5. **Generate 3 Companion Files (WAJIB)**: Setelah PRD selesai, selalu generate `[Nama-Proyek]-TODO.md`, `[Nama-Proyek]-IMPLEMENTATION-PROMPT.md`, dan `[Nama-Proyek]-UIUX-PROMPT.md` di folder yang sama — ikuti masing-masing reference file.
6. **Output Ringkasan**: Di akhir output, tampilkan jumlah item TODO + distribusi HIGH/MEDIUM/LOW, jumlah task di Implementation Prompt, jumlah task UI di UI/UX Prompt, dan catatan variasi yang tersedia di kedua prompt.
7. **Pengecualian Implementation Prompt**: Lewati Lampiran B HANYA jika proyek non-teknis ATAU user eksplisit minta tanpa prompt. Tanya konfirmasi kalau ragu.
8. **Pengecualian UI/UX Prompt**: Lewati Lampiran C HANYA jika proyek tanpa UI ATAU user eksplisit minta tanpa design spec. Tanya konfirmasi kalau ragu.
9. **Konsistensi Penomoran**: Sinkronkan nomor task lintas TODO / Implementation Prompt / UI/UX Prompt sebelum output final.

---

## 📝 Riwayat Perubahan

- **v1.1**: Direstrukturisasi jadi progressive disclosure (SKILL.md ringkas + `references/`) supaya lebih hemat context saat trigger. Perbaikan 2 bug karakter non-Indonesia yang nyelip (di contoh Tier 3 interview dan intro Lampiran C). Batas jumlah pertanyaan per call disesuaikan ke skema tool tanya-user yang sebenarnya tersedia (cek dulu sebelum asumsi angka tetap).
- **v1.0**: Versi awal, 1 file monolitik (~1000 baris).
