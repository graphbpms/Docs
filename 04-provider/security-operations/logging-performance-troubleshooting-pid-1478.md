---
id: 1478
title: "Logging، Performance و Troubleshooting"
englishTitle: "Diagnostics, Logging & Performance Tuning"
category: "04-provider/security-operations"
wikiPath: "1450/1474/1478"
wikiUrl: "https://wiki.graphbpms.ir/1450/1474/1478"
breadcrumbs: ["مدیریت منابع داده Provider","امنیت، دسترسی و عملیات","Logging، Performance و Troubleshooting"]
priority: 4
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# Logging، Performance و Troubleshooting

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `امنیت، دسترسی و عملیات` ❯ `Logging، Performance و Troubleshooting`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1474/1478](https://wiki.graphbpms.ir/1450/1474/1478)

در این آموزش

-   [61\. Provider Parameter Logging](#wiki-ops-troubleshooting-section-01)
-   [62\. از SELECT \* در Production پرهیز کنید](#wiki-ops-troubleshooting-section-02)
-   [63\. Paging برای Dataset بزرگ](#wiki-ops-troubleshooting-section-03)
-   [64\. Provider در onLoad](#wiki-ops-troubleshooting-section-04)
-   [65\. Parameter تعریف شده اما مقدار نمی‌گیرد](#wiki-ops-troubleshooting-section-05)
-   [66\. JSON.parse خطا می‌دهد](#wiki-ops-troubleshooting-section-06)
-   [67\. File ارسال نمی‌شود](#wiki-ops-troubleshooting-section-07)
-   [68\. File Output به شکل مورد انتظار Download نمی‌شود](#wiki-ops-troubleshooting-section-08)
-   [69\. ROWCOUNT یا TRANCOUNT به‌عنوان Parameter ظاهر شده](#wiki-ops-troubleshooting-section-09)
-   [70\. Paging نتیجه اشتباه می‌دهد](#wiki-ops-troubleshooting-section-10)

## 61\. Provider Parameter Logging

کلید زیر در `appSettings` فایل `web.config` وضعیت ثبت Log اجرای Provider را کنترل می‌کند:

```xml
<add
    key="isProviderParamLogActive"
    value="true"
/>
```

وقتی مقدار `true` باشد، اجرای Providerها و پارامترهای مربوط به هر اجرا به‌صورت کامل در جدول:

```text
ProviderLog
```

ثبت می‌شوند.

این قابلیت برای Audit و Debug مفید است، اما از منظر امنیت باید جدی گرفته شود. اگر Parameter شامل Token، Password، کد ملی، داده شخصی یا اطلاعات قراردادی باشد، همان داده می‌تواند وارد Log شود.

Constant/Default Parameter نیز Secret Vault نیست؛ بنابراین طراحی Provider و Policy نگهداری Log باید با فرض حساس بودن `ProviderLog` انجام شود.

در این سند Schema ستون‌های `ProviderLog` یا Retention Policy فرض نشده‌اند، چون برای توضیح قابلیت اصلی لازم نیستند و ممکن است به Deployment وابسته باشند.

## 62\. از `SELECT *` در Production پرهیز کنید

**\[راهکار عمومی SQL\]**

به‌جای:

```sql
SELECT *
FROM LargeTable;
```

فقط Fieldهای لازم:

```sql
SELECT
    Id,
    Title,
    StatusId
FROM LargeTable;
```

مزایا:

-   Network کمتر؛
-   Serialization کمتر؛
-   Schema تمیزتر؛
-   احتمال افشای داده حساس کمتر.

## 63\. Paging برای Dataset بزرگ

اگر Provider برای Grid، Report یا Search است و رکورد زیاد دارد، Paging فعال شود.

نشانه‌های نیاز:

-   Table بزرگ؛
-   Load زمان‌بر؛
-   Grid با تعداد رکورد زیاد؛
-   REST Service کند؛
-   مصرف زیاد Browser Memory.

## 64\. Provider در `onLoad`

Call سنگین در `onLoad` UX فرم را خراب می‌کند.

در صورت امکان:

-   فقط داده ضروری دریافت شود؛
-   Paging استفاده شود؛
-   Call تکراری حذف شود؛
-   Query و Indexهای Database بهینه باشند.

## 65\. Parameter تعریف شده اما مقدار نمی‌گیرد

بررسی کنید:

1.  `name` دقیقاً برابر Parameter باشد.
2.  اشتباه تایپی یا اختلاف نام وجود نداشته باشد.
3.  Parameter در Query واقعاً استفاده شده باشد.
4.  Constant Value در REST رفتار مورد نظر را تغییر نداده باشد.
5.  Parameter مخصوص Paging را دوباره در لیست عادی ارسال نکرده باشید.

## 66\. `JSON.parse` خطا می‌دهد

ممکن است Provider:

-   File برگرداند؛
-   خروجی Structured نداشته باشد؛
-   `providerResult` خالی باشد؛
-   `text/plain` باشد.

برای Structured Result:

```js
const rows =
    JSON.parse(
        result.Content.providerResult || '[]'
    );
```

ولی ابتدا نوع خروجی Provider را بررسی کنید.

## 67\. File ارسال نمی‌شود

بررسی کنید:

-   مقدار واقعاً `File` باشد؛
-   Blob خام نباشد؛
-   REST Content Type برابر `multipart/form-data` باشد؛
-   Parameter Type برابر `File` باشد؛
-   Position برابر `form-data` باشد.

Blob → File:

```js
const file =
    new File(
        [blob],
        'my-file.bin'
    );
```

## 68\. File Output به شکل مورد انتظار Download نمی‌شود

Helper فعلی File را وقتی تشخیص می‌دهد که Response Header شامل:

```text
Content-Disposition: attachment
```

باشد.

Endpoint و Network Response را بررسی کنید.

## 69\. `ROWCOUNT` یا `TRANCOUNT` به‌عنوان Parameter ظاهر شده

اگر Advanced Editor از:

```sql
@@ROWCOUNT
@@TRANCOUNT
```

استفاده می‌کند، Parser UI ممکن است آن‌ها را Parameter تشخیص دهد.

لیست Parameters را قبل از Save بازبینی کنید.

## 70\. Paging نتیجه اشتباه می‌دهد

کنترل کنید:

-   Page Number / Index مبنا؛
-   Page Size؛
-   Key Column؛
-   GlobalFilter Mapping؛
-   Filter Parameter Mapping؛
-   SortColumn / SortValue Pair؛
-   نام Parameterها؛
-   `ORDER BY` معتبر؛
-   منطق Page Count/Total در Provider مقصد.
