# ADR-0002: Local compose scope with contract-first app development
وضعیت: Accepted
تاریخ: 2026-02-13

## Context
در وضعیت فعلی پروژه، compose لوکال فقط زیرساخت پایه (`postgres`, `minio`, `nginx`) را اجرا می کند.
پیاده سازی کامل `api/web` در roadmap است، اما قراردادهای دامنه/API/ops از قبل باید پایدار و ممیزی پذیر باشند.

## Decision
- Compose لوکال فعلا روی زیرساخت پایه متمرکز بماند.
- قراردادهای app در docs و module/app README به شکل contract-first تثبیت شوند.
- تا قبل از اتصال سرویس های app به compose، health و runbookها با این scope همسو بمانند.

## Consequences
### مثبت
- کاهش پیچیدگی اجرایی اولیه
- امکان پیشروی سریع روی governance/contracts/tooling
- کاهش churn در مسیرهای app قبل از تثبیت قراردادها

### منفی
- اجرای end-to-end runtime کامل در compose به فاز بعدی منتقل می شود.

## Implementation References
- `ops/compose.local.yml`
- `docs/DEPLOYMENT/02_Docker_Compose_LocalFirst.md`
- `docs/API_DB_Deployment.md`
- `apps/api/README.md`
- `apps/web/README.md`
