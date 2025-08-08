# Project Checklist

## Initial Setup
- [ ] Create GitHub repository with README and license
- [ ] Initialize Laravel 11 project
- [ ] Set PHP version to 8.3 in `composer.json`
- [ ] Add `declare(strict_types=1);` to PHP files by default
- [ ] Configure `.env.example` with MySQL, Redis, and queue settings
- [ ] Create `.editorconfig` for consistent formatting
- [ ] Install Pest, PHPStan (level 8), and PHPCS
- [ ] Set up Docker Compose (mysql, redis, queue worker, nginx + php-fpm)
- [ ] Configure CI pipeline (GitHub Actions) for linting, static analysis, and tests

## Core Structure
- [ ] Create folder structure: Controllers, Services, Repositories, DTOs, Events, Listeners, Jobs, Policies
- [ ] Set up PSR-4 autoloading
- [ ] Install Tailwind CSS for Blade templates

## Authentication & Roles
- [ ] Create User model and migrations
- [ ] Seed roles: reader, writer, editor, admin
- [ ] Implement Laravel policies for Article CRUD
- [ ] Add session security (HttpOnly, Secure, SameSite)

## Articles Module
- [ ] Create Article model, migration, and factory
- [ ] Create ArticleService and ArticleRepository
- [ ] Implement CRUD for draft articles
- [ ] Add slug generation
- [ ] Add validation rules
- [ ] Write feature and unit tests

## Categories, Tags, Comments
- [ ] Create Category model and CRUD
- [ ] Create Tag model and pivot table
- [ ] Create Comment model with moderation
- [ ] Implement related policies and tests

## Publishing Workflow
- [ ] Create enum for article states: draft, in_review, published
- [ ] Create events: ArticleSubmittedForReview, ArticlePublished
- [ ] Create listeners for logging and email jobs
- [ ] Wrap publish flow in transactions

## File Uploads
- [ ] Implement secure upload handling (MIME, size validation, random names)
- [ ] Store files outside web root
- [ ] Serve files via controller with access checks
- [ ] Strip EXIF data from images

## Search & Caching
- [ ] Implement basic search (full-text index or LIKE)
- [ ] Cache homepage and category lists in Redis
- [ ] Invalidate cache on publish
- [ ] Increment view_count in Redis, flush nightly

## Scheduler & Maintenance
- [ ] Schedule nightly reindex
- [ ] Flush view counts to DB
- [ ] Prune stale drafts
- [ ] Configure Supervisor for queue worker

## Logging & Security
- [ ] Configure Monolog JSON output
- [ ] Add request & DB timing
- [ ] Apply rate limits to search and auth routes
- [ ] Run accessibility checks

## Deployment
- [ ] Set up VPS or Laravel Forge environment
- [ ] Configure zero-downtime deploy
- [ ] Run `composer install --no-dev --optimize-autoloader`
- [ ] Cache routes and config
- [ ] Migrate and seed database
- [ ] Verify queues and scheduled tasks are running

