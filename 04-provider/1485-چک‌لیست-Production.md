---
id: 1485
graphDocId: 1541
title: "چک‌لیست Production"
subTitle: null
module: "04-provider"
wikiPath: "1450/1480/1485"
wikiUrl: "https://wiki.graphbpms.ir/1450/1480/1485"
parentPageUrl: "https://wiki.graphbpms.ir/1450/1480/1485"
breadcrumbs: ["مدیریت منابع داده Provider","طراحی معماری و Production","چک‌لیست Production"]
parentId: 1480
parentTitle: "طراحی معماری و Production"
url: "1485"
priority: 5
language: 1
showInNavigation: true
isStandalonePage: true
summary: "مسیر موضوعی: مدیریت منابع داده Provider > ۶. طراحی معماری و Production. طراحی معماری و Production. GraphBPMS • Provider چک‌لیست Production در این آموزش 78. قبل از انتشار 78. قبل از انتشار [ ] عنوان Provider معنادار است. [ ] Provider ID بین محیط‌ها مدیریت شده است. [ ] Query فقط Fieldهای لازم را برمی‌گرداند. [ ] Secret در Client وجود ندارد. [ ] Input Parameters معتبر و کنترل‌شده‌اند. [ ] دسترسی ایجاد/ویرایش Provider و دسترسی Form/SubForm بررسی شده است. [ ] ProviderLimitations در System Settings و web.config بازبینی شده است. محورهای اصلی: 78. قبل از انتشار."
updatedAt: "2026-10-18T16:08:51"
---

# چک‌لیست Production

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `مدیریت منابع داده Provider` ❯ `طراحی معماری و Production` ❯ `چک‌لیست Production`  
> 🔗 **لینک نسخه برخط:** [https://wiki.graphbpms.ir/1450/1480/1485](https://wiki.graphbpms.ir/1450/1480/1485)

در این آموزش

-   [78\. قبل از انتشار](#wiki-production-checklist-section-01)

## 78\. قبل از انتشار

-   \[ \] عنوان Provider معنادار است.
-   \[ \] Provider ID بین محیط‌ها مدیریت شده است.
-   \[ \] Query فقط Fieldهای لازم را برمی‌گرداند.
-   \[ \] Secret در Client وجود ندارد.
-   \[ \] Input Parameters معتبر و کنترل‌شده‌اند.
-   \[ \] دسترسی ایجاد/ویرایش Provider و دسترسی Form/SubForm بررسی شده است.
-   \[ \] `ProviderLimitations` در System Settings و `web.config` بازبینی شده است.
-   \[ \] Paging برای Dataset بزرگ فعال است.
-   \[ \] REST Timeout و رفتار Endpoint روی استقرار مقصد تست شده است.
-   \[ \] File Output دارای Header مناسب است.
-   \[ \] Error Handling با `.catch` وجود دارد.
-   \[ \] Provider بدون رکورد Crash نمی‌کند.
-   \[ \] Nullable Output تست شده است.
-   \[ \] `@@ROWCOUNT`/`@@TRANCOUNT` در Advanced Editor بازبینی شده‌اند.
-   \[ \] در صورت فعال بودن `isProviderParamLogActive`، حساسیت داده‌های ثبت‌شده در `ProviderLog` پذیرفته و کنترل شده است.
-   \[ \] WSDL/REST Endpoint Production مشخص است.
-   \[ \] Test Data از Production Secret استفاده نمی‌کند.
-   \[ \] Command Provider از نظر Transaction و Partial Failure تست شده است.
-   \[ \] File → File در صورت استفاده End-to-End تست شده است.
