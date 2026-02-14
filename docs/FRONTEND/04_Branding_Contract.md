# Branding Contract
نسخه: 1.0

## هدف
هم‌راستاسازی برند محصول با برند مادر ASDEV در لایه‌های UI، SEO و مسیر ارتباط تجاری.

## اصول الزام‌آور
- برند مادر: `ASDEV`
- مالک برند: `Alireza Safaei`
- هر سطح public-facing (وب اپ، لندینگ، داشبورد عمومی) باید خط Attribution پایدار داشته باشد.
- یک مسیر مستقل برند الزامی است: `/brand` یا `/about-brand`.

## قرارداد UI
- Footer روی صفحات public باید شامل این خط باشد:
  - `Built by Alireza Safaei (ASDEV)` یا معادل فارسی.
- لینک Attribution باید قابل کلیک باشد و به صفحه برند داخلی متصل شود.
- صفحه برند باید حداکثر با 2 کلیک از صفحه اصلی قابل دسترس باشد.

## قرارداد SEO
- صفحه برند باید دارای `title` و `description` یکتا باشد.
- canonical صفحه برند باید تنظیم شود.
- `sitemap` و `robots` باید صفحه برند را پوشش دهند.
- JSON-LD از نوع `Person` یا `Organization` باید در صفحه برند وجود داشته باشد.

## معیار پذیرش
- Footer attribution در desktop/mobile قابل مشاهده باشد.
- مسیر `/brand` یا `/about-brand` در build production فعال باشد.
- تست یا چک CI برای وجود route برند و metadata پایه فعال باشد.
