---
id: 1460
title: "Advanced Editor"
englishTitle: "Advanced SQL Editor"
category: "04-provider/database-providers"
wikiPath: "1450/1456/1460"
wikiUrl: "https://wiki.graphbpms.ir/1450/1456/1460"
breadcrumbs: ["مدیریت منابع داده Provider","Providerهای بانک اطلاعاتی","Advanced Editor"]
priority: 4
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# Advanced Editor

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `Providerهای بانک اطلاعاتی` ❯ `Advanced Editor`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1456/1460](https://wiki.graphbpms.ir/1450/1456/1460)

در این آموزش

-   [11\. Provider از نوع EnterpriseEditor / ویرایشگر پیشرفته](#wiki-advanced-editor-section-01)
-   [11.1. نمای نهایی ویرایشگر پیشرفته](#wiki-advanced-editor-section-04)

## 11\. Provider از نوع EnterpriseEditor / ویرایشگر پیشرفته

این نوع برای SQLهای پیچیده‌تر استفاده می‌شود.

Editor فعلی از Ace Editor در حالت SQL استفاده می‌کند.

کاربردهای معمول:

-   چند Statement؛
-   Update/Insert/Delete؛
-   شرط‌های پیچیده؛
-   Transaction دستی؛
-   `THROW` و کنترل خطای SQL Server؛
-   خروجی نهایی با `SELECT`.

### 11.1. AutoTransaction

در Provider از نوع **ویرایشگر پیشرفته / EnterpriseEditor** گزینه «فعالسازی Transaction» برای اجرای Batch SQL به‌صورت یکپارچه وجود دارد.

وقتی `AutoTransaction` فعال باشد:

```text
شروع Transaction دیتابیس
        ↓
اجرای تمام Statementهای Batch
        ↓
آیا خطا رخ داده است؟
   ├─ بله → Rollback کل Batch
   └─ خیر → Commit کل Batch
```

بنابراین اگر چند `INSERT`، `UPDATE`، `DELETE` یا Statement دیگر در ویرایشگر پیشرفته وجود داشته باشند، موفقیت فقط بخشی از Batch به‌عنوان نتیجه نهایی پذیرفته نمی‌شود؛ وقوع خطا باعث Rollback کل Batch می‌شود و در اجرای بدون خطا، تغییرات کل Batch Commit می‌شوند.

نوع Transaction در این قابلیت، در حالت پیش‌فرض **Database Transaction** است.

`AutoTransaction` با Transaction دستی داخل متن SQL یکی نیست. برای جلوگیری از پیچیدگی در نگهداری، وقتی Transaction خودکار برای سناریو کافی است، از ایجاد Transaction دستی غیرضروری در همان Batch پرهیز شود.

### 11.2. استخراج پارامترها و نکته `@@`

ویرایشگر پیشرفته پارامترهای SQL را از متن Query استخراج می‌کند. اگر SQL شامل System Variableهای SQL Server مانند:

```sql
@@ROWCOUNT
@@TRANCOUNT
```

باشد، Parser سمت UI ممکن است آن‌ها را شبیه پارامتر Provider تشخیص دهد. در نمونه واقعی ارسالی، `ROWCOUNT` و `TRANCOUNT` در لیست پارامترهای ورودی ظاهر شدند.

بعد از هر تغییر SQL:

1.  لیست Parameters را بررسی کنید.
2.  System Variableهای اشتباه تشخیص‌داده‌شده را اصلاح کنید.
3.  Provider را با داده تستی معتبر اجرا کنید.

## 11.1. نمای نهایی ویرایشگر پیشرفته

در ویرایشگر پیشرفته می‌توان یک Batch SQL کامل را به‌صورت مستقیم تعریف کرد. در صفحه نهایی، متن Query، پارامترهای شناسایی‌شده و فیلدهای خروجی در کنار هم نمایش داده می‌شوند تا قبل از ذخیره بتوان Contract نهایی Provider را کنترل کرد.

![نمای نهایی Provider از نوع ویرایشگر پیشرفته؛ متن Batch SQL، پارامترهای ورودی و فیلدهای خروجی در صفحه جزئیات منبع داده نمایش داده می‌شوند.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=5B86114D-48FC-429A-A416-62CBB46D6A08)

نمای نهایی Provider از نوع ویرایشگر پیشرفته؛ متن Batch SQL، پارامترهای ورودی و فیلدهای خروجی در صفحه جزئیات منبع داده نمایش داده می‌شوند.
