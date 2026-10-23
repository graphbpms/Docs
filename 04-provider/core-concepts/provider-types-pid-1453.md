---
id: 1453
title: "انواع Provider"
englishTitle: "Provider Types & Connectors"
category: "04-provider/core-concepts"
wikiPath: "1450/1451/1453"
wikiUrl: "https://wiki.graphbpms.ir/1450/1451/1453"
breadcrumbs: ["مدیریت منابع داده Provider","آشنایی و شروع کار","انواع Provider"]
priority: 2
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# انواع Provider

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `آشنایی و شروع کار` ❯ `انواع Provider`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1451/1453](https://wiki.graphbpms.ir/1450/1451/1453)

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
