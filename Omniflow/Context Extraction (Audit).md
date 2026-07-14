---
tags:
  - omniflow
  - audit
  - bootstrap
  - reference
source: "[[Omniflow-Starter]]"
---

# Omniflow-Starter Audit Report

**Generated:** 2026-03-26
**Repository:** `c:\Users\Braincore\Documents\GitHub\Omniflow-Starter`
**Version:** 1.1.0
**Purpose:** Repository audit for module inheritance/bootstrap analysis

---

## 1. Repository Overview

**Omniflow-Starter** is a production-ready Node.js ERP module starter pack built with:
- **Express.js** (web framework)
- **MySQL** (database with Knex.js migrations)
- **Nunjucks** (server-side templating)
- **Redis** (caching with fallback)
- **RabbitMQ** (job queue with circuit breaker)

The repository provides comprehensive enterprise features including:
- User management with RBAC + PBAC permission system
- Activity logging with sensitive data masking
- 2FA email OTP authentication
- S3-compatible file storage
- Multi-tier rate limiting and bot protection
- Internationalization (EN, ID, ZH)
- AI integration features (chat, copilot, analysis)
- Docker development environment

**Total Size:** ~85KB documentation (CLAUDE.md), 316KB package-lock, plus application code

---

## 2. Top-Level Structure

```
Omniflow-Starter/
├── .dockerignore          # Docker build exclusions
├── .env                   # Environment config (MUST COPY - customize credentials)
├── .env.docker            # Docker-specific environment
├── .env.example           # Environment template
├── .env.prod              # Production environment
├── .git/                  # Git repository data (DO NOT COPY)
├── .github/               # GitHub Actions workflows
├── .gitignore             # Git exclusions
├── .secrets               # GitHub Actions secrets (MUST COPY)
├── .secrets.example       # Secrets template
├── app.js                 # Express app configuration
├── architecture.puml      # PlantUML architecture diagram
├── biome.json             # Code formatter/linter config
├── CLAUDE.md              # AI assistant documentation (85KB)
├── config/                # Configuration system
├── db/                    # Database layer (migrations, seeders, connections)
├── docker-compose.dev.yml # Development Docker services
├── docker-compose.yml     # Production Docker Compose
├── Dockerfile             # Multi-stage Docker build
├── DOCKER.md              # Docker deployment guide
├── documentation/         # MkDocs documentation
├── ecosystem.config.js    # PM2 process manager config
├── examples/              # Example implementations
├── flake.lock             # Nix flake lock
├── flake.nix              # Nix development environment
├── helpers/               # Utility functions (18 modules)
├── instrument.js          # OpenTelemetry setup
├── knexfile.js            # Knex migration config
├── knowledge/             # AI context knowledge base
├── LICENSE                # MIT License
├── locales/               # i18n translations (3 languages)
├── logs/                  # Application logs
├── middlewares/           # Express middlewares (14 modules)
├── node_modules/          # Dependencies (git-ignored)
├── package.json           # npm package definition
├── package-lock.json      # Locked dependencies
├── public/                # Static assets (CSS, JS, images)
├── README.md              # Project overview
├── routes/                # Route handlers & controllers
├── scripts/               # Utility scripts
├── server.js              # Application entry point
├── services/              # Service layer
├── uploads/               # File uploads (empty, git-ignored)
├── views/                 # Nunjucks templates
└── workers/               # Background job workers
```

### Brief Description of Each Top-Level Item

| Item | Type | Description |
|------|------|-------------|
| `.env` | Config | Environment configuration - customize credentials after copy |
| `.env.docker` | Config | Docker-specific environment variables |
| `.env.example` | Template | Safe template for environment setup |
| `.env.prod` | Config | Production environment variables |
| `.github/` | CI/CD | GitHub Actions workflows (ci.yml, cd.yml, test.yml) |
| `.gitignore` | Config | Git ignore rules |
| `.secrets` | Secrets | GitHub Actions secrets (MUST COPY) |
| `app.js` | Core | Express app setup, middleware registration, Nunjucks config |
| `architecture.puml` | Doc | PlantUML architecture diagram |
| `biome.json` | Config | Biome formatter/linter configuration |
| `CLAUDE.md` | Doc | Comprehensive AI assistant context (85KB) |
| `config/` | Core | Centralized configuration with validation |
| `db/` | Core | Database connections, migrations, seeders |
| `docker-compose*.yml` | Infra | Docker orchestration for dev/prod |
| `Dockerfile` | Infra | Multi-stage Docker build |
| `DOCKER.md` | Doc | Docker deployment documentation |
| `documentation/` | Doc | MkDocs documentation site |
| `ecosystem.config.js` | Config | PM2 process manager configuration |
| `examples/` | Dev | Example implementations and test files |
| `flake.nix` | Dev | Nix development environment |
| `helpers/` | Core | 18 utility modules (logging, caching, queue, etc.) |
| `instrument.js` | Core | OpenTelemetry instrumentation setup |
| `knexfile.js` | Config | Knex.js migration configuration |
| `knowledge/` | AI | AI context knowledge base (85 files) |
| `LICENSE` | Legal | MIT License |
| `locales/` | i18n | Internationalization files (EN, ID, ZH) |
| `logs/` | Runtime | Application log files |
| `middlewares/` | Core | 14 Express middleware modules |
| `node_modules/` | Deps | npm dependencies (git-ignored) |
| `package.json` | Config | npm package definition with scripts |
| `package-lock.json` | Lock | Locked dependency versions |
| `public/` | Static | CSS, JS libraries, images |
| `README.md` | Doc | Project quick start guide |
| `routes/` | Core | 106+ route handlers and controllers |
| `scripts/` | Infra | Utility scripts (docker-entrypoint.sh) |
| `server.js` | Core | Application entry point |
| `services/` | Core | Service layer (AI services) |
| `uploads/` | Runtime | File upload directory (empty) |
| `views/` | Core | 44+ Nunjucks templates |
| `workers/` | Core | Background job workers |

---

## 3. Important Reusable Areas

### 3.1 Configuration System (`config/`)

| File | Purpose |
|------|---------|
| `index.js` | Main configuration with all environment variables |
| `development.js` | Development-specific overrides |
| `production.js` | Production-specific overrides |
| `validation.js` | Feature-based environment validation |
| `ai-analysis-config.js` | AI analysis settings |

**Reusability:** HIGH - Core configuration pattern applicable to all modules
**Classification:** MUST COPY (all files including ai-analysis-config.js)
**Notes:** AI configuration included by default

---

### 3.2 Database Layer (`db/`)

```
db/
├── db.js              # MySQL connection pool management
├── redis.js           # Redis connection with auto-reconnect
├── migrations/        # 14 Knex migrations
│   ├── 20250802103717_create_jobs_table.js
│   ├── 20250804064633_create_roles_table.js
│   ├── 20250804064756_create_permissions_table.js
│   ├── 20250804064857_create_role_permissions_table.js
│   ├── 20250804065100_create_users_table.js
│   ├── 20250804065200_create_activity_logs_table.js
│   ├── 20250804065300_create_user_permissions_table.js
│   └── 2026020709*_create_ai_*.js  # AI-specific tables
└── seeders/
    ├── index.js       # Master seeder orchestrator
    └── data/
        ├── users.seeder.js
        ├── permissions.js
        ├── roles.js
        ├── rolePermissions.js
        ├── aiModels.js
        ├── aiUseCases.js
        └── aiAnalysisSettings.js
```

**Reusability:** HIGH - All migrations and seeders included
**Classification:** MUST COPY (all tables including AI)
**Notes:**
- All migrations should be inherited (core + AI features)
- AI-related migrations included by default - remove post-bootstrap if not needed

---

### 3.3 Helpers (`helpers/`)

| Helper | Purpose | Classification |
|--------|---------|----------------|
| `log.js` | Enterprise activity logging | MUST COPY |
| `cache.js` | Redis caching with DB fallback | MUST COPY |
| `queue.js` | RabbitMQ with circuit breaker | MUST COPY |
| `circuitBreaker.js` | Circuit breaker pattern | MUST COPY |
| `beepbot.js` | External critical notifications | MUST COPY |
| `passwordPolicy.js` | Password validation | MUST COPY |
| `permissionOverride.js` | PBAC + RBAC calculation | MUST COPY |
| `emailOTP.js` | Email-based 2FA | MUST COPY |
| `queuedEmail.js` | Non-blocking email queue | MUST COPY |
| `uploadToS3.js` | S3 file uploads | MUST COPY |
| `encryption.js` | Data encryption | MUST COPY |
| `i18n.js` | Internationalization | MUST COPY |
| `langfuse.js` | LLM tracing | MUST COPY |
| `contextLoader.js` | Dynamic context loading | MUST COPY |
| `templateHelpers.js` | Nunjucks filters | MUST COPY |
| `getUserAgent.js` | User-Agent parsing | MUST COPY |
| `getClientIP.js` | Client IP detection | MUST COPY |
| `softDelete.js` | Soft delete implementation | MUST COPY |

**Reusability:** HIGH - Core utility patterns
**Classification:** MUST COPY (all 18)

---

### 3.4 Middlewares (`middlewares/`)

| Middleware | Purpose | Classification |
|------------|---------|----------------|
| `errorHandler.js` | Custom error classes | MUST COPY |
| `centralizedErrorHandler.js` | Global error handling | MUST COPY |
| `csrfProtection.js` | CSRF protection | MUST COPY |
| `jwtAuth.js` | JWT verification | MUST COPY |
| `isLoggedIn.js` | Session auth check | MUST COPY |
| `isAdmin.js` | Admin-only protection | MUST COPY |
| `checkActiveUser.js` | Active user verification | MUST COPY |
| `checkPermission.js` | Permission-based access | MUST COPY |
| `rateLimiter.js` | Multi-tier rate limiting | MUST COPY |
| `botProtection.js` | Bot detection | MUST COPY |
| `compressionMiddleware.js` | Response compression | MUST COPY |
| `cache.js` | Response caching | MUST COPY |
| `corsMiddleware.js` | CORS configuration | MUST COPY |
| `cspNonce.js` | CSP nonce generation | MUST COPY |

**Reusability:** HIGH - All middlewares are core security/performance patterns
**Classification:** MUST COPY (all 14)

---

### 3.5 Routes Structure (`routes/`)

```
routes/
├── index.js                    # Main router
├── admin/
│   ├── admin.router.js         # Admin route aggregator
│   ├── auth/                   # Login, register, OTP (MUST COPY)
│   ├── user/                   # User CRUD, bulk upload (MUST COPY)
│   ├── roles/                  # Role management (MUST COPY)
│   ├── permissions/            # Permission CRUD (MUST COPY)
│   ├── queue/                  # Job queue management (MUST COPY)
│   ├── cache/                  # Cache management (MUST COPY)
│   ├── log/                    # Activity logs (MUST COPY)
│   ├── index/                  # Dashboard (MUST COPY)
│   ├── userProfile/            # Profile management (MUST COPY)
│   ├── aiModels/               # AI model management (MUST COPY)
│   ├── aiUseCases/             # AI use cases (MUST COPY)
│   └── aiAnalysisSettings/     # AI settings (MUST COPY)
├── api/
│   ├── api.router.js           # API route aggregator
│   ├── auth/                   # JWT authentication (MUST COPY)
│   ├── protected/              # Protected route example (MUST COPY)
│   ├── datatable/              # Server-side DataTables (MUST COPY)
│   ├── cache/                  # Cache API (MUST COPY)
│   ├── session/                # Session keep-alive (MUST COPY)
│   ├── health/                 # Health check (MUST COPY)
│   ├── aiChat/                 # AI chat API (MUST COPY)
│   ├── aiAssistant/            # AI assistant (MUST COPY)
│   └── aiCopilot/              # AI copilot (MUST COPY)
└── client/
    └── index/                  # Public landing (MUST COPY)
```

**Reusability:** HIGH - All routes included
**Classification:** MUST COPY (all 106+ route handlers)
**Notes:** AI features included by default - remove post-bootstrap if not needed

---

### 3.6 Views/Templates (`views/`)

```
views/
├── layout/
│   ├── masterLayout.njk        # Admin dashboard layout (MUST COPY)
│   └── clientMasterLayout.njk  # Public client layout (MUST COPY)
├── components/
│   ├── navbar.njk              # Navigation (MUST COPY)
│   ├── sidebar.njk             # Admin sidebar (MUST COPY)
│   ├── footer.njk              # Footer (MUST COPY)
│   ├── alert_flash_message.njk # Flash messages (MUST COPY)
│   ├── cache_info.njk          # Cache indicator (MUST COPY)
│   ├── session_timeout_modal.njk # Session warning (MUST COPY)
│   ├── aiChatSidebar.njk       # AI chat sidebar (MUST COPY)
│   └── aiCopilot.njk           # AI copilot UI (MUST COPY)
├── includes/
│   ├── head.njk                # HEAD section (MUST COPY)
│   └── script.njk              # Script tags (MUST COPY)
└── pages/
    ├── admin/
    │   ├── auth/               # Login, register, OTP (MUST COPY)
    │   ├── user/               # User management (MUST COPY)
    │   ├── roles/              # Role management (MUST COPY)
    │   ├── permissions/        # Permissions (MUST COPY)
    │   ├── queue/              # Queue stats (MUST COPY)
    │   ├── cache/              # Cache stats (MUST COPY)
    │   ├── log/                # Activity logs (MUST COPY)
    │   ├── index/              # Dashboard (MUST COPY)
    │   ├── userProfile/        # Profile (MUST COPY)
    │   ├── errors/             # Error pages 400-500 (MUST COPY)
    │   ├── aiModels/           # AI models (MUST COPY)
    │   ├── aiUseCases/         # AI use cases (MUST COPY)
    │   └── aiAnalysisSettings/ # AI settings (MUST COPY)
    ├── client/
    │   └── index/              # Landing page (MUST COPY)
    └── chat/                   # AI chat page (MUST COPY)
```

**Reusability:** HIGH - All templates included
**Classification:** MUST COPY (all 44+ templates)

---

### 3.7 Public Assets (`public/`)

```
public/
├── css/
│   ├── styles.css          # Custom styles (MUST COPY)
│   ├── dataTables.css      # DataTables styling (MUST COPY)
│   ├── markdown-content.css # Markdown styles (OPTIONAL)
│   └── toastify.min.css    # Toast notifications (MUST COPY)
├── js/
│   ├── bootstrap.bundle.min.js  # Bootstrap 5 (MUST COPY)
│   ├── jquery.js                # jQuery (MUST COPY)
│   ├── dataTables.js            # DataTables (MUST COPY)
│   ├── alpine.js                # Alpine.js (MUST COPY)
│   ├── htmx.js                  # HTMX (MUST COPY)
│   ├── chart.js                 # Chart.js (MUST COPY)
│   ├── marked.js                # Markdown parser (MUST COPY)
│   ├── font-awesome-all.js      # Font Awesome (MUST COPY)
│   ├── toastify-js.js           # Toast notifications (MUST COPY)
│   ├── scripts.js               # Custom scripts (MUST COPY)
│   ├── session-timeout.js       # Session timeout (MUST COPY)
│   └── networkGuard.js          # Network resilience (MUST COPY)
├── img/
│   ├── logo.png                 # App logo (CUSTOMIZE)
│   └── logomark-blue.svg        # Logo vector (CUSTOMIZE)
└── stt_test.html                # Speech-to-text test (DO NOT COPY)
```

**Reusability:** HIGH - All assets included
**Classification:** MUST COPY (all JS/CSS), CUSTOMIZE (logos)

---

### 3.8 Workers (`workers/`)

| File | Purpose | Classification |
|------|---------|----------------|
| `index.js` | WorkerManager orchestrator | MUST COPY |
| `emailWorker.js` | Email job processing | MUST COPY |
| `testWorker.js` | Development test worker | MUST COPY |

**Reusability:** HIGH - Core job processing pattern
**Classification:** MUST COPY (all workers)

---

### 3.9 Services (`services/`)

| File | Purpose | Classification |
|------|---------|----------------|
| `aiChatService.js` | AI conversation management | MUST COPY |
| `aiAnalysisService.js` | AI analysis operations | MUST COPY |

**Reusability:** HIGH - AI services included by default
**Classification:** MUST COPY
**Notes:** AI features included - remove post-bootstrap if not needed

---

### 3.10 Locales (`locales/`)

```
locales/
├── admin/
│   ├── en/    # English admin translations
│   ├── id/    # Indonesian admin translations
│   └── zh/    # Chinese admin translations
└── client/
    ├── en/    # English client translations
    ├── id/    # Indonesian client translations
    └── zh/    # Chinese client translations
```

**Files per language:** ~18 JSON files covering auth, users, permissions, roles, etc.
**Reusability:** HIGH - i18n infrastructure
**Classification:** MUST COPY (all files), CUSTOMIZE (content as needed)

---

### 3.11 Knowledge Base (`knowledge/`)

```
knowledge/
├── admin/     # Admin feature context (EN, ID, ZH)
└── user/      # User workflow context (EN, ID, ZH)
```

**Total files:** 85 markdown files
**Purpose:** AI assistant context documentation
**Reusability:** HIGH - AI context included by default
**Classification:** MUST COPY

---

### 3.12 Documentation (`documentation/`)

```
documentation/
├── docs/
│   ├── index.md
│   ├── getting-started.md
│   ├── architecture.md
│   ├── configuration.md
│   ├── features.md
│   └── api.md
├── mkdocs.yml
└── site/      # Compiled MkDocs output (DO NOT COPY)
```

**Reusability:** HIGH - Documentation structure included
**Classification:** MUST COPY (docs/), DO NOT COPY (site/)

---

### 3.13 Examples (`examples/`)

| File | Purpose | Classification |
|------|---------|----------------|
| `implement_jwt.html` | JWT API testing UI | MUST COPY |
| `test_beepbot.js` | BeepBot testing | MUST COPY |
| `test-encryption.js` | Encryption testing | MUST COPY |
| `enhanced-logging-usage.js` | Logging examples | MUST COPY |
| `stt_test.html` | Speech-to-text test | MUST COPY |

**Reusability:** HIGH - Useful for development and testing
**Classification:** MUST COPY

---

### 3.14 GitHub Workflows (`.github/workflows/`)

| File | Purpose | Classification |
|------|---------|----------------|
| `ci.yml` | Continuous Integration | MUST COPY |
| `cd.yml` | Continuous Deployment | MUST COPY |
| `test.yml` | Test pipeline | OPTIONAL COPY |

**Reusability:** HIGH - CI/CD patterns
**Classification:** MUST COPY (all workflows: ci, cd, test)

---

## 4. Non-Template / Noisy / Risky Items

### 4.1 CRITICAL SECURITY ISSUES

| Item | Risk Level | Issue |
|------|------------|-------|
| `.env` | **CRITICAL** | Contains live database IPs, passwords, API keys. **Currently committed to git!** |
| `.env.prod` | **HIGH** | Production credentials committed |
| `.env.docker` | **MEDIUM** | Docker-specific credentials |
| `.secrets` | **LOW** | Properly git-ignored |

**Immediate Actions Required:**
1. Add `.env` to `.gitignore` (currently commented out: `# .env`)
2. Remove `.env` and `.env.prod` from git history
3. Rotate all exposed credentials immediately

### 4.2 Exposed Credentials in `.env`

```
DB_HOST=100.114.179.76
DB_PASSWORD=rootpassword123
REDIS_HOST=100.127.148.109
REDIS_PASSWORD=Admin12345.
RABBITMQ_HOST=100.127.148.109
RABBITMQ_PASSWORD=Admin12345?.
BEEPBOT_SECRET=braincore
LANGFUSE_PUBLIC_KEY=pk-lf-a9ba9dbf-...
LANGFUSE_SECRET_KEY=sk-lf-eef2aef9-...
```

### 4.3 Runtime/Generated Items (DO NOT COPY)

| Item | Reason |
|------|--------|
| `.git/` | Git repository data |
| `node_modules/` | npm dependencies (install fresh) |
| `logs/` | Runtime log files |
| `logs/app.log` | Application logs |
| `uploads/` | Runtime file uploads |
| `documentation/site/` | Compiled MkDocs output |

### 4.4 Development/Testing Items (DO NOT COPY)

| Item | Reason |
|------|--------|
| `examples/` | Development test files |
| `public/stt_test.html` | Speech-to-text test page |
| `.secrets` | GitHub Actions secrets (local only) |

### 4.5 Machine-Specific Items

| Item | Reason |
|------|--------|
| `flake.lock` | Nix flake lock (regenerate per machine) |
| `package-lock.json` | Can be regenerated (though often kept) |

---

## 5. Bootstrap Classification Summary

### MUST COPY

| Item | Reason |
|------|--------|
| `app.js` | Core Express configuration |
| `server.js` | Application entry point |
| `instrument.js` | OpenTelemetry setup |
| `knexfile.js` | Knex migration config |
| `package.json` | Dependencies and scripts |
| `biome.json` | Code formatting config |
| `ecosystem.config.js` | PM2 configuration |
| `.env.example` | Environment template |
| `.secrets.example` | Secrets template |
| `.gitignore` | Git exclusions (fix first!) |
| `.dockerignore` | Docker exclusions |
| `Dockerfile` | Docker build |
| `docker-compose.dev.yml` | Development services |
| `docker-compose.yml` | Production compose |
| `config/` | Configuration system (all files) |
| `db/db.js` | MySQL connection |
| `db/redis.js` | Redis connection |
| `db/migrations/` | Core migrations (non-AI) |
| `db/seeders/` | Seeder system |
| `helpers/` | Core helpers (13 of 18) |
| `middlewares/` | All 14 middlewares |
| `routes/admin/` | Core admin routes (non-AI) |
| `routes/api/` | Core API routes (non-AI) |
| `routes/client/` | Public routes |
| `routes/index.js` | Main router |
| `views/layout/` | Master layouts |
| `views/components/` | Core components (6 of 8) |
| `views/includes/` | Head and script includes |
| `views/pages/admin/` | Core admin pages (non-AI) |
| `views/pages/client/` | Public pages |
| `public/css/` | Core stylesheets |
| `public/js/` | Core JavaScript libraries |
| `public/img/` | Images (customize logos) |
| `workers/` | Worker system |
| `scripts/` | Utility scripts |
| `locales/` | i18n structure |
| `.github/workflows/ci.yml` | CI pipeline |
| `.github/workflows/cd.yml` | CD pipeline |
| `LICENSE` | MIT License |
| `.env` | Environment configuration (customize credentials) |
| `.env.prod` | Production environment |
| `.env.docker` | Docker environment |
| `CLAUDE.md` | AI assistant documentation (85KB) |
| `knowledge/` | AI context knowledge base |
| `services/` | AI services (chat, analysis) |
| `routes/admin/aiModels/` | AI model routes |
| `routes/admin/aiUseCases/` | AI use case routes |
| `routes/admin/aiAnalysisSettings/` | AI settings routes |
| `routes/api/aiChat/` | AI chat API |
| `routes/api/aiAssistant/` | AI assistant API |
| `routes/api/aiCopilot/` | AI copilot API |
| `views/pages/admin/aiModels/` | AI model pages |
| `views/pages/admin/aiUseCases/` | AI use case pages |
| `views/pages/admin/aiAnalysisSettings/` | AI settings pages |
| `views/pages/chat/` | AI chat page |
| `views/components/aiChatSidebar.njk` | AI chat sidebar |
| `views/components/aiCopilot.njk` | AI copilot component |
| `db/migrations/2026020709*_ai_*.js` | AI migrations |
| `db/seeders/data/aiModels.js` | AI model seeders |
| `db/seeders/data/aiUseCases.js` | AI use case seeders |
| `db/seeders/data/aiAnalysisSettings.js` | AI settings seeders |
| `config/ai-analysis-config.js` | AI analysis config |
| `helpers/langfuse.js` | LLM tracing |
| `helpers/contextLoader.js` | Context loading |
| `helpers/beepbot.js` | External notifications |
| `helpers/uploadToS3.js` | S3 uploads |
| `public/js/chart.js` | Charting library |
| `public/js/marked.js` | Markdown parser |
| `public/css/markdown-content.css` | Markdown styles |
| `examples/` | Example implementations and test files |
| `public/stt_test.html` | Speech-to-text test page |
| `DOCKER.md` | Docker documentation |
| `architecture.puml` | Architecture diagram |
| `flake.nix` | Nix development environment |
| `documentation/docs/` | MkDocs source documentation |
| `.github/workflows/test.yml` | Test pipeline |
| `.secrets` | GitHub Actions secrets template |

### OPTIONAL COPY

| Item | Reason |
|------|--------|
| `README.md` | Project overview (customize for each module) |

### DO NOT COPY

| Item | Reason |
|------|--------|
| `.git/` | Git repository data - initialize fresh |
| `node_modules/` | Dependencies - run `npm install` |
| `logs/` | Runtime logs |
| `uploads/` | Runtime uploads |
| `documentation/site/` | Compiled MkDocs output |
| `package-lock.json` | Regenerate fresh (optional) |
| `flake.lock` | Nix lock file - regenerate |

---

## 6. Files That Need Post-Copy Customization

### 6.1 Package Identity

| File | Customization Needed |
|------|---------------------|
| `package.json` | Change `name`, `description`, `author`, `version` |
| `ecosystem.config.js` | Change `name` from `omniflow-starter` |
| `README.md` | Update project description and instructions |

### 6.2 Branding & UI

| File | Customization Needed |
|------|---------------------|
| `public/img/logo.png` | Replace with module logo |
| `public/img/logomark-blue.svg` | Replace with module logo |
| `views/components/navbar.njk` | Update brand name |
| `views/components/footer.njk` | Update copyright/brand |
| `views/includes/head.njk` | Update page titles |

### 6.3 Configuration

| File | Customization Needed |
|------|---------------------|
| `.env.example` | Update placeholder values |
| `docker-compose.dev.yml` | Update service names, container names |
| `docker-compose.yml` | Update production service names |
| `Dockerfile` | Update labels if needed |

### 6.4 Documentation

| File | Customization Needed |
|------|---------------------|
| `CLAUDE.md` | Update for module-specific context |
| `documentation/docs/*.md` | Update all documentation |
| `documentation/mkdocs.yml` | Update site name |

### 6.5 Database

| File | Customization Needed |
|------|---------------------|
| `db/seeders/data/users.seeder.js` | Update default users |
| `db/seeders/data/permissions.js` | Add module-specific permissions |
| `db/seeders/data/roles.js` | Update role definitions |

### 6.6 i18n

| File | Customization Needed |
|------|---------------------|
| `locales/**/*.json` | Update translations for module |

### 6.7 CI/CD

| File | Customization Needed |
|------|---------------------|
| `.github/workflows/ci.yml` | Update workflow name |
| `.github/workflows/cd.yml` | Update deployment targets |

---

## 7. Minimal Safe Bootstrap Recommendation

For creating a new Omniflow module repository, copy these items:

### Core Files (Required)

```
# Entry Points
server.js
app.js
instrument.js
knexfile.js

# Configuration
package.json          # (then customize)
biome.json
ecosystem.config.js   # (then customize)
.env                  # (then customize credentials)
.env.example
.env.prod             # (then customize)
.env.docker           # (then customize)
.secrets.example
.gitignore
.dockerignore

# Docker
Dockerfile
docker-compose.dev.yml
docker-compose.yml

# Documentation
LICENSE
CLAUDE.md
README.md
DOCKER.md
architecture.puml

# GitHub Actions
.github/workflows/ci.yml
.github/workflows/cd.yml
.github/workflows/test.yml
.secrets
.secrets.example
```

### Core Directories (Required)

```
config/               # All files (including ai-analysis-config.js)
db/
  ├── db.js
  ├── redis.js
  ├── migrations/     # ALL migrations (including AI)
  │   ├── 20250802103717_create_jobs_table.js
  │   ├── 20250804064633_create_roles_table.js
  │   ├── 20250804064756_create_permissions_table.js
  │   ├── 20250804064857_create_role_permissions_table.js
  │   ├── 20250804065100_create_users_table.js
  │   ├── 20250804065200_create_activity_logs_table.js
  │   ├── 20250804065300_create_user_permissions_table.js
  │   └── 2026020709*_create_ai_*.js  # AI migrations included
  └── seeders/
      ├── index.js
      └── data/       # ALL seeders (including AI)
          ├── users.seeder.js
          ├── permissions.js
          ├── roles.js
          ├── rolePermissions.js
          ├── aiModels.js
          ├── aiUseCases.js
          └── aiAnalysisSettings.js
helpers/              # ALL files (including AI helpers)
middlewares/          # All files
routes/               # ALL routes (including AI routes)
services/             # ALL services (including AI services)
views/                # ALL views (including AI views)
public/               # ALL assets (including charts, markdown)
workers/              # All files
scripts/              # All files
locales/              # All translations
knowledge/            # AI context knowledge base
examples/             # Example implementations
documentation/docs/   # MkDocs source documentation
flake.nix             # Nix development environment
.github/workflows/    # ci.yml, cd.yml, test.yml
```

### Estimated Bootstrap Size

- **Core files:** ~60 files
- **Config:** 5 files
- **Database:** 17 files (all migrations + seeders)
- **Helpers:** 18 files (all)
- **Middlewares:** 14 files
- **Routes:** ~106 files (all including AI)
- **Views:** ~44 files (all including AI)
- **Public:** ~22 files (all including charts)
- **Workers:** 3 files
- **Locales:** ~54 files
- **Knowledge:** ~85 files
- **Examples:** 5 files
- **Documentation:** ~7 files (MkDocs source)
- **GitHub Workflows:** 3 files
- **Total:** ~440-470 files

---

## 8. Suggested Exclude List for Future Copy/Sync

Create a `.bootstrap-ignore` or use this pattern for copy operations:

```gitignore
# Git
.git/

# Dependencies
node_modules/

# Secrets (local only)
.secrets
my.secrets
*.secrets

# Runtime
logs/
uploads/
*.log

# Build Output
documentation/site/
dist/
build/
coverage/

# Lock Files (optional - regenerate fresh)
package-lock.json
flake.lock

# OS Files
.DS_Store
Thumbs.db
*.swp
*.swo
*~

# IDE
.idea/
.vscode/
*.sublime-*
```

**Note:** AI features, knowledge base, charts, examples, dan `.env` files sekarang termasuk MUST COPY.

---

## 9. Risks / Uncertainties

### 9.1 Environment Configuration

**Status:** `.env` files are now MUST COPY items.

**Post-Copy Actions:**
1. Update all credentials in `.env` files for target environment
2. Update database host, passwords, API keys to match target infrastructure
3. Generate new secrets for production deployments

**Note:** The `.env` file serves as a working template with example credentials. Each new module should customize these values for their specific environment.

### 9.2 .gitignore Notes

**Current Status:** `.env` files are intentionally tracked as templates.

**Minor Issues to Consider:**
- Duplicate `logs/app.log` entries
- Malformed line: `.env.bakuploads/` (should be separate lines)
- Consider adding common patterns (IDE, OS files) for cleaner repos

**Note:** Since `.env` files are now MUST COPY, they should remain tracked in the starter repo.

### 9.3 AI Feature Integration

**Status:** AI features (chat, copilot, analysis) are now included by default in bootstrap.

All AI-related components are MUST COPY:
- AI migrations and seeders
- AI routes and views
- AI services and helpers
- Knowledge base context

**Note:** If a module doesn't need AI features, they can be removed post-bootstrap.

### 9.4 Knowledge Base

**Status:** `knowledge/` directory (85 markdown files) is now MUST COPY.

**Notes:**
- Provides comprehensive AI context for assistant features
- Can be customized or extended for module-specific documentation
- Multilingual support (EN, ID, ZH)

### 9.5 CLAUDE.md

**Status:** `CLAUDE.md` (85KB) is now MUST COPY.

**Notes:**
- Provides comprehensive context for AI-assisted development
- Should be updated to reflect module-specific features
- Consider splitting into multiple files if size becomes an issue

### 9.6 Package Lock Strategy

**Status:** `package-lock.json` is DO NOT COPY.

**Recommendation:** Regenerate fresh via `npm install` to get latest compatible versions.

**Alternative:** If exact dependency versions are critical, consider copying `package-lock.json`.

---

## 10. Summary

### Repository Health

| Aspect | Status |
|--------|--------|
| Code Quality | ✅ Good - Biome formatting, consistent patterns |
| Architecture | ✅ Good - Clean MVC, modular structure |
| Security | ✅ Good - Credentials included as templates |
| Documentation | ✅ Good - Comprehensive CLAUDE.md |
| CI/CD | ✅ Good - GitHub Actions workflows |
| Testing | ⚠️ Missing - No test framework configured |

### Bootstrap Readiness

| Aspect | Status |
|--------|--------|
| Core Framework | ✅ Ready |
| Auth/RBAC | ✅ Ready |
| Caching/Queue | ✅ Ready |
| Logging | ✅ Ready |
| i18n | ✅ Ready |
| AI Features | ✅ Ready - included by default |
| Environment | ✅ Ready - copy and customize credentials |
| Knowledge Base | ✅ Ready - included by default |
| Examples | ✅ Ready - included by default |

### Post-Bootstrap Actions

1. **Customize `package.json`** - Update name, description, author
2. **Update `.env` files** - Set credentials for target environment
3. **Update branding** - Replace logos and brand names
4. **Update documentation** - Customize README and docs
5. **Run `npm install`** - Install dependencies
6. **Run database setup** - `npm run db:setup`

---

*End of Audit Report*

---

## Related

- [[Architecture Reference]] — definitive architecture conventions
- [[AI Context (CLAUDE)]] — AI agent context for development
- [[Agent Execution]] — build modules from this starter
- [[Starter Template Rebranding]] — rebrand after copying this starter
