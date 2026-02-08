# Engineering Playbook — PatreonLiteIran
نسخه: 1.2
تاریخ: 2026-02-08

این سند برای اجرای MVP با برش‌های قابل لانچ و جلوگیری از افزایش Scope نوشته شده است و با فازهای P0..P3 هماهنگ است.

---

## 1) نقشه راه فازها (Phases)
- P0: Foundation & Governance + ابزارها + compose + CI + dashboards
- P1: Payments/Membership + Admin Troubleshooting (حداقل)
- P2: Assets protection + Search + SEO baseline
- P3: Admin Panel کامل + Payouts + Analytics

منبع حقیقت: `tools/phase-runner/phases.json`

---

## 2) Definition of Done (واحد)
مرجع: `docs/DEVELOPMENT/Definition_of_Done.md`

حداقل DoD برای هر PR/فاز:
- کد: lint + typecheck
- تست: unit (و برای مسیر حساس integration/e2e)
- لاگ: correlation_id + error code
- امنیت: security:scan + عدم وجود secret
- مستندات: docs update + docs:validate
- عملیات: rollback plan (برای migration/پرداخت/تسویه)

---

## 3) Gates (Quality Gates)
گیت‌های استاندارد:
- `pnpm -w docs:validate`
- `pnpm -w lint`
- `pnpm -w typecheck`
- `pnpm -w test:unit`
- `pnpm -w test:integration` (برای P1+)
- `pnpm -w test:e2e` (برای P2+)
- `pnpm -w security:scan`

اجرای خودکار: `./tools/phase-runner/run.sh <PHASE>`

---

## 4) هماهنگی با داشبوردها
- داشبورد توسعه: `ops/dashboards/development-dashboard.html`
- داشبورد دیپلوی: `ops/dashboards/deploy-dashboard.html`

این داشبوردها از:
- `tools/phase-runner/phases.json`
- `snapshots/<PHASE>/manifest.json`
استفاده می‌کنند.

---

## 5) سیاست Local-first
- runtime بدون CDN/سرویس خارجی
- self-host fonts
- MinIO private + signed URL

---

## 6) خروجی هر فاز
هر فاز باید snapshot داشته باشد:
- `snapshots/<PHASE>/manifest.json`
- `snapshots/<PHASE>/report.md`
و tag ایجاد شود.

پایان.
