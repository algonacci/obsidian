---
tags:
  - omniflow
  - starter
  - changelog
  - backport
---

# Template Changelog — Perubahan yang Wajib Dipropagasi

Registry perubahan **template dasar Omniflow** (fix/improvement pada file bawaan starter) yang harus di-backport ke **semua modul turunan**. Modul baru yang di-fork setelah tanggal fix di Omniflow-Starter otomatis sudah bersih.

> Cara pakai: setiap nemu bug/improvement di file bawaan template (bukan kode fitur modul), tambahkan entry di sini + update tabel rollout. Eksekusi backport per modul pakai prompt [[Template Backport]].

---

## TPL-002 — networkGuard force-enable semua kontrol form (2026-07-18)

**Severity:** Medium — bug diam-diam, merusak fitur apapun yang pakai `disabled` state
**File:** `public/js/networkGuard.js`

**Bug:** Setiap siklus cek koneksi yang hasilnya ONLINE, `toggleButtons('ONLINE')` me-loop **semua** button/input/select/textarea dan set `el.disabled = false` — termasuk kontrol yang sengaja di-disable aplikasi (locked form field, tombol self-protection, dll). Gejala: kontrol yang dikunci via JS/server-side render ke-enable sendiri beberapa detik setelah page load.

**Fix:** Di branch ONLINE, hanya restore elemen yang guard disable sendiri (punya class `network-guarded`), lalu bersihkan marker:

```js
} else {
    // Only restore elements this guard disabled itself — otherwise
    // every ONLINE check would force-enable controls the app
    // intentionally disabled (e.g. locked form fields).
    if (!el.classList.contains('network-guarded')) {
        return;
    }
    el.disabled = el.dataset.originalDisabled === 'true';
    delete el.dataset.originalDisabled;
    el.style.opacity = '';
    el.style.cursor = '';
    el.classList.remove('network-guarded');
}
```

(Mengganti blok `else` lama yang berisi `if (el.dataset.originalDisabled) {...} else { el.disabled = false; }`.)

---

## TPL-001 — session-timeout ketinggalan nilai testing (2026-07-18)

**Severity:** High — user ke-logout paksa tiap 30 detik–1 menit
**File:** `public/js/session-timeout.js`

**Bug:** Nilai production 24 jam dikomentari, nilai testing aktif (varian yang ditemukan: 30 detik di XRM/DealDesk, 1 menit di Starter/Profilex). Modal "session expired" muncul terus-menerus.

**Fix:** Hapus nilai testing, aktifkan kembali nilai production:

```js
const sessionTimeout = 24 * 60 * 60 * 1000; // 24 hours - Should match server session.cookie.maxAge
const warningTime = 2 * 60 * 1000; // 2 minutes before timeout
```

---

## Rollout Status

| Modul | TPL-001 | TPL-002 | Commit | Catatan |
|-------|---------|---------|--------|---------|
| Omniflow-Starter | ✅ | ✅ | `a1a1b3b` | Source of truth — fork baru otomatis bersih |
| Omniflow-DealDesk | ✅ | ✅ | `916f2e7`, `03505d0` | Tempat bug ditemukan |
| Omniflow-Profilex | ✅ | ✅ | `94f5fde` | |
| Omniflow-HRIS | n/a | ❌ | | Arsitektur lama; tidak punya session-timeout.js, tapi networkGuard.js kena TPL-002 (baris ±381) |
| Omniflow-XRM | ❌ | ❌ | | Belum di-clone lokal; sumber fork DealDesk, dipastikan kena dua-duanya |
| Omniflow-Inventory | ❌ | ❌ | | Belum dicek |
| Omniflow-Sales | ❌ | ❌ | | Belum dicek |
| Omniflow-Purchasing | ❌ | ❌ | | Belum dicek |
| Omniflow-Accounting | ❌ | ❌ | | Belum dicek |
| Omniflow-PoS | ❌ | ❌ | | Belum dicek |
| Omniflow-Helpdesk | ❌ | ❌ | | Belum dicek |
| Omniflow-KMS | ❌ | ❌ | | Belum dicek |
| Omniflow-Asset-Management | ❌ | ❌ | | Belum dicek |
| _...modul lain_ | ❌ | ❌ | | Cek saat modul dikerjakan berikutnya |

**Konvensi:** ✅ = fixed & pushed · ❌ = belum · n/a = file tidak ada di modul itu

---

## Kandidat (belum jadi keharusan)

- **AI Use Cases `category` (feature slot)** — DealDesk commit `5b6eb10` menambah kolom `category` + scoping di `ai_use_cases` supaya fitur bisa ambil prompt by category (mis. `quotation_pricing`), editable dari admin tanpa sentuh kode. Konsep `category`-nya layak masuk starter kalau modul lain mulai butuh prompt configurable per fitur; kolom `company_id`-nya spesifik DealDesk.
