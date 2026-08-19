# Panduan Lengkap: Lampiran A — TODO List Template

> **Dipanggil dari**: `SKILL.md`, Tahap 2 (WAJIB — tidak boleh dilewati).
> **Isi file ini**: Format file `[Nama-Proyek]-TODO.md`, panduan prioritas (HIGH/MEDIUM/LOW), panduan granularitas item, dan aturan wajib.
> **Baca file ini SEBELUM generate TODO list.**

---

## Lampiran A: TODO List Implementation (WAJIB DIBUAT)

Setiap output PRD HARUS disertai dengan **TODO List** terpisah sebagai file markdown yang siap dieksekusi untuk tim developer. Lampiran ini **tidak boleh dilewati** — TODO List adalah action item konkrit yang menurunkan PRD menjadi task-task yang bisa di-checklist.

### Format File TODO List

Simpan sebagai file terpisah: `[Nama-Proyek]-TODO.md` di folder yang sama dengan PRD. Gunakan struktur berikut secara presisi:

```markdown
# TODO — [Nama Proyek dari PRD]

> **Source**: PRD v1.0 — `[nama-file-prd].md`
> **Total**: [N] items ([N1] HIGH · [N2] MEDIUM · [N3] LOW)
> **Estimasi MVP**: [X–Y] minggu ([tim size])

---

## 🎯 Sprint 1 — Phase 1: MVP (HIGH Priority)
> Target: [X–Y] minggu. Wajib selesai sebelum lanjut ke Phase 2.

- [ ] **#1** [Task deskriptif + tech spesifik]
- [ ] **#2** [Task deskriptif + tech spesifik]
- [ ] **#3** [Task deskriptif + tech spesifik]

---

## 🚀 Sprint 2 — Phase 2: [Nama Phase] (MEDIUM Priority)
> Target: [X–Y] minggu setelah MVP stabil.

- [ ] **#N** [Task deskriptif]

---

## 💎 Sprint 3 — Phase 3: [Nama Phase] (MEDIUM Priority)
> Target: [X–Y] minggu. Fokus ke [analitik / retensi / dll].

- [ ] **#N** [Task deskriptif]

---

## 🛠 Sprint 4 — Phase 4: Polish & Enterprise (LOW Priority)
> Target: [X–Y] minggu. Nice-to-have, bisa di-skip kalau timeline mepet.

- [ ] **#N** [Task deskriptif]

---

## 👥 Saran Split Kerja (N Orang)

### Pondasi (Minggu 1–2) — Kerjain Bareng
[Tabel item pondasi yang WAJIB dishare antar anggota]

### Setelah Pondasi Ready — Split Stream
[Tabel distribusi Stream A dan Stream B per anggota tim]

---

## 🏃 Quick Wins (Biar Keliatan Progress Cepet)
1. ⏱ [Task 1–2 hari yang impactful]
2. 🌱 [Task visual early]
3. 🧾 [Task demo-friendly]
4. 📊 [Task visual chart / dashboard]

---

## 📌 Catatan Penting
- [Catatan kritis tentang non-obvious decisions]
- [Item yang sering di-defer tapi harus dipertimbangkan]
- [Trade-off teknis yang perlu diingat]
```

### Panduan Prioritas Item

| Prioritas | Kapan Pakai | Contoh |
|---|---|---|
| **HIGH (MVP)** | Fitur wajib agar produk bisa digunakan pertama kali | Setup foundation, Auth, Core CRUD, Core transaction flow |
| **MEDIUM** | Meningkatkan value tapi produk tetap jalan tanpa-nya | Stock mgmt, Multi-payment, CRM, Reporting lanjutan |
| **LOW (Polish)** | Nice-to-have, optimasi, enterprise-grade | PWA offline, Audit log, Self-host Docker, Super-admin |

### Panduan Granularitas & Jumlah Item

- **1 item = 1 task** yang bisa selesai dalam 0.5–2 hari oleh 1 developer.
- **Total ideal**: 15–30 item. Terlalu sedikit = kurang actionable. Terlalu banyak = overwhelming.
- **Sprint 1 (HIGH)**: 30–50% dari total items.
- **Grouping**: Item-item yang saling bergantung (misal "Setup Drizzle" + "Initial migration" + "Seed data") WAJIB digabung jadi 1 item besar, atau di-nomor berurutan `#2`, `#3` agar jelas urutannya.

### Aturan Wajib untuk TODO List

1. **Wajib gunakan checkbox markdown** `- [ ]` (kompatibel dengan GitHub Projects, Notion, Obsidian, VS Code).
2. **Wajib ada nomor urut** `**#N**` di tiap item untuk referensi lintas dokumen (misal "Lihat TODO #7").
3. **Wajib ada estimasi sprint** di tiap header sprint.
4. **WAJIB sertakan saran split kerja** jika tim size disebutkan user (default: asumsikan 1–2 orang).
5. **WAJIB sertakan Quick Wins section** — 3–5 task early yang impact visual / demo-nya besar.
6. **WAJIB gunakan emoji header** untuk section: 🎯 (MVP), 🚀 (Phase 2), 💎 (Phase 3), 🛠 (Phase 4), 👥 (tim), 🏃 (quick wins), 📌 (catatan).
7. **Bahasa**: Sama dengan bahasa PRD (default: Bahasa Indonesia).

