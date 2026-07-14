---
tags:
  - agent
  - prompt
  - omniflow
  - port-management
  - devops
  - standardization
agents:
  - opencode
  - claude-code
  - codex
---

# Port Management (Omniflow Standardization)

> **Task:** Standardize repo port configuration to match Omniflow policy

## Port Policy

| Range | Purpose |
|-------|---------|
| `4xxx` | Production client apps |
| `6xxx` | Trial apps |
| `9xxx` | Demo / staging / fresh product / internal tooling |
| `10xxx` | `*-Frontend` |
| `20xxx` | `*-Backend` |
| `40xxx` | Infra / control plane / internal services |
| `50xxx` | Starter templates |

## Env Policy

- `.env` = dev
- `.env.prod` = prod
- `.env.example` = template
- `.env` dan `.env.prod` **tidak perlu di-gitignore**
- Baseline MySQL host-level = `33060`, bukan `3306`
- Baseline PostgreSQL host-level = `54320`, bukan `5432`

## Files to Check & Update

- `.env`
- `.env.prod`
- `.env.example`
- `.env.docker` (kalau masih dipakai)
- `config/index.*`
- `docker-compose*.yml`
- `ecosystem.config.*`
- `Dockerfile`
- `vite.config.*` (kalau frontend)
- File runtime lain yang hardcode port

## Documentation to Update

- Repo lokal: `README.md`, `DOCKER.md`, `CLAUDE.md`, `documentation/docs/*`
- Registry pusat: `Omniflow-Documentation/docs/repos/ports.md`

## Rules

- Preserve perubahan user yang sudah ada
- Jangan sentuh repo lain kecuali `Omniflow-Documentation` untuk update `ports.md`
- Kalau branding lama seperti "Starter" masih ada, biarkan kecuali diminta rapikan
- Setelah selesai, cek `git status`, lalu `git add .`, `git commit`, dan `git push`
- Di akhir, kasih ringkasan: repo type, port final, file yang diubah, status commit/push

## Special Rule: Express + Nunjucks Modules

Kalau repo ini keluarga modul Express + Nunjucks:
- Default-kan ke blok `9xxx`
- Ambil port berikutnya yang masih kosong

---

## Summary Output

Setelah selesai, berikan ringkasan:
- Repo type
- Port final
- Files changed
- Commit/push status

---

## Related

- [[Agent Execution]] — modules that need port config
- [[Starter Template Rebranding]] — pair with rebranding workflow
- [[Omniflow Architecture]] — overall architecture context
