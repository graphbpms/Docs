---
id: 1453
graphDocId: 1472
title: "انواع Provider"
subTitle: null
module: "04-provider"
wikiPath: "1450/1451/1453"
wikiUrl: "https://wiki.graphbpms.ir/1450/1451/1453"
parentPageUrl: "https://wiki.graphbpms.ir/1450/1451/1453"
breadcrumbs: ["مدیریت منابع داده Provider","آشنایی و شروع کار","انواع Provider"]
parentId: 1451
parentTitle: "آشنایی و شروع کار"
url: "1453"
priority: 2
language: 1
showInNavigation: true
isStandalonePage: true
summary: "مسیر موضوعی: مدیریت منابع داده Provider > ۱. آشنایی و شروع کار. آشنایی و شروع کار. GraphBPMS • Provider انواع Provider در این آموزش 3. طبقه‌بندی انواع منبع داده 3. طبقه‌بندی انواع منبع داده 3.1. منابع مبتنی بر بانک اطلاعاتی نوع کاربرد اصلی Query واکشی داده با Query از نوع Select StoredProcedure اجرای Stored Procedure موجود در DB Function اجرای Function موجود در DB OptionalOptions تعریف داده‌های اختیاری/ثابت قابل استفاده مجدد EnterpriseEditor / ویرایشگر پیشرفته SQL پیشرفته و سناریوهای پیچیده‌تر…. محورهای اصلی: 3. طبقه‌بندی انواع منبع داده؛ 3.1. منابع مبتنی بر بانک اطلاعاتی؛ 3.2. منابع مبتنی بر Web Service."
updatedAt: "2026-10-18T16:08:51"
---

# انواع Provider

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `مدیریت منابع داده Provider` ❯ `آشنایی و شروع کار` ❯ `انواع Provider`  
> 🔗 **لینک نسخه برخط:** [https://wiki.graphbpms.ir/1450/1451/1453](https://wiki.graphbpms.ir/1450/1451/1453)

در این آموزش

-   [3\. طبقه‌بندی انواع منبع داده](#wiki-provider-types-section-01)

## 3\. طبقه‌بندی انواع منبع داده

### 3.1. منابع مبتنی بر بانک اطلاعاتی

| نوع | کاربرد اصلی |
| --- | --- |
| `Query` | واکشی داده با Query از نوع Select |
| `StoredProcedure` | اجرای Stored Procedure موجود در DB |
| `Function` | اجرای Function موجود در DB |
| `OptionalOptions` | تعریف داده‌های اختیاری/ثابت قابل استفاده مجدد |
| `EnterpriseEditor` / ویرایشگر پیشرفته | SQL پیشرفته و سناریوهای پیچیده‌تر |
| `SystemicEntity` / موجودیت سیستم | ساخت Provider از موجودیت‌های داخلی مانند Form/SubForm |

### 3.2. منابع مبتنی بر Web Service

| نوع | کاربرد اصلی |
| --- | --- |
| `WSDL` | استفاده از Web Serviceهای SOAP/WSDL از قبل تفسیرشده |
| `REST` | فراخوانی Endpointهای REST با Method، Header، Body، File و Response |

در ماژول فعلی، خانواده‌های اصلی Database Provider و Web Service Provider در کنار `SystemicEntity` در دسترس هستند.
