# Panduan Lengkap: Lampiran B — Implementation Prompt Template

> **Dipanggil dari**: `SKILL.md`, Tahap 2.
> **Isi file ini**: Format file `[Nama-Proyek]-IMPLEMENTATION-PROMPT.md` yang siap-paste ke coding agent (Claude Code, Cursor, dll), termasuk variasi mode (Solo/Pair/Demo-First/Phase Skip).
> **Skip lampiran ini HANYA** jika proyek non-teknis (SOP, content strategy) atau user eksplisit minta tanpa prompt — lihat "Kapan TIDAK Perlu Implementation Prompt" di bawah.

---


Setiap output PRD WAJIB disertai dengan **Implementation Prompt** terpisah sebagai file markdown yang siap di-paste ke coding agent (Mavis, Claude Code, Cursor, Cody, atau engineer manusia) untuk mengeksekusi PRD + TODO. Lampiran ini **tidak boleh dilewati** — Implementation Prompt adalah "blueprint eksekusi" yang menurunkan PRD + TODO menjadi perintah siap-pakai.

### Mengapa File Ini Penting

- **Self-contained**: Agent eksekutor tidak punya konteks awal, jadi prompt harus menyertakan semua info penting (tech stack, prinsip kerja, urutan task).
- **Reusable**: Prompt yang sama bisa dipakai lintas fase (cukup update variabel `CURRENT_PHASE`).
- **Reduces hand-off friction**: Tidak perlu briefing ulang setiap pindah sprint / pindah agent.

### Format File Implementation Prompt

Simpan sebagai file terpisah: `[Nama-Proyek]-IMPLEMENTATION-PROMPT.md` di folder yang sama dengan PRD dan TODO. Gunakan struktur berikut secara presisi:

```markdown
# PROMPT IMPLEMENTASI — [Nama Proyek]

> **Cara pakai**: Copy seluruh isi bagian `--- PROMPT START` sampai `--- PROMPT END`, paste ke chat baru / coding agent sebagai pesan pertama.
> **File referensi wajib** (letakkan di folder kerja):
> - `[PRD-filename].md`
> - `[TODO-filename].md`
>
> **Update `CURRENT_PHASE`** di bawah ini setiap kali lanjut ke fase berikutnya.

---

## PROMPT START — COPY DARI SINI

\`\`\`
Kamu adalah **Senior Fullstack Engineer** yang ditugaskan untuk membangun **[Nama Proyek]** secara end-to-end.

## 📂 Konteks Proyek

Baca dan pahamai dulu dokumen referensi sebelum mulai coding:
1. `[PRD-filename].md` — Product Requirements Document
2. `[TODO-filename].md` — Task list per sprint (total [N] items)

Jangan skip baca — semua keputusan teknis harus sesuai PRD.

## 🎯 Misi

Implementasikan secara **inkremental per task**, mulai dari Task #1. Setelah setiap task selesai:
1. Commit dengan conventional commit (feat:, fix:, chore:, dll).
2. Update status di `[TODO-filename].md` (ubah \`- [ ]\` jadi \`- [x]\`).
3. Lapor progres singkat: apa yang selesai, blockers, next task.

**JANGAN loncat-loncat task** kecuali task sebelumnya sudah commit + verified jalan.

## 🛠 Tech Stack (LOCKED — jangan diganti)

| Layer | Teknologi |
| :--- | :--- |
| [layer 1] | [tech] |
| [layer 2] | [tech] |
| ... | ... |

## 🚦 CURRENT_PHASE

**[Nama Phase] — [Deskripsi]**

Tasks: #[X] sampai #[Y] dari \`[TODO-filename].md\` ([priority]).
Target: [estimasi].

**Update variabel ini saat pindah fase.**

## 📐 Prinsip Kerja (WAJIB DITAATI)

1. **[Prinsip 1]** — [Penjelasan singkat]
2. **[Prinsip 2]** — [Penjelasan singkat]
3. **[Prinsip 3]** — [Penjelasan singkat]
4. ... (5–10 prinsip tergantung kompleksitas)

## 🏗 Arsitektur File (jika Next.js / framework spesifik)

\`\`\`
[project-name]/
├── [folder tree singkat]
\`\`\`

## 📋 Task Execution Order

**[Sprint/Fase] — [Nama]**:
- #[N] [Task deskriptif + tech spesifik]
- #[N+1] [Task deskriptif + tech spesifik]
- ...

## 🎬 Eksekusi Sekarang

Mulai dari **Task #[X]**.

**Langkah konkret Task #[X]**:
\`\`\`bash
# 1. [Langkah 1]
[command]
# 2. [Langkah 2]
[command]
\`\`\`

Setelah Task #[X] selesai, update \`[TODO-filename].md\`:
\`\`\`diff
- [ ] **#[X]** [Task name] ...
+ [x] **#[X]** [Task name] ...
\`\`\`

Commit:
\`\`\`bash
git add -A
git commit -m "[type]: [message] (#[X])"
\`\`\`

**Lapor balik** dengan format:
\`\`\`
✅ Task #[X] selesai: [Nama task].
- File yang dibuat: [list]
- Commit: [hash]
- Next: Task #[X+1] — [Nama].
- Blockers: [none / atau jelaskan]
\`\`\`

## ⚠️ Hard Limits

- **JANGAN [larangan 1]**.
- **JANGAN [larangan 2]**.
- ... (5–8 hard limit yang critical)

## 📞 Komunikasi

Setiap selesai 1 task (atau kalau ada blocker > 30 menit), lapor balik. Kalau ada pertanyaan arsitektur yang tidak ada di PRD, tanyakan ke user sebelum ambil keputusan — jangan asumsi.

Mulai sekarang. Task pertama: **[Nama task pertama]**.
\`\`\`

## PROMPT END — COPY HINGGA SINI

---

## 💡 Tips Pemakaian

| Situasi | Yang harus dilakukan |
| :--- | :--- |
| Mulai sprint baru | Paste ulang prompt, update \`CURRENT_PHASE\`. |
| Lanjutin di tengah task | Paste prompt + tambahkan: "Lanjut dari Task #[X], status sebelumnya: [kondisi]". |
| Pindah mesin / fresh chat | Paste prompt + pastikan PRD + TODO ada di workspace. |
| Delegasi ke coder agent | Prompt sudah siap — langsung paste ke coder subagent. |

## 🔄 Variasi Prompt (Opsional)

### Variasi A — Solo Developer Mode
Tambahkan di awal prompt:
\`\`\`
CATATAN: Saya solo developer. Tasks yang di TODO "Split Stream" kerjakan
urut sesuai nomor, bukan paralel. Prioritaskan backend dulu.
\`\`\`

### Variasi B — Pair Programming Mode
\`\`\`
CATATAN: Saya akan review tiap commit. Setelah selesaikan 1 task, BERHENTI dan
tunggu approval saya sebelum lanjut task berikutnya. Jangan auto-continue.
\`\`\`

### Variasi C — Demo-First Mode (untuk pitch)
\`\`\`
CATATAN: Saya butuh demo dalam [X] minggu. Prioritaskan quick wins dulu:
1. [Quick win 1] — [Y] hari
2. [Quick win 2] — [Y] hari
Setelah itu baru lanjut sesuai urutan.
\`\`\`

### Variasi D — Phase Skip
Untuk lanjut ke fase berikutnya, ganti \`CURRENT_PHASE\` di prompt:
\`\`\`
**Phase [N] — [Nama Phase]**

Tasks: #[X] sampai #[Y] (priority).
Asumsi: Phase sebelumnya sudah selesai dan stabil.
\`\`\`
```

### Aturan Wajib untuk Implementation Prompt

1. **Wajib di dalam blok `\`\`\``** (code block) dengan marker `--- PROMPT START` / `--- PROMPT END` agar mudah di-copy.
2. **Wajib self-contained**: agent eksekutor TIDAK boleh perlu konteks tambahan di luar PRD + TODO + prompt ini. Semua info (tech stack, prinsip, urutan) harus ada di dalam prompt.
3. **Wajib ada `CURRENT_PHASE`**: variabel yang bisa di-swap saat pindah fase tanpa rewrite prompt dari awal.
4. **Wajib ada langkah konkret Task pertama**: bash command + diff + commit template untuk task #1 atau task pertama di phase aktif.
5. **Wajib ada format laporan**: standar komunikasi setelah setiap task (selesai / blockers / next).
6. **Wajib ada Hard Limits**: 5–8 larangan keras yang agent TIDAK boleh langgar (security, RLS, no hardcode, dll).
7. **Wajib ada minimal 2 variasi prompt**: Solo Mode + minimal 1 lagi (Pair / Demo-First / Phase Skip).
8. **Bahasa**: Sama dengan bahasa PRD dan TODO (default: Bahasa Indonesia). Command code tetap dalam bahasa Inggris.
9. **Bahasa prompt di dalam code block**: WAJIB menggunakan Bahasa Indonesia yang konsisten dengan PRD + TODO. Code identifier, path, dan CLI command tetap dalam bahasa Inggris.
10. **Penomoran task harus sinkron dengan TODO**: nomor task di prompt = nomor task di TODO file.

### Kapan TIDAK Perlu Implementation Prompt

- PRD untuk produk **non-teknis** (misal: proses bisnis, SOP, content strategy) — tidak ada yang akan di-code.
- PRD untuk **dokumen referensi** saja, bukan untuk dieksekusi.
- User secara eksplisit minta **hanya PRD** atau **hanya TODO**.

Tanya user konfirmasi jika ragu apakah prompt diperlukan.

