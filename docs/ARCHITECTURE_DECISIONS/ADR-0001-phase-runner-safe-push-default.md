# ADR-0001: Safe-by-default push policy for phase runner
وضعیت: Accepted
تاریخ: 2026-02-13

## Context
`tools/phase-runner/run.sh` قبلا پس از اجرای موفق فاز، branch و tag را بدون opt-in به remote push می کرد.
در محیطی که چند session همزمان روی یک ریپو کار می کنند، این رفتار ریسک تداخل و push ناخواسته ایجاد می کند.

## Decision
- Push پیش فرض phase-runner غیرفعال باشد.
- Push فقط با opt-in صریح انجام شود: `PHASE_RUNNER_PUSH=1`.
- script کمکی `pnpm -w phase:run:push` برای اجرای opt-in اضافه شود.

## Consequences
### مثبت
- کاهش شدید ریسک push ناخواسته
- کنترل بهتر روی زمان/برنچ push
- سازگار با workflow چند session همزمان

### منفی
- یک مرحله اضافی برای push لازم می شود.

## Implementation References
- `tools/phase-runner/run.sh`
- `tools/phase-runner/README.md`
- `package.json`
