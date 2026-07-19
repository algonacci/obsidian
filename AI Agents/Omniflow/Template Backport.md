---
tags:
  - agent
  - prompt
  - omniflow
  - backport
  - maintenance
agents:
  - opencode
  - claude-code
  - codex
---

# Template Backport (Omniflow Maintenance)

> **Task:** Terapkan perubahan template dasar dari [[Template Changelog]] ke modul Omniflow yang sedang dibuka

## Instruksi

Kamu sedang berada di salah satu repo modul Omniflow. Baca `obsidian/Omniflow/Template Changelog.md` (repo vault, biasanya `~/Documents/GitHub/obsidian`), lalu untuk **setiap entry TPL-xxx** yang statusnya belum ✅ untuk modul ini:

1. **Cek dulu, jangan asal patch.** Buka file target di modul ini dan verifikasi bug/pola lamanya memang ada. File bisa saja sudah beda (modul arsitektur lama, atau sudah pernah di-fix manual). Kalau file tidak ada → tandai `n/a`. Kalau pola lama tidak ketemu tapi file ada → periksa manual apakah sudah ter-fix atau berbeda struktur, jangan paksakan patch.
2. **Terapkan fix persis seperti di changelog** (diff/snippet ada di tiap entry). Jangan improvisasi di luar scope entry.
3. **Validasi**: `node --check` untuk file JS; kalau modul punya lint (`npm run lint`), jalankan untuk file yang diubah.
4. **Commit per backport batch** dengan format:
   `fix: backport TPL-xxx[, TPL-yyy] from template changelog`
   Body commit menyebut ringkasan tiap TPL. Push ke `main` (atau branch kerja aktif kalau repo sedang mid-feature).
5. **Update rollout table** di `Template Changelog.md`: tandai ✅/n/a + isi kolom commit, lalu commit & push vault-nya juga.

## Rules

- Satu sumber kebenaran: snippet di changelog. Kalau kondisi modul membuat snippet tidak bisa diterapkan bersih, laporkan ke user, jangan tebak-tebakan.
- Jangan sekalian refactor hal lain di file yang sama — backport harus berupa diff minimal supaya gampang di-review lintas puluhan modul.
- Preserve perubahan lokal modul yang memang disengaja.
