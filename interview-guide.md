# Panduan Lengkap: Tahap 0 — Pre-Planning Interview

> **Dipanggil dari**: `SKILL.md`, Tahap 0.
> **Isi file ini**: Daftar lengkap pertanyaan per tier (1–4), aturan pakai tool tanya-user, loop & termination logic, dan contoh flow end-to-end.
> **Baca file ini SEBELUM mulai interview** — jangan generate pertanyaan dari ingatan/tebakan.

---

## 🚦 Tahap 0: Pre-Planning Interview — Resolusi Ambiguitas (WAJIB SEBELUM GENERATE)

Sebelum agent mulai nulis PRD, agent WAJIB menjalani **Pre-Planning Interview** untuk ngilangin ambiguitas. **TIDAK ADA BATAS JUMLAH PERTANYAAN** — agent bertanya terus (loop) sampai **planning sudah tidak ambigu** atau user explicit opt-out.

### Kapan Interview Berhenti (Hanya 2 Kondisi)

1. **Tidak ada lagi ambiguitas kritis** yang tersisa → agent masuk ke Tahap 1 (generate file PRD).
2. **User explicit opt-out** dengan kata kunci seperti *"skip interview"*, *"cukup"*, *"langsung generate"*, *"pakai default saja"*, *"udah cukup"*, *"lanjut"* → agent dokumentasikan semua asumsi di section **"Catatan & Asumsi"** PRD, lalu masuk ke Tahap 1.

> **Hard rule**: Agent **TIDAK BOLEH** berhenti hanya karena info yang dikumpulkan "sebagian besar sudah ada". Loop terus sampai planning clear atau user stop. Bertanya terus = lebih bagus daripada PRD yang isinya banyak asumsi keliru.

### Definisi: Apa yang "Ambigu"?

Input user disebut **ambigu** (wajib diklarifikasi) jika ketiadaannya akan memaksa agent mengambil keputusan yang **materially mengubah** scope, tech stack, architecture, atau feature set PRD. Contoh ambigu:

- ❌ Tidak ada **nama produk/aplikasi**
- ❌ Tidak ada **domain industri** (retail, F&B, SaaS B2B, edukasi, kesehatan, logistik, dll)
- ❌ Tidak ada info **platform target** (web app / mobile native / mobile web / desktop / CLI / API-SDK / IoT)
- ❌ Tidak ada info **user/role/permission**
- ❌ Tidak ada list **fitur MVP** (cuma "buat aplikasi X" tanpa spek fitur)
- ❌ Tidak ada **referensi UI/UX** (Figma, mockup, brand kit, design system, contoh app)
- ❌ **Tech stack** tidak disebut padahal krusial untuk pilihan implementation
- ❌ **Workflow / business rule** belum jelas (FIFO/LIFO stok, approval multi-step, hitung ulang, dll)
- ❌ Tidak ada info **integrasi eksternal** (payment gateway, SSO, third-party API, email service, cloud storage)
- ❌ Tidak ada info **deployment target** (Vercel / self-host Docker / on-premise / specific cloud)
- ❌ **Compliance/security** tidak jelas padahal industri mengharuskan (GDPR, HIPAA, PCI-DSS)
- ❌ **Bahasa output PRD** tidak disebut (Indonesia / English)
- ❌ **Auth method** tidak jelas untuk proyek dengan login

Yang **TIDAK ambigu** (agent boleh pakai default tanpa nanya):

- ✅ Detail typography minor (font size, line-height) → pakai default Section 7
- ✅ Icon library (lucide-react default)
- ✅ Sprint timeline detail → agent estimasi sendiri berdasarkan kompleksitas
- ✅ Quick wins → agent tentukan berdasarkan best practice
- ✅ Naming convention file/folder → agent pilih yang idiomatik per framework
- ✅ State management library per framework → pakai default komunitas
- ✅ Test framework per bahasa → default (Jest/Vitest untuk JS, pytest untuk Python, dll)

### Cara Bertanya: WAJIB Pakai `ask_user`

Agent **WAJIB** pakai structured question tool `ask_user` — **BUKAN** tanya di plain text chat. Aturan main:

- **Maksimal 3 pertanyaan per call, maksimal 4 opsi per pertanyaan** — ini batas tool tanya-user Claude (`ask_user_input_v0`) per Agustus 2026. **Cek dulu skema tool yang tersedia di environment sebelum generate batch** — kalau beda platform/tool, batasnya bisa berbeda.
- **2–4 opsi konkret per pertanyaan**, mutually exclusive, level abstraksi sama.
- **Tidak ada tombol "Lainnya" bawaan** di tool ini — kalau user mau jawab di luar opsi yang ditawarkan, mereka cukup ketik jawaban bebas di pesan balasan berikutnya. Agent WAJIB menerima dan memproses jawaban bebas tersebut sebagai valid, jangan maksa user pilih salah satu tombol.
- **Batch pintar**: re-batch lintas tier kalau lebih efisien (misal 2 dari Tier 2 + 2 dari Tier 3 dalam 1 call).
- **Loop sampai clear**: setelah setiap batch jawaban, evaluasi lagi. Kalau masih ada ambiguitas kritis, batch berikutnya → tanya lagi → evaluasi → dst.
- **JANGAN ulangi pertanyaan** yang sudah terjawab (cek history chat dulu).
- **JANGAN tanya detail kosmetik** yang bisa diasumsikan dari PRD Section 7.

### Tier Pertanyaan (Bertahap, Bukan Bombarding)

Agent bertanya **per tier**, mulai dari Tier 1, dan **BOLEH skip tier** kalau info sudah jelas dari request awal atau jawaban tier sebelumnya.

#### Tier 1 — Identitas & Lingkup (WAJIB, kecuali sudah jelas di request)

1. **Nama produk/aplikasi** — kalau user belum kasih.
2. **Domain industri** — retail, F&B, SaaS B2B/B2C, edukasi, kesehatan, logistik, finance, manufaktur, dll.
3. **Platform target** — web app, mobile native (iOS/Android), mobile web/PWA, desktop (Electron/Tauri), CLI, API/SDK, embedded/IoT, atau kombinasi.
4. **Target pengguna & role** — siapa primary user, role apa saja, apakah perlu RBAC multi-level.
5. **Apakah sudah ada PRD/dokumen referensi sebelumnya** — untuk konsistensi (jika ya, minta lokasi file).
6. **Bahasa output PRD** — Bahasa Indonesia atau English (default: Indonesia, sesuai bahasa user).

#### Tier 2 — Fitur & Workflow (Ditanya kalau fitur user masih vague)

1. **Fitur MVP wajib** — list fitur inti (1–3 kalimat per fitur), atau pilih preset by domain.
2. **Yang TIDAK masuk MVP** — out-of-scope eksplisit (out-of-scope = tidak dibangun di fase awal).
3. **Workflow / business rule khusus** — approval flow, FIFO/LIFO, auto-numbering, multi-step form, validasi spesifik, dll.
4. **Entitas data utama** — list entity (User, Product, Order, dll) atau preset by domain.
5. **Reporting / analytics** — laporan apa saja yang harus ada di MVP (penjualan, stok, keuangan, dll).

#### Tier 3 — Tech & Design (Ditanya kalau belum disebut & krusial)

1. **Tech stack preference** — framework, bahasa, database, ORM (atau *"pilih yang terbaik untuk use case"* → agent putuskan).
2. **UI/UX reference** — apakah ada Figma/mockup/brand kit/design system existing? (jika tidak, agent generate design system default).
3. **Color/typography preference** — warna brand, font khusus, atau *"pakai default profesional"*.
4. **Auth method** — email/password, OAuth (Google/GitHub/Apple), magic link, SSO enterprise, atau kombinasi.
5. **Integrasi eksternal** — payment gateway, email service, third-party API, cloud storage, CDN, search engine.
6. **Deployment target** — Vercel/Netlify, AWS/GCP/Azure, self-host Docker, on-premise, atau *"belum tahu"*.

#### Tier 4 — Non-Functional (Ditanya HANYA jika material terhadap scope)

1. **Performance / scale** — expected user count, data volume, peak load, response time target.
2. **Compliance / security** — GDPR, HIPAA, PCI-DSS, ISO 27001, SOC 2, atau standar lokal (UU PDP Indonesia, dll).
3. **Multi-tenancy** — apakah multi-tenant (satu aplikasi untuk banyak organisasi) atau single-org.
4. **i18n / multi-bahasa** — apakah perlu support multiple locale (i18n) atau single language cukup.
5. **Notification channels** — email, push notification, in-app, SMS, WhatsApp, webhook.
6. **File upload / media handling** — perlu upload file? format apa? ukuran max? (gambar, dokumen, video).
7. **Offline / PWA** — apakah perlu jalan tanpa internet (PWA, sync queue).
8. **Audit log** — apakah perlu track semua perubahan data untuk compliance/debug.

### Template Pesan Pembuka Interview

Saat masuk mode interview, agent WAJIB buka dengan pesan (atau setara) untuk set expectation:

```
Oke, sebelum gua generate PRD-nya, gua perlu klarifikasi beberapa hal dulu
biar hasilnya tepat sasaran dan ga ada asumsi yang meleset.

Gua bakal tanya pakai question card — jawab seadanya, atau pilih opsi
yang paling cocok. Kalau lo udah ada jawabannya, langsung ketik via
opsi "Lainnya".

Kalau lo udah males dan mau skip interview, bilang aja "skip interview"
atau "langsung generate dengan default" — gua bakal catat semua asumsi
yang gua pakai di section "Catatan & Asumsi" PRD.

Mulai dari pertanyaan pertama...
```

### Loop & Termination Logic

Setelah setiap batch `ask_user` dijawab, agent jalankan loop ini:

1. **Parse jawaban** user dari semua pertanyaan di batch.
2. **Cek ambiguitas** yang masih tersisa (referensi "Definisi: Apa yang Ambigu?" di atas).
3. **Jika masih ada ambiguitas kritis** → batch pertanyaan berikutnya (Tier selanjutnya atau pertanyaan yang ter-skip) → tanya lagi via `ask_user` → goto step 1.
4. **Jika tidak ada lagi ambiguitas kritis** → tampilkan **ringkasan asumsi terkonfirmasi** ke user, tanya konfirmasi final: *"Lanjut generate dengan info ini?"* (1 pertanyaan terakhir, binary) → masuk ke Tahap 1.
5. **Jika user bilang stop/opt-out di tengah** ("cukup", "lanjut", "udah") → agent evaluasi cepat, dokumentasikan asumsi untuk yang belum terjawab, **tetap tampilkan ringkasan + tanya konfirmasi final**, lalu masuk ke Tahap 1.

### Catatan Penting untuk Agent

- **JANGAN** tanya pakai plain text — selalu `ask_user` (kecuali pesan pembuka & ringkasan).
- **JANGAN** tanya detail kosmetik yang bisa diasumsikan (typography minor, spacing, icon library).
- **JANGAN** ulangi pertanyaan yang sudah terjawab.
- **BOLEH skip tier** kalau info di tier tersebut sudah jelas dari request awal.
- **BOLEH re-batch** pertanyaan lintas tier (misal 2 Tier 2 + 2 Tier 3 dalam 1 call) untuk efisiensi.
- **SELALU** dokumentasikan asumsi di section **"Catatan & Asumsi"** PRD kalau user opt-out dari pertanyaan tertentu.
- **Jika user bilang "yang penting jadi dulu" / "simple aja"** — interview tetap dilakukan, tapi agent kompres jadi 1–2 batch dengan pertanyaan paling kritis saja (identitas + fitur MVP + tech stack).
- **Jika user kasih info super lengkap di request awal** (misal: *"buatkan PRD untuk POS kafe multi-outlet, Next.js + Supabase, sudah ada Figma di link X, target 50 outlet dalam 6 bulan"*) — agent boleh skip interview, langsung tampilkan ringkasan asumsi + tanya konfirmasi final, lalu generate.

### Contoh Flow

**Request user**: *"buatkan PRD untuk aplikasi POS untuk kafe"*

**Agent — Pembuka**: pesan pembuka interview (template di atas).

**Agent — Batch 1** (Tier 1, 4 pertanyaan via `ask_user`):
- Q1: Bahasa output PRD? → ID / EN
- Q2: Target deployment? → Vercel / Self-host Docker / Cloud (AWS/GCP) / Belum tahu
- Q3: Auth method? → Email-password / OAuth (Google) / SSO enterprise
- Q4: Ada Figma/design system reference? → Ya (sebutkan) / Tidak (generate default)

*(user jawab batch 1)*

**Agent — Evaluasi**: POS untuk kafe sudah jelas konteksnya, payment & printer struk masih ambigu → lanjut batch 2.

**Agent — Batch 2** (Tier 3, 3 pertanyaan via `ask_user`):
- Q1: Payment method? → Tunai saja / Tunai + QRIS / Multi-payment (tunai + QRIS + debit + e-wallet)
- Q2: Integrasi printer struk? → Bluetooth thermal / Network printer / Tanpa printer
- Q3: Multi-outlet atau single outlet? → Single outlet / Multi-outlet dengan konsolidasi / Multi-outlet independen (tanpa konsolidasi)

*(user jawab batch 2)*

**Agent — Evaluasi**: Tidak ada ambiguitas kritis tersisa → tampilkan ringkasan asumsi + tanya konfirmasi final.

**Agent — Ringkasan**:
```
Asumsi terkonfirmasi:
- POS kafe, single outlet, deployment Vercel, auth email-password
- Payment: tunai + QRIS, printer struk Bluetooth, design system default
- Bahasa output: Indonesia

Lanjut generate dengan info ini? (Yes / No, mau ubah sesuatu)
```

*(user jawab Yes)*

**Agent → Tahap 1**: Generate 4 file (PRD, TODO, Implementation Prompt, UI/UX Prompt) + section "Catatan & Asumsi" di PRD yang list semua asumsi di atas.

---
