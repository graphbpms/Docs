---
id: 1458
graphDocId: 1473
title: "StoredProcedure و Function"
subTitle: null
module: "04-provider"
wikiPath: "1450/1456/1458"
wikiUrl: "https://wiki.graphbpms.ir/1450/1456/1458"
parentPageUrl: "https://wiki.graphbpms.ir/1450/1456/1458"
breadcrumbs: ["مدیریت منابع داده Provider","Providerهای بانک اطلاعاتی","StoredProcedure و Function"]
parentId: 1456
parentTitle: "Providerهای بانک اطلاعاتی"
url: "1458"
priority: 2
language: 1
showInNavigation: true
isStandalonePage: true
summary: "مسیر موضوعی: مدیریت منابع داده Provider > ۲. Providerهای بانک اطلاعاتی. Providerهای بانک اطلاعاتی. GraphBPMS • Provider StoredProcedure و Function در این آموزش 8. Provider از نوع StoredProcedure 9. Provider از نوع Function 8. Provider از نوع StoredProcedure در این نوع Provider یک Stored Procedure موجود در Database انتخاب می‌شود. Stored Procedure می‌تواند پارامتر ورودی داشته باشد، عملیات خواندن یا تغییر داده انجام دهد و Result Set برگرداند. 8.1. مراحل عنوان Provider را مشخص کنید. Database Connection را انتخاب کنید. محورهای اصلی: 8. Provider از نوع StoredProcedure؛ 8.1. مراحل؛ 8.2. Paging در StoredProcedure؛ 9. Provider از نوع Function؛ 9.1. ترتیب پارامترها."
updatedAt: "2026-10-18T16:08:51"
---

# StoredProcedure و Function

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `مدیریت منابع داده Provider` ❯ `Providerهای بانک اطلاعاتی` ❯ `StoredProcedure و Function`  
> 🔗 **لینک نسخه برخط:** [https://wiki.graphbpms.ir/1450/1456/1458](https://wiki.graphbpms.ir/1450/1456/1458)

در این آموزش

-   [8\. Provider از نوع StoredProcedure](#wiki-sp-function-section-01)
-   [9\. Provider از نوع Function](#wiki-sp-function-section-04)

## 8\. Provider از نوع StoredProcedure

در این نوع Provider یک Stored Procedure موجود در Database انتخاب می‌شود. Stored Procedure می‌تواند پارامتر ورودی داشته باشد، عملیات خواندن یا تغییر داده انجام دهد و Result Set برگرداند.

### 8.1. مراحل

1.  عنوان Provider را مشخص کنید.
2.  Database Connection را انتخاب کنید.
3.  Stored Procedure را انتخاب کنید.
4.  GraphBPMS پارامترهای Procedure را دریافت می‌کند.
5.  برای تست، مقدار پارامترها را وارد کنید.
6.  Procedure تست می‌شود.
7.  Schema خروجی استخراج می‌شود.
8.  Provider ذخیره می‌شود.

### 8.2. Paging در StoredProcedure

در UI فعلی Paging StoredProcedure از طریق پارامترهای خود Procedure پیکربندی می‌شود.

پارامترهای معمول:

```text
PageSize
PageNumber
GlobalFilter
FilterColumn
SortColumn
SortValue
```

اگر Procedure پارامتر مناسب نداشته باشد، Paging پارامتری قابل تعریف نیست.

## 9\. Provider از نوع Function

تصاویر StoredProcedure و Function Provider

![ایجاد Provider از نوع Function و انتخاب Database پایه.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=A95B97D2-2A7B-4AE2-B9F9-252FBB0EA350)

ایجاد Provider از نوع Function و انتخاب Database پایه.

![تنظیمات Paging و Test در StoredProcedure Provider.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=3BC19D5F-B524-48A1-AE5C-FECE910EB83E)

تنظیمات Paging و Test در StoredProcedure Provider.

![مرحله اطلاعات پایه StoredProcedure Provider.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=820E945E-5C61-43C0-B426-FA9AC6655FFE)

مرحله اطلاعات پایه StoredProcedure Provider.

![خروجی فیلدهای StoredProcedure پس از Test.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=80E2980C-2D57-4D5E-9566-C00EC882D737)

خروجی فیلدهای StoredProcedure پس از Test.

![صفحه Summary یک StoredProcedure Provider.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=C98E4B15-8FDD-4CAD-8C38-1593959C0C0B)

صفحه Summary یک StoredProcedure Provider.

![مرحله انتخاب Stored Procedure موجود در Database.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=5E96C91A-72F0-43F4-9905-BF5A04C206B8)

مرحله انتخاب Stored Procedure موجود در Database.

برای استفاده از Function موجود در Database، نوع Function انتخاب می‌شود.

### 9.1. ترتیب پارامترها

در Function Provider پارامترها دارای اولویت/ترتیب هستند. تمام پارامترها باید Priority داشته باشند و Priority تکراری نداشته باشند. این موضوع برای Functionهایی که ترتیب آرگومان‌ها در اجرای آن‌ها اهمیت دارد ضروری است.
