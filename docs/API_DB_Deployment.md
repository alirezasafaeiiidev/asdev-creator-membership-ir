API/DB/Deployment — PatreonLiteIran
نسخه: 2.2
تاریخ: 2026-02-08

این سند نسخه‌به‌روزشده‌ی طراحی API، استانداردهای دیتابیس و استقرار Local-first است و با:
- `docs/ARCHITECTURE/03_API_Standards.md`
- `docs/ARCHITECTURE/04_Database_Standards.md`
- `docs/DEPLOYMENT/*`
هماهنگ است.

---

## بخش 1 — API Design
### 1.1 Base URL و نسخه‌بندی
- Base: `/api/v1`
- پاسخ‌ها envelope استاندارد داشته باشند.
- errorها فقط با Error Codeهای رسمی.

### 1.2 Auth & User
- `POST /api/v1/auth/register`
- `POST /api/v1/auth/login`
- `POST /api/v1/auth/logout`
- `POST /api/v1/auth/refresh`
- `GET  /api/v1/me`
- `PATCH /api/v1/me`

### 1.3 Creator (Public SEO + Creator profile)
- `POST /api/v1/creators/apply`
- `GET  /api/v1/creators/:username`  (public SEO)
- `PATCH /api/v1/creators/me`

### 1.4 Tiers
- `POST /api/v1/tiers`
- `GET  /api/v1/creators/:username/tiers` (public)
- `PATCH /api/v1/tiers/:id`
- `DELETE /api/v1/tiers/:id`

### 1.5 Payments
- `POST /api/v1/payments/create`
- `POST /api/v1/payments/callback`
- (admin) `GET /api/v1/admin/payments` (search/filter)
- (admin) `GET /api/v1/admin/payments/:id/events`

قواعد:
- callback باید idempotent باشد (gateway_ref unique)
- payment_events قبل از verify ذخیره شوند.

### 1.6 Memberships
- `GET  /api/v1/memberships/me`
- (internal) ایجاد/تمدید membership پس از پرداخت موفق

### 1.7 Posts & Assets (Protected Content)
- `POST /api/v1/posts`
- `GET  /api/v1/creators/:username/posts` (public list با رعایت visibility)
- `GET  /api/v1/posts/:id` (public/authorized)
- `POST /api/v1/assets` (upload metadata; upload flow در MVP ساده نگه داشته شود)
- `GET  /api/v1/assets/:id/download` (signed URL + gate + rate limit)

### 1.8 Search (MVP با Postgres)
- `GET /api/v1/public/search/creators?q=...&page=...`
- `GET /api/v1/public/search/posts?q=...&page=...&sort=createdAt:desc`

### 1.9 Admin Analytics (MVP)
- `GET /api/v1/admin/analytics/payments?from=...&to=...`
- `GET /api/v1/admin/analytics/assets?from=...&to=...`
- `GET /api/v1/admin/analytics/payouts?from=...&to=...`

(در MVP می‌تواند Query مستقیم از DB/audit_logs باشد.)

### 1.10 Health
- `GET /api/v1/health/live`
- `GET /api/v1/health/ready` (DB + MinIO)

---

## بخش 2 — Database (PostgreSQL)
### 2.1 موجودیت‌های حیاتی
- users, creator_profiles, tiers
- memberships
- payments, payment_events
- posts, assets
- payouts, payout_items
- audit_logs

### 2.2 Constraints کلیدی
- creator_profiles.username UNIQUE
- payments.gatewayRef UNIQUE
- payout_items.paymentId UNIQUE (جلوگیری از دوباره‌شماری)
- memberships: جلوگیری از duplicate active per (userId, tierId)

### 2.3 Indexها (حداقلی)
- payments(status, createdAt)
- payment_events(gatewayRef, receivedAt)
- audit_logs(action, createdAt)
- search:
  - trigram روی creator username
  - FTS روی post title/body

---

## بخش 3 — Deployment (Local-first)
### 3.1 Compose
- Local: Postgres + MinIO + Nginx + api + web
- Stage: نزدیک prod
- Prod: TLS + secrets خارج repo + backup قبل migration

### 3.2 ممنوعیت‌ها (runtime)
- CDN/analytics خارجی
- سرویس‌های third-party برای storage یا auth (در MVP)

پایان سند.
