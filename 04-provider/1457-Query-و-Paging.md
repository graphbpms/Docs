---
id: 1457
graphDocId: 1430
title: "Query و Paging"
subTitle: null
module: "04-provider"
wikiPath: "1450/1456/1457"
wikiUrl: "https://wiki.graphbpms.ir/1450/1456/1457"
parentPageUrl: "https://wiki.graphbpms.ir/1450/1456/1457"
breadcrumbs: ["مدیریت منابع داده Provider","Providerهای بانک اطلاعاتی","Query و Paging"]
parentId: 1456
parentTitle: "Providerهای بانک اطلاعاتی"
url: "1457"
priority: 1
language: 1
showInNavigation: true
isStandalonePage: true
summary: "مسیر موضوعی: مدیریت منابع داده Provider > ۲. Providerهای بانک اطلاعاتی. Providerهای بانک اطلاعاتی. GraphBPMS • Provider Query و Paging در این آموزش 6. Provider از نوع Query 7. Paging در Query Provider 6. Provider از نوع Query Query Provider برای واکشی اطلاعات استفاده می‌شود و این نوع Provider برای Queryهای SELECT است؛ دستورات INSERT ، UPDATE و DELETE برای این نوع در نظر گرفته نشده‌اند. برای عملیات تغییردهنده داده، از روش مناسب‌تری مانند StoredProcedure یا ویرایشگر پیشرفته استفاده شود. 6.1. محورهای اصلی: 6. Provider از نوع Query؛ 6.1. ساخت Query؛ 6.2. پارامتر Query؛ 6.3. فیلدهای خروجی؛ 7. Paging در Query Provider؛ 7.1. Paging بدون پارامترهای Query؛ 7.2. Paging مبتنی بر پارامتر."
updatedAt: "2026-10-18T16:08:51"
---

# Query و Paging

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `مدیریت منابع داده Provider` ❯ `Providerهای بانک اطلاعاتی` ❯ `Query و Paging`  
> 🔗 **لینک نسخه برخط:** [https://wiki.graphbpms.ir/1450/1456/1457](https://wiki.graphbpms.ir/1450/1456/1457)

در این آموزش

-   [6\. Provider از نوع Query](#wiki-query-paging-section-01)
-   [7\. Paging در Query Provider](#wiki-query-paging-section-05)

## 6\. Provider از نوع Query

Query Provider برای واکشی اطلاعات استفاده می‌شود و این نوع Provider برای Queryهای `SELECT` است؛ دستورات `INSERT`، `UPDATE` و `DELETE` برای این نوع در نظر گرفته نشده‌اند.

برای عملیات تغییردهنده داده، از روش مناسب‌تری مانند StoredProcedure یا ویرایشگر پیشرفته استفاده شود.

### 6.1. ساخت Query

در مرحله مدیریت Query، QueryBuilder گرافیکی نمایش داده می‌شود. در UI جاری می‌توان Database و Table/View را انتخاب کرد، Fieldها را به Query اضافه کرد، Filter و Sort تعریف کرد، Query تولیدشده را مشاهده و آن را تست کرد.

### 6.2. پارامتر Query

اگر Query حاوی پارامتر باشد، سیستم پارامترها را استخراج کرده و در مرحله «لیست پارامترها» نمایش می‌دهد.

مثال:

```sql
SELECT
    Id,
    CityName
FROM Abfa_City
WHERE StateId = @StateId;
```

پارامتر:

```text
StateId
```

هنگام Test باید مقدار آزمایشی مناسب وارد شود.

### 6.3. فیلدهای خروجی

پس از تست موفق، GraphBPMS Schema خروجی را استخراج می‌کند و فیلدهای Provider ثبت می‌شوند.

مثال:

```text
Id
CityName
StateId
```

## 7\. Paging در Query Provider

تصاویر Query Builder، Paging و خروجی Query

![تنظیم Paging در Query Provider و نمایش نتیجه تست رکوردها.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=EFBDAE16-7759-49E1-9E46-609DAA58C647)

تنظیم Paging در Query Provider و نمایش نتیجه تست رکوردها.

![Query Provider با Paging غیرفعال و نمایش نتیجه تست.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=6714D061-202A-4B0B-9186-191E214C856F)

Query Provider با Paging غیرفعال و نمایش نتیجه تست.

![تنظیمات Paging پارامتری Query پس از پاک‌سازی نشانه‌گذاری دستی.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=B9350C3B-B539-4CDC-AEFA-BD42DCD7AC9A)

تنظیمات Paging پارامتری Query پس از پاک‌سازی نشانه‌گذاری دستی.

![Query Builder و محیط طراحی Query پیشرفته.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=0CBD07E9-9C60-4E22-B429-FD5678E5637A)

Query Builder و محیط طراحی Query پیشرفته.

![مرحله تنظیم/مشاهده فیلدهای خروجی Query.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=9E56203F-2F35-4BBF-A415-A1E955C19020)

مرحله تنظیم/مشاهده فیلدهای خروجی Query.

![Query Builder پس از پاک‌سازی خطوط نشانه‌گذاری دستی.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=6E3F9DFB-2D9B-4C82-94A9-2388570B2068)

Query Builder پس از پاک‌سازی خطوط نشانه‌گذاری دستی.

![صفحه Summary/Details یک Query Provider.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=3CFF76B2-38B9-4754-B84D-110D25682FE3)

صفحه Summary/Details یک Query Provider.

در UI جاری Query Provider تنظیمات اصلی زیر دیده می‌شوند:

-   فعال‌سازی Paging
-   Page Size پیش‌فرض
-   نام ستون کلید
-   استفاده از پارامترها برای Paging
-   پارامتر Page Number
-   پارامتر Page Size
-   پارامتر Global Filter
-   پارامترهای فیلتر ستونی
-   پارامترهای Sort

### 7.1. Paging بدون پارامترهای Query

در این حالت Provider از Paging داخلی استفاده می‌کند و Key Column لازم است.

```text
Paging Enabled: true
Default Page Size: 10
Key Column: Id
```

### 7.2. Paging مبتنی بر پارامتر

در این حالت Query خودش پارامترهای Paging را دریافت می‌کند.

```text
@PageNumber
@PageSize
@GlobalFilter
@FilterColumn
@SortColumn1
@SortValue1
```

نمونه SQL Server:

```sql
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
