<div align="center">

<img src="assets/banner.png" alt="Nova Worker — Cloudflare Workers proxy management" width="100%">

<a href="https://github.com/IRNova/NovaWorker"><img src="https://img.shields.io/badge/Project-Nova%20Worker-111827?style=for-the-badge&logo=cloudflare&logoColor=F38020" alt="Nova Worker"></a>
<img src="https://img.shields.io/badge/Version-Beta--0.0.1-7c3aed?style=for-the-badge" alt="Beta 0.0.1">
<img src="https://img.shields.io/badge/License-GPL--2.0-0891b2?style=for-the-badge" alt="GPL v2">
<img src="https://img.shields.io/badge/Languages-FA%20%7C%20EN%20%7C%20RU-0e7490?style=for-the-badge" alt="Languages">

# Nova Worker

**یک Worker و پنل مدیریت پروکسی بر پایه Cloudflare Workers**

[وب‌سایت پنل](https://nova-panel.github.io) · [گزارش مشکل](https://github.com/IRNova/NovaWorker/issues) · [گفت‌وگو](https://t.me/irnova_group)

</div>

<div align="center">
  <img src="assets/animations/nova-pulse.svg" alt="Nova Worker animated identity" width="82%">
</div>

---

## ✨ درباره پروژه

**Nova Worker** یک Worker برای ارائه و مدیریت سرویس پروکسی روی زیرساخت Cloudflare Workers است. این پروژه همراه با پنل مدیریتی مستقل ارائه می‌شود؛ پنل بازطراحی‌شده از سه زبان **فارسی (RTL)، انگلیسی و روسی** پشتیبانی می‌کند.

هدف این مخزن، نگهداری کد Worker و فراهم‌کردن مسیر نصب، پیکربندی و اتصال آن به پنل Nova است.

> وضعیت انتشار فعلی: **Beta-0.0.1** — آبان ۱۴۰۵

## 🧭 معماری در یک نگاه

<div align="center">
  <img src="assets/animations/network-flow.svg" alt="Client to Nova Worker to Internet animated flow" width="100%">
</div>

- **Client:** کلاینت سازگار، از طریق لینک اشتراک به پیکربندی دسترسی پیدا می‌کند.
- **Nova Worker:** درخواست‌ها و مسیرهای تعریف‌شده را مدیریت می‌کند.
- **Cloudflare KV:** محل نگهداری تنظیمات و داده‌های مدیریتی.
- **Nova Panel:** رابط مدیریت مستقل که از طریق Worker در دسترس قرار می‌گیرد.

## 📦 مخزن‌های پروژه

| بخش | مخزن | آدرس |
| --- | --- | --- |
| Worker | [IRNova/NovaWorker](https://github.com/IRNova/NovaWorker) | دامنه Worker شما |
| پنل مدیریت | [Nova-Panel/nova-panel.github.io](https://github.com/Nova-Panel/nova-panel.github.io) | [nova-panel.github.io](https://nova-panel.github.io) |

## 🚀 قابلیت‌ها

- اجرای Worker روی **Cloudflare Workers**.
- اتصال به فضای ذخیره‌سازی **Cloudflare KV** با بایندینگ `KV`.
- پنل مدیریت سه‌زبانه: فارسی، انگلیسی و روسی.
- مسیرهای راه‌اندازی اولیه، ورود مدیر و مدیریت.
- ارائه اشتراک با قالب‌های Clash، Sing-box و Mixed.
- پشتیبانی از `ASSETS` یا `PANEL_URL` برای اتصال پنل.
- کش لبه برای صفحه‌ها و فایل‌های استاتیک پنل.
- نگهداری تنظیمات و داده‌های مدیریتی در KV.
- کوکی نشست امن با ویژگی‌های `HttpOnly`، `Secure` و `SameSite=Lax`.

*این فهرست بر اساس توضیحات و مستندات نسخه فعلی مخزن تهیه شده است؛ رفتار نهایی را با نسخه منتشرشده کد بررسی کنید.*

## 🧰 پیش‌نیازها

- حساب Cloudflare با دسترسی Workers
- یک فضای KV در Cloudflare
- Node.js و Wrangler برای انتشار از خط فرمان (اختیاری)

## ⚙️ نصب و راه‌اندازی

### ۱. ساخت فضای KV

```bash
wrangler kv namespace create KV
```

شناسه‌ای را که دستور برمی‌گرداند، در فایل `wrangler.toml` قرار دهید:

```toml
name = "nova-worker"
main = "_worker.js"
compatibility_date = "2025-11-04"
keep_vars = true

[[kv_namespaces]]
binding = "KV"
id = "<KV_ID>"
```

**مهم:** نام بایندینگ باید دقیقاً `KV` باشد.

### ۲. انتشار Worker

در ریشه پروژه اجرا کنید:

```bash
wrangler deploy
```

### ۳. راه‌اندازی اولیه

پس از انتشار، آدرس زیر را در مرورگر باز کنید:

```text
https://<worker-domain>/install
```

رمز مدیریت را تعیین کنید. رمز در KV ذخیره می‌شود و ورودهای بعدی از مسیر `/login` انجام می‌شوند.

### ۴. نصب از داشبورد Cloudflare

اگر Wrangler را استفاده نمی‌کنید:

1. در داشبورد Cloudflare یک Worker بسازید.
2. محتوای فایل `_worker.js` را در ویرایشگر قرار دهید.
3. یک KV binding با نام `KV` اضافه کنید.
4. Worker را Deploy کنید.
5. مسیر `/install` را باز کنید و راه‌اندازی اولیه را انجام دهید.

## 🔌 اتصال پنل مدیریت

Worker به‌صورت پیش‌فرض پنل را از نشانی زیر دریافت می‌کند:

```text
https://nova-panel.github.io
```

برای تغییر منبع پنل دو روش وجود دارد:

1. **بایندینگ `ASSETS`:** برای زمانی که پنل و Worker در یک پروژه Cloudflare Pages قرار دارند؛ این روش اولویت بالاتری دارد.
2. **متغیر `PANEL_URL`:** برای زمانی که پنل جداگانه منتشر شده است.

نمونه تنظیم متغیر:

```toml
[vars]
PANEL_URL = "https://nova-panel.github.io"
```

مسیرهای `/admin`، `/login` و `/install` از پنل خوانده می‌شوند. فایل‌های استاتیک پنل نیز از دامنه Worker پروکسی می‌شوند. اگر پنل در دسترس نباشد، Worker صفحه محلی با پیام سه‌زبانه نمایش می‌دهد.

### کش و عملکرد

| نوع پاسخ | مدت کش |
| --- | --- |
| صفحه‌های HTML پنل | ۵ دقیقه |
| لوگو، فونت، Leaflet، QR و manifest | ۱ ساعت |
| مسیرهای داده مانند `config.json`، `log.json`، `check`، `getADDAPI` و `getCloudflareUsage` | بدون کش |

در مستندات پروژه، برای درخواست‌های تکراری از کش لبه Cloudflare استفاده شده است تا نیاز به دریافت مجدد صفحه‌ها و فایل‌های استاتیک کاهش یابد. مسیرهای داده بدون کش و مستقیم از KV پاسخ می‌گیرند.

## 🧩 متغیرهای محیطی

متغیرها اختیاری هستند و می‌توان آن‌ها را در `wrangler.toml` یا بخش **Settings → Variables** داشبورد Cloudflare تعریف کرد.

| متغیر | کاربرد |
| --- | --- |
| `ADMIN` | رمز مدیریت؛ جایگزین رمز ذخیره‌شده در KV |
| `KEY` | کلید رمزنگاری کوکی و لینک‌ها |
| `UUID` | UUID ثابت کاربر به‌جای مقدار مشتق‌شده از رمز |
| `HOST` | دامنه یا دامنه‌های Worker برای ساخت لینک اشتراک |
| `PANEL_URL` | آدرس پنل مدیریت |
| `PROXYIP` | آدرس پروکسی جایگزین؛ یک یا چند مقدار |
| `URL` | آدرس صفحه پوششی؛ `nginx` یا `1101` برای حالت‌های آماده |
| `PATH` | مسیر سفارشی برای اتصال‌ها |
| `OFF_LOG` | با مقدار `1` ثبت گزارش‌ها غیرفعال می‌شود |
| `BEST_SUB` | با مقدار `1` اشتراک بهینه برای کلاینت‌های خاص فعال می‌شود |
| `DEBUG` | با مقدار `1` لاگ‌های تشخیصی چاپ می‌شوند |
| `GO2SOCKS5` | دامنه‌های اضافه برای عبور از SOCKS5 |
| `PRELOAD_RACE_DIAL` | با مقدار `1` پیش‌بارگذاری و مسابقه اتصال فعال می‌شود |
| `PROXY_CONCURRENT_DIAL` | تعداد اتصال هم‌زمان به پروکسی |
| `TCP_CONCURRENT_DIAL` | تعداد اتصال هم‌زمان TCP |

تنظیمات تفصیلی پروتکل‌ها، نودها، مبدل، تلگرام و Cloudflare از پنل مدیریت انجام می‌شود و در KV نگهداری می‌گردد.

## 🛣️ مسیرهای Worker

| مسیر | کاربرد |
| --- | --- |
| `/admin` | پنل مدیریت؛ نیازمند ورود |
| `/login` | ورود با رمز مدیریت |
| `/install` | راه‌اندازی اولیه |
| `/sub` | دریافت اشتراک با قالب Clash، Sing-box یا Mixed |
| `/version` | نسخه عمومی Worker و شماره Build |
| `/robots.txt` | جلوگیری از ایندکس‌شدن |

## 🔐 نکات امنیتی

- برای حساب مدیریت، رمز قوی و منحصربه‌فرد انتخاب کنید.
- مقدار پیش‌فرض `KEY` را در محیط واقعی تغییر دهید.
- کلیدها و توکن‌ها را در مخزن Git قرار ندهید.
- مقادیر محرمانه را فقط در تنظیمات امن Cloudflare یا KV نگهداری کنید.
- پنل با `noindex, nofollow` منتشر می‌شود.
- کوکی ورود از ویژگی‌های `HttpOnly`، `Secure` و `SameSite=Lax` استفاده می‌کند.
- نشست پس از ۳۰ دقیقه بی‌فعالیتی نامعتبر می‌شود.
- مسیرهای داده مدیریتی بدون نشست معتبر باید پاسخ `401` بدهند.

## 🔄 نسخه و تغییرات

پنل اطلاعات نسخه و تغییرات را از فایل‌های زیر در شاخه `main` دریافت می‌کند:

- [`_worker.js`](https://raw.githubusercontent.com/IRNova/NovaWorker/main/_worker.js)
- [`CHANGELOG`](https://raw.githubusercontent.com/IRNova/NovaWorker/main/CHANGELOG)

بنابراین فایل‌های `_worker.js` و `CHANGELOG` باید در ریشه شاخه `main` باشند. فهرست تغییرات نسخه‌های پیشین پروژه بالادست در `CHANGELOG.upstream` نگهداری می‌شود.

## 🌐 شبکه‌های اجتماعی و ارتباط با Nova

<div align="center">

[![Telegram Channel](https://img.shields.io/badge/Telegram-Channel-229ED9?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/irnova)
[![Telegram Group](https://img.shields.io/badge/Telegram-Community-229ED9?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/irnova_group)
[![YouTube](https://img.shields.io/badge/YouTube-Channel-FF0033?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/@novaproxyir)
[![X](https://img.shields.io/badge/X-Follow-111111?style=for-the-badge&logo=x&logoColor=white)](https://x.com/irNovaProxy)
[![Instagram](https://img.shields.io/badge/Instagram-Follow-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://www.instagram.com/irnova_team)

</div>

- تلگرام: [@irnova](https://t.me/irnova)
- گروه کاربران: [irnova_group](https://t.me/irnova_group)
- یوتیوب: [Nova Proxy IR](https://www.youtube.com/@novaproxyir)
- ایکس: [@irNovaProxy](https://x.com/irNovaProxy)
- اینستاگرام: [@irnova_team](https://www.instagram.com/irnova_team)

## 📜 لایسنس و اعتبار

این پروژه با نام **Nova Worker** در مخزن [IRNova/NovaWorker](https://github.com/IRNova/NovaWorker) منتشر می‌شود و تحت لایسنس **GNU GPL v2** ارائه شده است. متن کامل لایسنس باید در فایل `LICENSE` مخزن موجود باشد. حقوق پروژه بالادست و مشارکت‌کنندگان آن محفوظ است.

پنل مدیریت Nova Worker به‌صورت جداگانه در مخزن [Nova-Panel/nova-panel.github.io](https://github.com/Nova-Panel/nova-panel.github.io) نگهداری می‌شود.

---

<div align="center">

**ساخته‌شده برای تجربه‌ای ساده‌تر در مدیریت Nova Worker**

<sub>Nova Worker · Beta-0.0.1 · IRNova</sub>

</div>
