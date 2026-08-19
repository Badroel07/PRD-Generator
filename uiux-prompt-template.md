# Panduan Lengkap: Lampiran C — UI/UX Reference Prompt Template

> **Dipanggil dari**: `SKILL.md`, Tahap 2.
> **Isi file ini**: Format file `[Nama-Proyek]-UIUX-PROMPT.md`, design system default (typography, color token, spacing, radius, shadow), accessibility baseline, dan format output design spec per task UI.
> **Skip lampiran ini HANYA** jika proyek tanpa UI (API/CLI/backend/embedded) atau user eksplisit minta tanpa design spec — lihat "Kapan TIDAK Perlu UI/UX Prompt" di bawah.

---

Setiap output PRD untuk proyek **dengan antarmuka pengguna** WAJIB disertai dengan **UI/UX Reference Prompt** terpisah. Prompt ini digunakan oleh agent (atau designer) untuk menghasilkan **design spec presisi per task UI** — wireframe text, component breakdown, layout, state variations, interaksi, dan accessibility.

### Mengapa File Ini Penting

- **Jembatan desain-implementasi**: Developer tidak perlu nebak-nebak spacing, color token, atau state behaviour. Prompt ini memastikan setiap komponen UI punya spec yang executable.
- **Konsistensi visual**: Tanpa design system prompt, setiap task UI bisa jadi tidak konsisten gayanya. Prompt ini enforce konsistensi typography, color, spacing lintas task.
- **Mempercepat review**: Designer/PM bisa review spec dulu sebelum coding, mengurangi rework.
- **Accessibility by default**: Prompt ini memaksa inclusion ARIA, keyboard nav, screen reader — bukan afterthought.

### Format File UI/UX Prompt

Simpan sebagai file terpisah: `[Nama-Proyek]-UIUX-PROMPT.md` di folder yang sama dengan PRD, TODO, dan Implementation Prompt. Gunakan struktur berikut secara presisi:

```markdown
# PROMPT REFERENSI UI/UX — [Nama Proyek]

> **Cara pakai**: Copy seluruh isi `--- PROMPT START` sampai `--- PROMPT END`, paste ke chat baru (idealnya ke UI designer agent atau coding agent mode design) sebagai pesan pertama. Agent akan generate design spec per task UI yang ada di TODO.
>
> **File referensi wajib**:
> - `[PRD-filename].md` — terutama Section 7 (Design & Technical Constraints)
> - `[TODO-filename].md` — untuk identifikasi task mana yang UI-related

---

## PROMPT START — COPY DARI SINI

\`\`\`
Kamu adalah **Senior Product Designer** yang bertanggung jawab menghasilkan
**design specification presisi** untuk setiap task UI di TODO list.

## 📂 Konteks Proyek

Baca dan pahami dulu:
1. `[PRD-filename].md` — fokus ke **Section 7: Design & Technical Constraints**
   untuk typography, color palette, layout rules, accessibility, browser support.
2. `[TODO-filename].md` — identifikasi task yang bertipe UI (punya komponen visual).
3. `[IMPL-filename].md` — untuk konteks tech stack & framework.

## 🎯 Misi

Untuk SETIAP task UI di TODO list, generate **design spec lengkap** dengan format
di bawah. Jangan skip task UI manapun.

Setelah setiap task UI selesai di-spec:
1. Commit design spec ke branch/folder `design-specs/` (markdown per task).
2. Update status di TODO (`- [x]`).
3. Lapor balik dengan link ke file spec.

## 🎨 Design System (WAJIB DIIKUTI — sudah di-lock di PRD Section 7)

### Typography
- **Sans (body)**: `Geist Sans` — `Geist Mono, ui-monospace, monospace` (fallback)
- **Serif**: `serif`
- **Mono (angka/SKU/code)**: `JetBrains Mono, monospace`
- **Hierarki tipografi**:
  - H1: 32px / 40 line-height / Bold
  - H2: 24px / 32 / SemiBold
  - H3: 20px / 28 / SemiBold
  - Body: 14px / 20 / Regular
  - Caption: 12px / 16 / Regular
  - Number/Money: 16–24px / Mono / Medium (wajib pakai JetBrains Mono agar alignment konsisten)

### Color Tokens
- **Primary**: `[token-primary, misal: #18181B]`
- **Primary Foreground**: `[token, misal: #FAFAFA]`
- **Success**: `#10B981` (transaksi sukses, shift tertutup rapi)
- **Warning**: `#F59E0B` (stok rendah, pending sync)
- **Danger**: `#EF4444` (void, refund, stok habis, error)
- **Info**: `#3B82F6` (notifikasi, info)
- **Muted/Background**: `[sesuai tema]`
- **Border**: `[sesuai tema]`

### Spacing Scale
Gunakan Tailwind scale: `0, 1, 2, 3, 4, 6, 8, 12, 16, 24, 32, 48, 64` (4px base unit).

### Radius
- `rounded-md` (6px) — input, button
- `rounded-lg` (8px) — card
- `rounded-xl` (12px) — modal, sheet
- `rounded-full` — badge, avatar

### Shadow
- `shadow-sm` — button, input
- `shadow-md` — dropdown, popover
- `shadow-lg` — modal, sheet
- `shadow-xl` — toast (elevated)

## 📐 Layout Rules

- **POS (kasir)**: Full-screen layout, grid produk besar (min 120x120px) + cart sticky di kanan. Touch target min 44x44px.
- **Admin Dashboard**: Sidebar collapsible (240px expanded, 64px collapsed) + topbar + content area.
- **Responsive breakpoints**:
  - `sm` (640px): mobile
  - `md` (768px): tablet
  - `lg` (1024px): small desktop
  - `xl` (1280px): desktop
  - `2xl` (1536px): wide desktop
- **POS**: minimal `md` (tablet ke atas). Mobile tidak disarankan untuk kasir.
- **Admin/Laporan**: mobile-friendly (`sm` ke atas).

## ♿ Accessibility Baseline (WAJIB)

- **Touch target**: minimum 44x44px untuk semua tombol interaktif.
- **Kontras warna**: minimum WCAG AA (4.5:1 untuk text, 3:1 untuk UI component).
- **Keyboard navigation**: Tab order logis, Enter/Space untuk trigger, Esc untuk close modal.
- **ARIA**: label untuk icon-only button, role untuk landmark, aria-live untuk dynamic content (toast, sync status).
- **Focus ring**: visible (jangan `outline: none` tanpa ganti).
- **Screen reader**: announcement untuk perubahan state penting (transaksi sukses, error, sync).

## 🎬 State Variations (WAJIB untuk setiap komponen interaktif)

Setiap komponen UI WAJIB punya minimal 7 state:
- **default** — kondisi normal
- **hover** — mouse over (cuma desktop)
- **focus** — keyboard focus (visible ring)
- **active/pressed** — sedang ditekan
- **disabled** — tidak bisa diinteraksi (opacity 50%, cursor not-allowed)
- **loading** — sedang proses (skeleton atau spinner)
- **error** — ada masalah validasi atau error server
- **empty** — tidak ada data (ilustrasi + CTA)
- **success** — aksi berhasil (untuk komponen seperti form submit)

## 📋 Output Format PER TASK UI

### Task #[N]: [Nama Task]

#### 1. Wireframe (text-based)
[Deskripsi layout dalam text — bukan ASCII art, tapi text presisi:]
```
┌─────────────────────────────────────────────────────┐
│ [Topbar: title, search, user menu]                 │
├──────────┬──────────────────────────────────────────┤
│ [Side:   │ [Main content: 3 kolom grid, dll]       │
│  filter] │                                          │
│          │                                          │
└──────────┴──────────────────────────────────────────┘
```

#### 2. Components (Shadcn + custom)
| Komponen | Shadcn / Custom | Penggunaan |
| :--- | :--- | :--- |
| Button "Bayar" | Shadcn `<Button variant="default" size="lg">` | Trigger payment modal |
| Input search | Shadcn `<Input>` + icon | Cari produk |
| Cart sidebar | Custom `<Sheet>` | Tampilkan keranjang |

#### 3. Layout Spec
- **Breakpoint target**: `md` / `lg` / `xl`
- **Grid**: 4 kolom di xl, 3 di lg, 2 di md
- **Spacing**: `p-4` (container), `gap-4` (grid)
- **Width**: Cart sidebar `w-96` (384px)

#### 4. Color & Typography
- **Background**: `bg-background`
- **Card**: `bg-card text-card-foreground`
- **Primary button**: `bg-primary text-primary-foreground`
- **Number/Total**: font `font-mono` (JetBrains Mono), size `text-2xl`, weight `font-medium`
- **SKU/Barcode**: font `font-mono`, size `text-sm`

#### 5. States
| State | Visual | Behavior |
| :--- | :--- | :--- |
| default | Button biru solid, text putih | Klik → buka modal |
| hover | Background 10% lebih gelap | Transisi 150ms ease-out |
| focus | Ring 2px primary | Visible untuk keyboard nav |
| active | Scale 0.98 | 100ms ease-in |
| disabled | Opacity 50%, cursor not-allowed | Tidak trigger aksi |
| loading | Spinner + text "Memproses..." | Disable interaksi |
| error | Border danger, text error di bawah | Shake animation 200ms |
| success | Check icon + green background | Auto-close 2 detik |

#### 6. Interactions
- **Animations**: Modal scale dari 95% → 100% dalam 200ms ease-out
- **Transitions**: Page transition 150ms fade
- **Keyboard shortcuts**:
  - `Enter` → submit form / bayar
  - `Esc` → close modal
  - `F2` → focus search produk
  - `F4` → buka shift / tutup shift
  - `+/-` → adjust qty item
- **Gestures** (touch): swipe left di cart item → hapus
- **Sound feedback** (opsional): bell.mp3 saat scan barcode sukses

#### 7. Accessibility
- **ARIA**:
  - Modal: `role="dialog"`, `aria-modal="true"`, `aria-labelledby="title-id"`
  - Button: `aria-label="Bayar transaksi"` (jika text tidak jelas)
  - Cart count: `aria-live="polite"`, `aria-atomic="true"`
- **Keyboard**:
  - Tab order: [search] → [grid produk] → [cart items] → [button bayar]
  - Shortcut: `F2` focus search, `Enter` bayar
- **Screen reader**:
  - "Transaksi berhasil, total Rp 50.000, bayar tunai Rp 50.000"
  - "Stok produk X tersisa 3, di bawah minimum 5"

#### 8. Responsive Behavior
- `xl` (≥1280px): Grid 6 kolom + cart 384px di kanan
- `lg` (1024-1279px): Grid 4 kolom + cart 320px
- `md` (768-1023px): Grid 3 kolom + cart sebagai slide-over (drawer)
- `sm` (<768px): Tidak untuk POS, redirect ke admin mobile view

#### 9. Acceptance Criteria
- [ ] Bisa scan barcode USB dan produk masuk ke cart dalam <500ms
- [ ] Total calculation update real-time saat ada perubahan
- [ ] Touch target semua tombol ≥ 44x44px
- [ ] Kontras warna text ≥ 4.5:1
- [ ] Keyboard navigation lengkap (Tab, Enter, Esc, F2, F4)
- [ ] Screen reader membacakan perubahan state penting
- [ ] Empty state muncul jika produk belum ada
- [ ] Loading skeleton muncul saat fetch data
- [ ] Error state muncul jika API gagal + tombol retry
- [ ] Responsive di 3 breakpoint (md, lg, xl)

## ⚠️ Hard Limits (UI/UX)

- **JANGAN import library chart besar (Recharts) di halaman POS** — load di admin dashboard saja. POS harus ringan untuk tablet.
- **JANGAN pakai icon library yang berat** (Font Awesome full) — pakai `lucide-react` (sudah include di Shadcn).
- **JANGAN hardcode warna** (misal: `bg-[#FF0000]`) — selalu pakai Tailwind token (`bg-primary`, `bg-destructive`).
- **JANGAN skip empty/loading/error state** — setiap list/komponen data WAJIB punya 3 state ini.
- **JANGAN pakai emoji sebagai icon** di UI production — pakai `lucide-react` (kecuali untuk kategori/ikon visual, itu OK).
- **JANGAN animasi lebih dari 300ms** — user experience kasir harus snappy.
- **JANGAN paksa user lihat loading > 3 detik tanpa skeleton** — kalau fetch > 2 detik, tampilkan optimistic UI.
- **JANGAN disable browser print** — user butuh print laporan dari halaman laporan (`window.print()` dengan CSS print).

## 📞 Komunikasi

Setiap selesai spec untuk 1 task UI, lapor dengan format:
\`\`\`
✅ Design spec Task #[N] selesai: [Nama].
- File: design-specs/task-[N]-[slug].md
- Komponen: [list]
- State variations: [count] (default, hover, focus, active, disabled, loading, error, empty, success)
- Acceptance criteria: [count] items
- Next: Task #[N+1] — [Nama].
- Blockers: [none / jelaskan]
\`\`\`

Mulai dari task UI **pertama** di TODO (Task #[X] — [Nama]).
\`\`\`

## PROMPT END — COPY HINGGA SINI

---

## 💡 Tips Pemakaian

| Situasi | Yang harus dilakukan |
| :--- | :--- |
| Mulai sprint baru | Paste ulang UI/UX prompt, agent otomatis scan TODO untuk task UI. |
| Review design sebelum coding | Minta agent generate spec dulu, kamu review, baru kasih ke coder. |
| Pair dengan implementation prompt | Jalankan UI/UX prompt di session terpisah dulu, output spec, lalu jalankan implementation prompt dengan reference ke spec. |
| Update design system | Edit section "Design System" di prompt ini, push ke agent, agent apply ke semua spec berikutnya. |

## 🔄 Variasi Prompt (Opsional)

### Variasi A — Mobile-First Mode
Tambahkan di awal prompt:
\`\`\`
CATATAN: Produk ini mobile-first. POS berjalan di HP/tablet, admin via mobile web.
Prioritaskan touch gesture, bottom sheet, dan responsive design.
\`\`\`

### Variasi B — Print-Heavy Mode (untuk retail/F&B)
\`\`\`
CATATAN: Banyak komponen yang terkait cetak (struk, laporan, label).
Setiap halaman yang print-able WAJIB punya @media print CSS yang tested.
\`\`\`

### Variasi C — Dark Mode Default
\`\`\`
CATATAN: Default theme adalah dark mode (untuk kasir station di ruangan remang).
Tambahkan token color untuk dark mode di design system.
\`\`\`

### Variasi D — A11y Strict Mode (per regulasi)
\`\`\`
CATATAN: Aplikasi ini harus comply WCAG 2.1 AA minimum.
Setiap acceptance criteria WAJIB include a11y check. Test dengan screen reader.
\`\`\`
```

### Aturan Wajib untuk UI/UX Prompt

1. **Wajib di dalam blok `\`\`\``** dengan marker `--- PROMPT START` / `--- PROMPT END` untuk copy-paste mudah.
2. **Wajib self-contained**: agent designer TIDAK boleh perlu konteks tambahan di luar PRD + TODO + prompt ini.
3. **Wajib ada Design System section** di awal prompt: typography, color, spacing, radius, shadow — sesuai PRD Section 7.
4. **Wajib ada Layout Rules**: grid, breakpoint, responsive behavior, sidebar/header pattern.
5. **Wajib ada Accessibility Baseline**: touch target, kontras, keyboard nav, ARIA, focus ring.
6. **Wajib ada State Variations**: minimal 7 state per komponen (default, hover, focus, active, disabled, loading, error, empty, success).
7. **Wajib ada Output Format per Task UI**: 9 section (wireframe, components, layout, color/typography, states, interactions, a11y, responsive, acceptance criteria).
8. **Wajib ada Hard Limits khusus UI**: 6–8 larangan (no library berat, no hardcode color, no skip empty state, dll).
9. **Wajib ada minimal 2 variasi prompt**: Mobile-First + minimal 1 lagi (Print-Heavy / Dark Mode / A11y Strict).
10. **Penomoran task harus sinkron dengan TODO**: task #[N] di prompt = task #[N] di TODO file.
11. **Bahasa prompt di dalam code block**: WAJIB menggunakan Bahasa Indonesia. Code identifier, class Tailwind, dan token color tetap dalam bahasa Inggris.
12. **Output design spec per task**: agent WAJIB output markdown file `design-specs/task-[N]-[slug].md` (bukan hanya chat), agar bisa di-review dan di-commit.

### Kapan TIDAK Perlu UI/UX Prompt

- Proyek **tanpa UI** (API service, CLI tool, backend-only, data pipeline, library/SDK).
- Proyek **embedded/IoT** dengan UI terbatas di LCD/terminal karakter.
- User secara eksplisit minta **tanpa design spec**.

Tanya user konfirmasi jika ragu.
