# Knowledge Base & Article Platform

## Tech & Structure
- Laravel 11, PHP 8.3, MySQL, Redis, Nginx + PHP-FPM, Pest, PHPStan lvl 8, PHPCS.
- Layers: Controllers → Services → Repos (DB/Eloquent) → Models/DTOs. Policies, Events/Listeners, Jobs, Console Commands.
- Blade for server-rendered UI (accessible by default), Tailwind for quick styling.

## Core Domain
- **Entities:** User, Article, Category, Attachment, Comment, Tag (many-to-many), AuditLog.
- **Roles:** reader (public), writer, editor, admin.
- **States:** draft → in_review → published (enum).

## Routes (high-level)
- **Public:** GET /, GET /articles, GET /articles/{slug}, GET /search?q=…
- **Auth:** GET/POST /login, dashboard GET /me/articles
- **Writer:** CRUD draft articles, upload images.
- **Editor:** approve/publish, category management.
- **API (read-only):** GET /api/articles (paginated, cached), GET /api/articles/{slug}.

## Data Model Highlights
- `articles(slug UNIQUE, title, body, state ENUM, category_id, author_id, published_at, view_count INT DEFAULT 0)`
- **Indexes:** slug, (state, published_at DESC), category_id, author_id, published_at, pivot indexes for tags.
- `attachments(path, mime, size, article_id)` — store files outside web root.
- `audit_logs(user_id, entity, entity_id, action, meta JSON, created_at)`.

## Security & Perf Defaults
- `declare(strict_types=1);`, PSR-12, OPcache on, APP_ENV=prod with errors logged (not displayed).
- CSRF on forms, escape output in Blade, Argon2id password hashing.
- Cookies: HttpOnly, Secure, SameSite=Lax, session ID regen on login.
- Eager load relations, kill N+1, cache heavy lists with Redis.

## 10–12 Day Build Plan (part-time ⇒ 2–3 weeks)

### Day 1 – Scaffold & CI
- Laravel new app, Composer scripts (lint/test/stan).
- Pest + a smoke test, PHPStan lvl 8, PHPCS.
- Docker compose (mysql, redis, queue).
- **Deliverable:** pipeline green, Hello page.

### Day 2 – Auth, Roles, Policies
- Users + seed roles. Policies for Article (create/update/publish).
- Sessions hardened, login/logout tests.

### Day 3–4 – Articles CRUD (+DTO/Service/Repo)
- Migrations + models + factories.
- Service (`ArticleService`) + Repo (`ArticleRepositoryInterface` + Eloquent impl).
- Writer dashboard: create/edit draft, auto-slug, validation.
- Feature tests (create/edit), unit tests for Service.

### Day 5 – Categories, Tags, Comments
- Category admin (editor+), tags pivot, comment moderation.
- Policies for each. Tests.

### Day 6 – Publishing Flow & Events
- States enum: draft→in_review→published.
- **Event:** `ArticleSubmittedForReview` → **Listener** logs audit.
- **Event:** `ArticlePublished` → queue **Job** to email subscribers.
- Transaction around publish. Tests.

### Day 7 – Uploads & Media
- Image/PDF upload: MIME+size validation, random filenames, store outside web root.
- Generate responsive URLs via controller; strip EXIF.
- Tests for upload + access rules.

### Day 8 – Search & Caching
- Simple search (full-text index on title/body or LIKE fallback).
- Cache: homepage lists + category pages (Redis, 5–15 min, tags invalidate on publish).
- Increment `view_count` via atomic Redis counter flushed nightly.

### Day 9 – Scheduler & Maintenance
- **Schedule** nightly: reindex search, flush hot counts to DB, prune stale drafts.
- Queue worker via Supervisor config.

### Day 10 – Logging/Observability & Rate Limits
- Monolog JSON to stdout; add request + DB timing in context.
- Rate limit search and auth routes.
- Accessibility pass (labels, focus, contrast). Lighthouse check.

### Day 11 – Docs, Seeds, Polishing
- Realistic seeders (10 categories, 60 articles, 150 comments).
- README: setup, decisions, tradeoffs, demo user creds, ERD image, screenshots.
- Public “About this KB” page for recruiters (what you built & why).

### Day 12 – Deploy
- Build: `composer install --no-dev --optimize-autoloader`, `route:cache`, `config:cache`.
- Migrate + seed on deploy, queue worker, cron.
- Smoke tests in CI against staging DB.
