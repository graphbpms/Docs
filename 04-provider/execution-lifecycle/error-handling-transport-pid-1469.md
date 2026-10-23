---
id: 1469
title: "absoluteMethodHelper.runProvider و قرارداد Parameter"
englishTitle: "Error Handling & Transport Pipeline"
category: "04-provider/execution-lifecycle"
wikiPath: "1450/1468/1469"
wikiUrl: "https://wiki.graphbpms.ir/1450/1468/1469"
breadcrumbs: ["مدیریت منابع داده Provider","اجرای Provider در کد","absoluteMethodHelper.runProvider و قرارداد Parameter"]
priority: 1
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# absoluteMethodHelper.runProvider و قرارداد Parameter

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `اجرای Provider در کد` ❯ `absoluteMethodHelper.runProvider و قرارداد Parameter`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1468/1469](https://wiki.graphbpms.ir/1450/1468/1469)

در این آموزش

-   [25\. absoluteMethodHelper.runProvider](#wiki-runprovider-section-01)
-   [26\. ساده‌ترین فراخوانی — Provider بدون پارامتر](#wiki-runprovider-section-03)
-   [27\. Provider دارای پارامتر](#wiki-runprovider-section-04)
-   [35\. خلاصه سناریوها](#wiki-runprovider-section-05)
-   [36\. سناریو 1 — بدون ورودی، دارای خروجی](#wiki-runprovider-section-06)
-   [37\. سناریو 2 — دارای ورودی، دارای خروجی](#wiki-runprovider-section-07)
-   [38\. سناریو 3 — بدون ورودی و بدون خروجی](#wiki-runprovider-section-08)
-   [39\. سناریو 4 — دارای ورودی، بدون خروجی](#wiki-runprovider-section-10)

## 25\. `absoluteMethodHelper.runProvider`

امضای فعلی:

```js
absoluteMethodHelper.runProvider(providerId, parameters = [])
```

`providerId` شناسه Provider است.

`parameters` آرایه‌ای از Objectها با ساختار زیر است:

```js
[
    {
        name: 'ParameterName',
        value: someValue
    }
]
```

### 25.1. چرا این API مرجع است؟

در نمونه‌های رسمی Wiki و در Sample Code تولیدشده توسط صفحه Provider Details، برای Provider غیر Paging از:

```js
absoluteMethodHelper.runProvider(...)
```

استفاده می‌شود.

از فراخوانی مستقیم Endpoint داخلی `/Provider/RunProvider` در Custom JS خودداری کنید، مگر آنکه برای Build مقصد API رسمی مستقلی مستند شده باشد.

## 26\. ساده‌ترین فراخوانی — Provider بدون پارامتر

```js
absoluteMethodHelper
    .runProvider(PROVIDER_ID)
    .then(result => {
        console.log(result);
    })
    .catch(error => {
        console.error(error);
    });
```

پارامتر دوم Optional است و مقدار پیش‌فرض آن `[]` است. بنابراین برای Provider بدون ورودی نیازی به آرایه خالی نیست.

فرم پیشنهادی:

```js
runProvider(PROVIDER_ID)
```

به‌جای:

```js
runProvider(PROVIDER_ID, [])
```

## 27\. Provider دارای پارامتر

```js
const parameters = [
    {
        name: 'UserId',
        value: 25
    },
    {
        name: 'Status',
        value: 1
    }
];

absoluteMethodHelper
    .runProvider(PROVIDER_ID, parameters)
    .then(result => {
        console.log(result);
    })
    .catch(error => {
        console.error(error);
    });
```

نام `name` باید با Parameter تعریف‌شده در Provider تطبیق داشته باشد.

## 35\. خلاصه سناریوها

| سناریو | فراخوانی/رفتار |
| --- | --- |
| بدون ورودی + خروجی Structured | `runProvider(PROVIDER_ID)` و Parse کردن `providerResult` |
| دارای ورودی + خروجی Structured | `runProvider(PROVIDER_ID, parameters)` |
| بدون ورودی + بدون خروجی | `runProvider(PROVIDER_ID)`؛ از Parse اجباری خروجی خودداری شود |
| دارای ورودی + بدون خروجی | `runProvider(PROVIDER_ID, parameters)`؛ Success/Error مدیریت شود |
| File Input | مقدار Parameter باید `File` باشد |
| File Output | فایل در `result.Content` |
| File Input + File Output | FormData در Request و Attachment در Response |
| `text/plain` | این نوع خروجی پشتیبانی می‌شود؛ Contract Result در بخش مربوط به `text/plain` توضیح داده شده است |
| Paging | نمونه جاری محصول: `providerDesigner.runProviderPaging(...)` |

## 36\. سناریو 1 — بدون ورودی، دارای خروجی

Provider مفهومی:

```sql
SELECT
    Id,
    UserName
FROM UsersTable;
```

Custom JS:

```js
absoluteMethodHelper
    .runProvider(PROVIDER_ID)
    .then(result => {
        const rows =
            JSON.parse(
                result.Content.providerResult || '[]'
            );

        console.log(rows);
    })
    .catch(error => {
        console.error(error);
    });
```

کاربرد:

-   دریافت Lookup؛
-   دریافت تنظیمات؛
-   نمایش لیست عمومی؛
-   اجرای Query بدون Filter.

اگر Table شامل اطلاعات امنیتی یا Credential است فقط ستون‌های مورد نیاز را Select کنید و از `SELECT *` در مثال Production پرهیز کنید.

## 37\. سناریو 2 — دارای ورودی، دارای خروجی

```sql
SELECT
    Id,
    CityName
FROM Abfa_City
WHERE StateId = @StateId;
```

```js
absoluteMethodHelper
    .runProvider(
        PROVIDER_ID,
        [
            {
                name: 'StateId',
                value: 1
            }
        ]
    )
    .then(result => {
        const rows =
            JSON.parse(
                result.Content.providerResult || '[]'
            );

        console.log(rows);
    })
    .catch(error => {
        console.error(error);
    });
```

## 38\. سناریو 3 — بدون ورودی و بدون خروجی

این حالت برای Command ساده ممکن است.

نمونه واقعی آزمایشگاهی ارسالی:

```sql
UPDATE GB_RequestHireForm
SET StatusRequest = 4;
```

مشخصات Provider:

```text
Input Parameters: ندارد
Output Fields: ندارد
```

Custom JS:

```js
absoluteMethodHelper
    .runProvider(PROVIDER_ID)
    .then(result => {
        console.log(result);
    })
    .catch(error => {
        console.error(error);
    });
```

### نکته بسیار مهم

در این حالت نباید فرض کنید:

```js
result.Content.providerResult
```

حتماً Array قابل Parse است. Provider خروجی Business Data ندارد؛ موفقیت یا خطای اجرای عملیات را مدیریت کنید.

## 39\. سناریو 4 — دارای ورودی، بدون خروجی

مثال مفهومی:

```sql
UPDATE Requests
SET StatusId = @StatusId
WHERE Id = @RequestId;
```

```js
absoluteMethodHelper
    .runProvider(
        PROVIDER_ID,
        [
            {
                name: 'RequestId',
                value: requestId
            },
            {
                name: 'StatusId',
                value: statusId
            }
        ]
    )
    .then(result => {
        console.log(
            'Provider executed successfully',
            result
        );
    })
    .catch(error => {
        console.error(error);
    });
```

برای عملیات حساس، بهتر است SQL نتیجه Business Operation را صریح برگرداند:

```sql
UPDATE Requests
SET StatusId = @StatusId
WHERE Id = @RequestId;

SELECT
    CAST(1 AS bit) AS IsSuccess,
    @@ROWCOUNT AS UpdatedCount;
```

سپس Client می‌تواند تعداد رکورد تغییرکرده را نیز کنترل کند.
