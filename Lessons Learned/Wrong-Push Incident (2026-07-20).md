---
tags:
  - lessons-learned
  - postmortem
  - workflow
  - safety
  - git
  - omniflow
created: 2026-07-20
incident: ONNI-Group bootstrap wrong-push to Omniflow-Starter
status: resolved
severity: high
---

# 🚨 Lessons Learned — Wrong-Push Incident (2026-07-20)

> **Read this before ANY bootstrap, copy, or push operation across Omniflow repos.**

## What Happened

Saat bootstrap `ONNI-Group` module dari `Omniflow-Starter`, aku:

1. ✅ Copy file/folder dari Starter ke ONNI-Group working dir (correctly excluded `.git/`)
2. ✅ Rebrand `omniflow-starter` → `onni-group`
3. ✅ Set port 9001
4. ❌ **LEWATI** `git remote -v` check
5. ❌ **ASUMSI** remote `origin` di ONNI-Group udah pointing ke `Omniflow-Clients/ONNI-Group`
6. ❌ `git push origin main` → actually pushed ke `Omniflow-id/Omniflow-Starter` (wrong repo)
7. **Result**: 86 files, 1109 insertions, 26091 deletions ter-push ke Starter repo
8. **Recovery**: force-push revert Starter + re-push ke ONNI-Group (15 minutes of work + stress)

## Root Cause

**Missing pre-push verification step.** Existing guidance di Obsidian (e.g. `Context Extraction (Audit).md`, `Starter Template Rebranding.md`) udah cover:
- ✅ Don't copy `.git/`
- ✅ Don't copy `node_modules/`, `logs/`, `uploads/`
- ✅ Don't copy `documentation/site/`
- ✅ Don't copy `.secrets`, `*.secrets`

**TAPI nggak ada explicit protocol untuk verify git remote sebelum push.** Dan itu celah fatalnya.

## Pre-Push Safety Protocol (MANDATORY)

Sebelum `git push` di repo yang baru di-clone atau baru di-bootstrap, **SELALU jalankan checklist ini**:

### 1. Verify Remote

```bash
# BUKAN cuma baca .git/config — fetch real data
git remote -v

# Verifikasi URL kedua remote dengan teliti:
# - Apakah URL = repo yang SEHARUSNYA jadi target push?
# - Apakah org/username benar?
# - Apakah nama repo benar?
```

**Red flags**:
- 🚩 Remote `origin` masih pointing ke source repo (e.g. clone dari Starter tapi origin = Starter)
- 🚩 Tidak ada `origin` sama sekali
- 🚩 `origin` di shared/central repo (e.g. `Omniflow-id/Omniflow-Starter`) yang isinya sudah di-bootstrap ke client
- 🚩 URL typo (e.g. `Omniflow-id` vs `Omniflow-Clients`)

### 2. Dry-Run Push

```bash
# Lihat APA yang bakal ke-push, tanpa actually push
git push --dry-run origin main

# Output menunjukkan:
# - Branch yang bakal di-push
# - Commits yang bakal ke-upload
# - Destination URL
```

**Selalu baca output `To <url>` baris**. Kalau URL ≠ expected destination, **JANGAN push**.

### 3. Verify First Commit

```bash
# Untuk repo fresh, HEAD harus = Initial commit (kosong atau LICENSE+README)
git log --oneline origin/main -1
```

Kalau `origin/main` udah punya banyak commits (shared history), kemungkinan **remote pointing ke repo yang salah**.

### 4. Branch Tracking

```bash
git branch -vv
# * main    <commit-hash> [origin/main] <commit-message>
```

Pastikan:
- Branch lokal: `main`
- Tracking: `origin/main` (bukan `upstream/main` atau `starter/main`)
- Commit message ≠ yang baru aja kamu bikin di repo lain

## Bootstrap-Specific Checklist

Saat copy dari satu repo ke repo lain (rebrand/bootstrap workflow):

```bash
# STEP 1: Pre-flight
cd /path/to/SOURCE
git status  # clean?
git log -1  # what's HEAD?

cd /path/to/DEST
git status  # clean? (or initial commit only)
git remote -v  # ⚠️ MANDATORY
git log --oneline origin/main -1  # should be initial commit

# STEP 2: Copy (exclude runtime/git)
# cp -r SOURCE/. DEST/  -- but AVOID copying .git/
# OR: rsync -a --exclude='.git' --exclude='node_modules' SOURCE/ DEST/

# STEP 3: Rebrand + customize
# (file replacements, port changes, etc.)

# STEP 4: Pre-commit verify
git status  # should show only intended changes
git diff --stat  # reasonable count? (10-50 files = good, 500+ = suspicious)

# STEP 5: Commit
git add -A
git commit -m "..."

# STEP 6: ⚠️ PRE-PUSH VERIFY (skipping this = disaster)
echo "=== REMOTE ==="
git remote -v
echo ""
echo "=== DRY RUN ==="
git push --dry-run origin main
echo ""
echo "=== LAST CHANCE: is the URL correct? ==="
read -p "Push to above URL? (yes/no): " confirm
if [ "$confirm" != "yes" ]; then exit 1; fi

# STEP 7: Push
git push origin main
```

## Recovery Playbook (Kalau Terlanjur Salah Push)

### Step 1: Don't Panic
- Repo public/private masih sama, history masih bisa di-recover via reflog
- Force-push asal-asalan = tambah masalah

### Step 2: Verify Damage
```bash
git fetch <wrong-remote>
git log --oneline <wrong-remote>/main -5
git show --stat <bad-commit>  # what was actually pushed
```

### Step 3: Pilih Recovery Strategy

| Opsi | Kapan | Trade-off |
|---|---|---|
| **A. Force-push revert** (reset --hard + push --force) | Repo **private** & **no collaborators** sejak bad push | Cepat, history ke-rewrite |
| **B. Revert commit** (git revert + push) | Repo **public/shared/ada orang lain** | Aman, history intact, +1 commit |
| **C. Manual cleanup** | Kalau ada partial damage | Lambat, manual |

### Step 4: Verify Recovery
```bash
# Wrong remote should be back to good state
git log --oneline <wrong-remote>/main -1
# Should be the previous good commit, not the bad one

# Right remote should have the bootstrap commit
git log --oneline <right-remote>/main -1
# Should be your bootstrap commit
```

## Specific Incidents to Remember

### 2026-07-20: ONNI-Group bootstrap → Omniflow-Starter wrong-push

- **Trigger**: Cloned new `ONNI-Group` repo (presumably from somewhere), but `origin` URL in `.git/config` still pointed to `Omniflow-id/Omniflow-Starter` (the source repo)
- **What I should have done**: `git remote -v` BEFORE any push
- **Damage**: 86 files / 1109 insertions / 26091 deletions pushed to wrong repo
- **Recovery time**: ~15 minutes (force-push revert + re-push to correct remote)
- **Lesson written down**: This file

## Related

- [[Starter Template Rebranding]] — workflow that needs this protocol
- [[Context Extraction (Audit)]] — original `.git/` DO NOT COPY guidance
- [[Port Management]] — also involves git push, applies same protocol

---

**⚠️ This protocol is mandatory for ALL bootstrap/copy/push operations. No exceptions.**
