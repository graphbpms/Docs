---
id: 1473
title: "Server-Side Script و مثال‌های آموزشی"
englishTitle: "Server-Side Scripts & Integration Patterns"
category: "04-provider/execution-lifecycle"
wikiPath: "1450/1468/1473"
wikiUrl: "https://wiki.graphbpms.ir/1450/1468/1473"
breadcrumbs: ["مدیریت منابع داده Provider","اجرای Provider در کد","Server-Side Script و مثال‌های آموزشی"]
priority: 5
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# Server-Side Script و مثال‌های آموزشی

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `اجرای Provider در کد` ❯ `Server-Side Script و مثال‌های آموزشی`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1468/1473](https://wiki.graphbpms.ir/1450/1468/1473)

در این آموزش

-   [51\. فراخوانی Provider در C#](#wiki-server-examples-section-01)
-   [52\. دریافت شهرهای یک استان](#wiki-server-examples-section-04)
-   [53\. اجرای Command بدون خروجی](#wiki-server-examples-section-05)
-   [54\. Upload فایل به REST](#wiki-server-examples-section-06)
-   [55\. Download فایل](#wiki-server-examples-section-07)
-   [56\. Upload + Process + Download](#wiki-server-examples-section-08)

## 51\. فراخوانی Provider در C#

در Server-Side Script مستند GraphBPMS:

```cs
var Parameters =
    new Dictionary<string,string>();

Parameters.Add(
    "ProviderId",
    "1"
);

Parameters.Add(
    "ParameterName1",
    "ParameterValue1"
);

Parameters.Add(
    "ParameterName2",
    "ParameterValue2"
);

var RunProviderResult =
    (DataTable)Functions["RunProvider"]
        .DynamicInvoke(Parameters);

var username =
    (string)RunProviderResult
        .Rows[0]["UserName"];
```

### 51.1. چه زمانی Server-Side مناسب‌تر است؟

-   منطق حساس است؛
-   نمی‌خواهید تصمیم یا Secret در JavaScript Client قرار گیرد؛
-   عملیات بخشی از Lifecycle Server فرم است؛
-   داده قبل/بعد از Save باید سمت Server کنترل شود.

### 51.2. محدودیت مستند

نمونه فعلی Server-Side `Dictionary<string,string>` را نشان می‌دهد.

در API مستند فعلی Server-Side، نمونه `RunProvider` از `Dictionary<string,string>` استفاده می‌کند و مسیر ارسال `File` با همین API مستند نشده است. بنابراین رفتار File در `absoluteMethodHelper.runProvider` سمت Client را به این API Server-Side تعمیم ندهید.

## 52\. دریافت شهرهای یک استان

Query:

```sql
SELECT
    Id,
    CityName
FROM Abfa_City
WHERE StateId = @StateId
ORDER BY CityName;
```

Custom JS:

```js
function getCities(stateId) {
    return absoluteMethodHelper
        .runProvider(
            PROVIDER_ID,
            [
                {
                    name: 'StateId',
                    value: stateId
                }
            ]
        )
        .then(result => {
            return JSON.parse(
                result.Content.providerResult || '[]'
            );
        });
}
```

## 53\. اجرای Command بدون خروجی

```js
function updateRequest(requestId) {
    return absoluteMethodHelper
        .runProvider(
            PROVIDER_ID,
            [
                {
                    name: 'RequestId',
                    value: requestId
                }
            ]
        )
        .then(result => {
            toast.show(
                'success',
                'عملیات انجام شد'
            );

            return result;
        })
        .catch(error => {
            console.error(error);
        });
}
```

## 54\. Upload فایل به REST

```js
function uploadFile() {
    return global
        .getFileFromUser({
            accept: '*'
        })
        .then(file => {
            return absoluteMethodHelper.runProvider(
                PROVIDER_ID,
                [
                    {
                        name: 'file',
                        value: file
                    },
                    {
                        name: 'category',
                        value: 'document'
                    }
                ]
            );
        })
        .then(result => {
            console.log(result);
        });
}
```

## 55\. Download فایل

```js
function downloadFile(documentId) {
    return absoluteMethodHelper
        .runProvider(
            PROVIDER_ID,
            [
                {
                    name: 'documentId',
                    value: documentId
                }
            ]
        )
        .then(result => {
            const file = result.Content;

            saveAs(
                file,
                file.name
            );
        });
}
```

## 56\. Upload + Process + Download

```js
function convertFile() {
    return global
        .getFileFromUser({
            accept: '*'
        })
        .then(inputFile => {
            return absoluteMethodHelper.runProvider(
                PROVIDER_ID,
                [
                    {
                        name: 'file',
                        value: inputFile
                    },
                    {
                        name: 'targetFormat',
                        value: 'pdf'
                    }
                ]
            );
        })
        .then(result => {
            const convertedFile = result.Content;

            saveAs(
                convertedFile,
                convertedFile.name
            );
        })
        .catch(error => {
            console.error(error);
        });
}
```
