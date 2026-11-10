---
id: 1453
title: "انواع Provider"
englishTitle: "Organization Chart & Roles"
category: "05-system-management/organization-structure"
wikiUrl: "https://wiki.graphbpms.ir/1453"
breadcrumbs: ["انواع Provider"]
priority: 2
updatedAt: "2026-10-25T21:33:55"
---

# انواع Provider

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `انواع Provider`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1453](https://wiki.graphbpms.ir/1453)

# انواع Provider
در این آموزش
- [3. طبقه‌بندی انواع منبع داده](#wiki-provider-types-section-01)

## 3. طبقه‌بندی انواع منبع داده

### 3.1. منابع مبتنی بر بانک اطلاعاتی

نوع
کاربرد اصلی

`Query`
واکشی داده با Query از نوع Select

`StoredProcedure`
اجرای Stored Procedure موجود در DB

`Function`
اجرای Function موجود در DB

`OptionalOptions`
تعریف داده‌های اختیاری/ثابت قابل استفاده مجدد

`EnterpriseEditor` / ویرایشگر پیشرفته
SQL پیشرفته و سناریوهای پیچیده‌تر

`SystemicEntity` / موجودیت سیستم
ساخت Provider از موجودیت‌های داخلی مانند Form/SubForm

### 3.2. منابع مبتنی بر Web Service

نوع
کاربرد اصلی

`WSDL`
استفاده از Web Serviceهای SOAP/WSDL از قبل تفسیرشده

`REST`
فراخوانی Endpointهای REST با Method، Header، Body، File و Response

در ماژول فعلی، خانواده‌های اصلی Database Provider و Web Service Provider در کنار `SystemicEntity` در دسترس هستند.
