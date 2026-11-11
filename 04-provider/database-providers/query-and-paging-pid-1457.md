---
id: 1457
title: "Query و Paging"
englishTitle: "SQL Queries & Grid Paging"
category: "04-provider/database-providers"
wikiPath: "1457"
wikiUrl: "https://wiki.graphbpms.ir/1457"
breadcrumbs: ["مدیریت منابع داده Provider","Providerهای بانک اطلاعاتی","Query و Paging"]
priority: 1
isCanonical: true
updatedAt: "2026-10-25T21:33:55"
---

# Query و Paging

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `Providerهای بانک اطلاعاتی` ❯ `Query و Paging`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1457](https://wiki.graphbpms.ir/1457)

# Query و Paging
در این آموزش
- [6. Provider از نوع Query](#wiki-query-paging-section-01)
- [7. Paging در Query Provider](#wiki-query-paging-section-05)

## 6. Provider از نوع Query

Query Provider برای واکشی اطلاعات استفاده می‌شود و این نوع Provider برای Queryهای `SELECT` است؛ دستورات `INSERT`، `UPDATE` و `DELETE` برای این نوع در نظر گرفته نشده‌اند.

برای عملیات تغییردهنده داده، از روش مناسب‌تری مانند StoredProcedure یا ویرایشگر پیشرفته استفاده شود.

### 6.1. ساخت Query

در مرحله مدیریت Query، QueryBuilder گرافیکی نمایش داده می‌شود. در UI جاری می‌توان Database و Table/View را انتخاب کرد، Fieldها را به Query اضافه کرد، Filter و Sort تعریف کرد، Query تولیدشده را مشاهده و آن را تست کرد.

### 6.2. پارامتر Query

اگر Query حاوی پارامتر باشد، سیستم پارامترها را استخراج کرده و در مرحله «لیست پارامترها» نمایش می‌دهد.

مثال:

```
SELECT
    Id,
    CityName
FROM Abfa_City
WHERE StateId = @StateId;

```

پارامتر:

```
StateId

```

هنگام Test باید مقدار آزمایشی مناسب وارد شود.

### 6.3. فیلدهای خروجی

پس از تست موفق، GraphBPMS Schema خروجی را استخراج می‌کند و فیلدهای Provider ثبت می‌شوند.

مثال:

```
Id
CityName
StateId

```

## 7. Paging در Query Provider

تصاویر Query Builder، Paging و خروجی Queryتنظیم Paging در Query Provider و نمایش نتیجه تست رکوردها.Query Provider با Paging غیرفعال و نمایش نتیجه تست.تنظیمات Paging پارامتری Query پس از پاک‌سازی نشانه‌گذاری دستی.Query Builder و محیط طراحی Query پیشرفته.مرحله تنظیم/مشاهده فیلدهای خروجی Query.Query Builder پس از پاک‌سازی خطوط نشانه‌گذاری دستی.صفحه Summary/Details یک Query Provider.

در UI جاری Query Provider تنظیمات اصلی زیر دیده می‌شوند:

- فعال‌سازی Paging

- Page Size پیش‌فرض

- نام ستون کلید

- استفاده از پارامترها برای Paging

- پارامتر Page Number

- پارامتر Page Size

- پارامتر Global Filter

- پارامترهای فیلتر ستونی

- پارامترهای Sort

### 7.1. Paging بدون پارامترهای Query

در این حالت Provider از Paging داخلی استفاده می‌کند و Key Column لازم است.

```
Paging Enabled: true
Default Page Size: 10
Key Column: Id

```

### 7.2. Paging مبتنی بر پارامتر

در این حالت Query خودش پارامترهای Paging را دریافت می‌کند.

```
@PageNumber
@PageSize
@GlobalFilter
@FilterColumn
@SortColumn1
@SortValue1

```

نمونه SQL Server:

```
SELECT
    Id,
    CityName,
    StateId
FROM Abfa_City
WHERE
    (
        @GlobalFilter IS NULL
        OR CityName LIKE N'%' + @GlobalFilter + N'%'
    )
ORDER BY Id
OFFSET ((@PageNumber - 1) * @PageSize) ROWS
FETCH NEXT @PageSize ROWS ONLY;

```

مبنای Page Number/Index باید با قرارداد Provider مقصد تست شود؛ API Client فعلی `runProviderPaging` از `pageIndex` استفاده می‌کند و نمونه تولیدشده مقدار `0` را برای صفحه اول می‌فرستد.
