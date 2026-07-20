---
tags:
  - MOC
  - lessons-learned
  - postmortem
  - knowledge-base
---

# 📚 Lessons Learned

> Postmortems, gotchas, dan wisdom yang udah kita dapet dari incident nyata. **Read sebelum bootstrap/copy/push operations.**

## Why This Folder Exists

Obsidian punya banyak technical guidance (`Omniflow/Architecture Reference.md`, `AI Agents/*`, dll) — tapi **incident-driven wisdom** belum punya home. Folder ini jadi:
- 🩺 **Postmortem** dari insiden yang udah terjadi
- 🛡️ **Pre-flight checklist** sebelum operasi berisiko
- 🔍 **Pattern recognition** — supaya AI agent (atau Eric sendiri) bisa nge-spot red flags lebih awal

## Index

| Incident | Date | Severity | Status |
|----------|------|----------|--------|
| [[Wrong-Push Incident (2026-07-20)]] | 2026-07-20 | 🟠 High | ✅ Resolved |

## Themes yang Muncul

- **Git remote safety** — selalu verify destination sebelum push
- **Bootstrap workflow** — copy + rebrand operations butuh checklist sendiri
- **Fail-fast principles** — better stop & verify than recover later

## How to Use This Folder

1. **Sebelum operasi berisiko** (bootstrap repo, force-push, mass replacement): baca postmortem terkait dulu
2. **Setelah incident**: bikin postmortem baru di sini dengan pola yang sama:
   - What happened (timeline)
   - Root cause (kenapa bisa terjadi)
   - Damage assessment
   - Recovery steps
   - Prevention checklist
3. **Update related prompts** (`AI Agents/Omniflow/*.md`) dengan link ke postmortem ini

## Conventions

- File naming: `<Incident Name> (YYYY-MM-DD).md`
- Frontmatter wajib: `tags`, `created`, `incident`, `status`, `severity`
- Severity levels: 🟢 Low / 🟡 Medium / 🟠 High / 🔴 Critical
- Status: `Open` / `Investigating` / `Resolved` / `Won't Fix`

## Related

- [[../AI Agents/Omniflow/README|Omniflow Agents]] — workflow yang harus comply sama lessons learned ini
- [[../Omniflow/Context Extraction (Audit)]] — original DO NOT COPY guidance
