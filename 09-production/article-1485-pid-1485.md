---
id: 1485
title: "چک‌لیست Production"
englishTitle: "چک‌لیست Production"
category: "09-production"
wikiUrl: "https://wiki.graphbpms.ir/1485"
breadcrumbs: ["چک‌لیست Production"]
priority: 5
updatedAt: "2026-10-25T21:33:55"
---

# چک‌لیست Production

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `چک‌لیست Production`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1485](https://wiki.graphbpms.ir/1485)

# چک‌لیست Production
در این آموزش
- [78. قبل از انتشار](#wiki-production-checklist-section-01)

## 78. قبل از انتشار

- [ ] عنوان Provider معنادار است.

- [ ] Provider ID بین محیط‌ها مدیریت شده است.

- [ ] Query فقط Fieldهای لازم را برمی‌گرداند.

- [ ] Secret در Client وجود ندارد.

- [ ] Input Parameters معتبر و کنترل‌شده‌اند.

- [ ] دسترسی ایجاد/ویرایش Provider و دسترسی Form/SubForm بررسی شده است.

- [ ] `ProviderLimitations` در System Settings و `web.config` بازبینی شده است.

- [ ] Paging برای Dataset بزرگ فعال است.

- [ ] REST Timeout و رفتار Endpoint روی استقرار مقصد تست شده است.

- [ ] File Output دارای Header مناسب است.

- [ ] Error Handling با `.catch` وجود دارد.

- [ ] Provider بدون رکورد Crash نمی‌کند.

- [ ] Nullable Output تست شده است.

- [ ] `@@ROWCOUNT`/`@@TRANCOUNT` در Advanced Editor بازبینی شده‌اند.

- [ ] در صورت فعال بودن `isProviderParamLogActive`، حساسیت داده‌های ثبت‌شده در `ProviderLog` پذیرفته و کنترل شده است.

- [ ] WSDL/REST Endpoint Production مشخص است.

- [ ] Test Data از Production Secret استفاده نمی‌کند.

- [ ] Command Provider از نظر Transaction و Partial Failure تست شده است.

- [ ] File → File در صورت استفاده End-to-End تست شده است.
