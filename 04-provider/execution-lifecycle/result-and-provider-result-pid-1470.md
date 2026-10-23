---
id: 1470
title: "Result و providerResult"
englishTitle: "Result Payloads & Data Contracts"
category: "04-provider/execution-lifecycle"
wikiPath: "1450/1468/1470"
wikiUrl: "https://wiki.graphbpms.ir/1450/1468/1470"
breadcrumbs: ["مدیریت منابع داده Provider","اجرای Provider در کد","Result و providerResult"]
priority: 2
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# Result و providerResult

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `اجرای Provider در کد` ❯ `Result و providerResult`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1468/1470](https://wiki.graphbpms.ir/1450/1468/1470)

در این آموزش

-   [28\. نتیجه معمول Provider](#wiki-result-providerresult-section-01)
-   [29\. Result معمول و Result فایل یکی نیستند](#wiki-result-providerresult-section-03)
-   [43\. سناریو 8 — text/plain](#wiki-result-providerresult-section-04)

## 28\. نتیجه معمول Provider

در نمونه‌های مستند فعلی، Providerهای Structured Data یک Result Model برمی‌گردانند و داده Provider در:

```js
result.Content.providerResult
```

قرار دارد.

این مقدار در نمونه‌های Wiki به‌صورت JSON String است:

```js
absoluteMethodHelper
    .runProvider(PROVIDER_ID)
    .then(result => {
        const rows =
            JSON.parse(result.Content.providerResult);

        console.log(rows);
    })
    .catch(error => {
        console.error(error);
    });
```

برای اولین رکورد:

```js
const firstRow =
    JSON.parse(result.Content.providerResult)[0];
```

### 28.1. نتیجه خالی

**\[راهکار عمومی JavaScript\]**

```js
const rows =
    JSON.parse(
        result.Content.providerResult || '[]'
    );

if (rows.length === 0) {
    console.log('No data');
    return;
}
```

## 29\. Result معمول و Result فایل یکی نیستند

Structured Result:

```text
result
  └── Content
       └── providerResult
            └── JSON string
```

File Result:

```text
result
  └── Content
       └── File
```

برای File نباید `providerResult` را `JSON.parse` کنید.

## 43\. سناریو 8 — `text/plain`

REST Provider می‌تواند Response Content Type از نوع `text/plain` داشته ��اشد.

در Contract جاری، Result متنی داخل:

```text
result.Content.providerResult
```

قرار می‌گیرد و `providerResult` یک JSON String از آرایه Recordها است که برای `text/plain` فیلدی با نام `Result` دارد:

```js
absoluteMethodHelper
    .runProvider(PROVIDER_ID, parameters)
    .then(result => {
        const rows = JSON.parse(
            result.Content.providerResult
        );

        console.log(
            rows[0]?.Result
        );
    })
    .catch(error => {
        console.error(error);
    });
```

اگر متن سرویس مقصد خودش JSON-string باشد، Parse بعدی فقط بر اساس Contract همان سرویس انجام شود.
