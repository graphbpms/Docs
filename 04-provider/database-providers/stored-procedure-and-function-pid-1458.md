---
id: 1458
title: "StoredProcedure و Function"
englishTitle: "Stored Procedures & Functions"
category: "04-provider/database-providers"
wikiPath: "1458"
wikiUrl: "https://wiki.graphbpms.ir/1458"
breadcrumbs: ["StoredProcedure و Function"]
priority: 2
isCanonical: true
updatedAt: "2026-10-25T21:33:55"
---

# StoredProcedure و Function

> [!NOTE]
> 🧭 **سلسلهمراتب موضوعی:** `StoredProcedure و Function`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1458](https://wiki.graphbpms.ir/1458)

# StoredProcedure و Function
در این آموزش
- [8. Provider از نوع StoredProcedure](#wiki-sp-function-section-01)
- [9. Provider از نوع Function](#wiki-sp-function-section-04)

## 8. Provider از نوع StoredProcedure

در این نوع Provider یک Stored Procedure موجود در Database انتخاب می‌شود. Stored Procedure می‌تواند پارامتر ورودی داشته باشد، عملیات خواندن یا تغییر داده انجام دهد و Result Set برگرداند.

### 8.1. مراحل

- عنوان Provider را مشخص کنید.

- Database Connection را انتخاب کنید.

- Stored Procedure را انتخاب کنید.

- GraphBPMS پارامترهای Procedure را دریافت می‌کند.

- برای تست، مقدار پارامترها را وارد کنید.

- Procedure تست می‌شود.

- Schema خروجی استخراج می‌شود.

- Provider ذخیره می‌شود.

### 8.2. Paging در StoredProcedure

در UI فعلی Paging StoredProcedure از طریق پارامترهای خود Procedure پیکربندی می‌شود.

پارامترهای معمول:

```
PageSize
PageNumber
GlobalFilter
FilterColumn
SortColumn
SortValue

```

اگر Procedure پارامتر مناسب نداشته باشد، Paging پارامتری قابل تعریف نیست.

## 9. Provider از نوع Function

تصاویر StoredProcedure و Function Providerایجاد Provider از نوع Function و انتخاب Database پایه.تنظیمات Paging و Test در StoredProcedure Provider.مرحله اطلاعات پایه StoredProcedure Provider.خروجی فیلدهای StoredProcedure پس از Test.صفحه Summary یک StoredProcedure Provider.مرحله انتخاب Stored Procedure موجود در Database.

برای استفاده از Function موجود در Database، نوع Function انتخاب می‌شود.

### 9.1. ترتیب پارامترها

در Function Provider پارامترها دارای اولویت/ترتیب هستند. تمام پارامترها باید Priority داشته باشند و Priority تکراری نداشته باشند. این موضوع برای Functionهایی که ترتیب آرگومان‌ها در اجرای آن‌ها اهمیت دارد ضروری است.
