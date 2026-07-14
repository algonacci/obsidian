---
tags:
  - agent
  - prompt
  - omniflow
  - rebranding
  - starter
agents:
  - opencode
  - claude-code
  - codex
---

# Starter Template Rebranding

> **Task:** Replace 'omniflow-starter' branding with a new module name

## Scope Definition

### MUST (Critical Branding — 10 files)

- `package.json` (name field, pm2:logs script)
- `package-lock.json` (all name references)
- `.env.example` (header comment, DB_NAME, REDIS_KEY_PREFIX, OTEL_SERVICE_NAME)
- `.env.docker` (header comment, INSTANCE_NODE_NAME, DB_NAME, REDIS_KEY_PREFIX, SMTP_FROM_NAME, OTEL_SERVICE_NAME)
- `.env.prod` (header comment, DB_NAME, S3_FOLDER_NAME, OTEL_SERVICE_NAME)
- `db/migrations/` (application defaultTo values)
- `config/index.js` (if any hardcoded values)
- `helpers/langfuse.js` (source field)
- `helpers/log.js` (application field)
- `routes/api/health/health.api.controller.js` (service field)

### SHOULD (Consistency — 5 files)

- `README.md` (title, clone instructions, DB_NAME example)
- `CLAUDE.md` (application description, OTEL_SERVICE_NAME default doc)
- `DOCKER.md` (clone paths, Docker volume names for backups)
- `documentation/docs/getting-started.md` (description, clone instructions, DB_NAME)
- `omniflow_starter_context_extraction.md` → rename to `omniflow_<module>_context_extraction.md`

### NICE (Generated — 3 patterns)

- `documentation/site/` (auto-generated, rebuild after changes)
- `documentation/mkdocs.yml` (site_name field)
- `documentation/site/search/search_index.json` (auto-generated, rebuild)

---

## Cross-Platform Requirements

- Use grep for searching (Linux/macOS/Windows Git Bash compatible)
- Use Edit tool for replacements (atomic & safe, platform independent)
- Use git for version control
- NO PowerShell or Windows-only commands
- Handle line endings automatically (CRLF/LF)

---

## Search Strategy

Find all occurrences across codebase:

```bash
grep -r "omniflow-starter" . \
  --include="*.js" \
  --include="*.json" \
  --include="*.md" \
  --include="*.env*" \
  --include="*.yml" \
  --include="*.yaml" \
  --exclude-dir=node_modules \
  --exclude-dir=.git \
  2>/dev/null
```

Expected results:
- package.json: 2 occurrences
- package-lock.json: 2 occurrences
- .env files: ~12 occurrences
- Source code: ~4 occurrences
- Documentation: ~10 occurrences
- **Total: ~30+ replacements across ~15 files**

---

## Replacement Strategy

**Pattern to replace:**
- Old: `omniflow-starter`
- New: `omniflow-<module-name>`

Also handle derived patterns:
- Database name: `omniflow_starter` → `omniflow_<module>`
- File rename: `omniflow_starter_context_extraction.md` → `omniflow_<module>_context_extraction.md`

**Method:** File-by-file using Edit tool (most reliable, cross-platform safe)

### Examples

**Before (.env files):**
```env
# Omniflow Starter Configuration
DB_NAME=omniflow_starter
REDIS_KEY_PREFIX=omniflow-starter:
OTEL_SERVICE_NAME=omniflow-starter
```

**After (.env files):**
```env
# Omniflow Modul Baru Configuration
DB_NAME=omniflow_modul_baru
REDIS_KEY_PREFIX=omniflow-modul-baru:
OTEL_SERVICE_NAME=omniflow-modul-baru
```

**Before (package.json):**
```json
{
  "name": "omniflow-starter",
  "scripts": {
    "pm2:logs": "pm2 logs omniflow-starter"
  }
}
```

**After (package.json):**
```json
{
  "name": "omniflow-modul-baru",
  "scripts": {
    "pm2:logs": "pm2 logs omniflow-modul-baru"
  }
}
```

**Before (helpers/log.js):**
```javascript
application: "omniflow-starter",
```

**After (helpers/log.js):**
```javascript
application: "omniflow-modul-baru",
```

**Before (DOCKER.md):**
```bash
cd omniflow-starter
docker run --rm -v omniflow-starter_mysql_data:/data
docker volume rm omniflow-starter_mysql_data
```

**After (DOCKER.md):**
```bash
cd omniflow-modul-baru
docker run --rm -v omniflow-modul-baru_mysql_data:/data
docker volume rm omniflow-modul-baru_mysql_data
```

---

## Verification Steps

### Step 1: Verify zero old occurrences remain
```bash
grep -r "omniflow-starter" . \
  --include="*.js" --include="*.json" --include="*.md" --include="*.env*" \
  --exclude-dir=node_modules --exclude-dir=.git \
  2>/dev/null | wc -l
# Should output: 0
```

### Step 2: Verify new branding exists
```bash
grep -r "omniflow-modul-baru" . \
  --include="*.js" --include="*.json" --include="*.md" --include="*.env*" \
  --exclude-dir=node_modules --exclude-dir=.git \
  2>/dev/null | wc -l
# Should output: 30+
```

### Step 3: Verify database name updated
```bash
grep -r "omniflow_modul_baru" . \
  --include="*.js" --include="*.md" --include="*.env*" \
  2>/dev/null
```

### Step 4: Verify file rename
```bash
ls -la omniflow_modul_baru_context_extraction.md  # Should exist
ls -la omniflow_starter_context_extraction.md     # Should NOT exist
```

### Step 5: Git verification
```bash
git status
# Should show: ~15 modified, 1 renamed
```

---

## Finalization

Create single atomic commit:
```bash
git add -A
git commit -m "refactor: rebrand omniflow-starter to omniflow-<module>

- Update all service names and branding across codebase
- Replace database name: omniflow_starter → omniflow_<module>
- Update Redis key prefix and OTEL service name
- Update Docker volume names for consistency
- Rename context extraction document
- Update configuration files (.env.*, package.json)
- Update documentation (README, CLAUDE.md, DOCKER.md)

Total changes:
- 30+ replacements across 15 files
- 1 file renamed"
```

---

## Success Criteria

- Zero 'omniflow-starter' occurrences remaining
- All new module references present throughout codebase
- Database name consistently updated
- Configuration files aligned
- Documentation updated and consistent
- Context extraction file renamed
- Single atomic commit created
- Git status clean
- Application starts without branding-related errors

---

## Related

- [[Agent Execution]] — build modules after rebranding
- [[Port Management]] — ensure ports match the new module
- [[Project Initialization]] — the PRD that becomes the new module
