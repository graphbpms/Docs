---
id: 1470
title: "Result و providerResult"
englishTitle: "Result Payloads & Data Contracts"
category: "04-provider/execution-lifecycle"
wikiPath: "1470"
wikiUrl: "https://wiki.graphbpms.ir/1470"
breadcrumbs: ["Result و providerResult"]
priority: 2
isCanonical: true
updatedAt: "2026-10-25T21:33:55"
---

# Result و providerResult

> [!NOTE]
> 🧭 **سلسلهمراتب موضوعی:** `Result و providerResult`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1470](https://wiki.graphbpms.ir/1470)

# Result و providerResult
در این آموزش
- [28. نتیجه معمول Provider](#wiki-result-providerresult-section-01)
- [29. Result معمول و Result فایل یکی نیستند](#wiki-result-providerresult-section-03)
- [43. سناریو 8 — text/plain](#wiki-result-providerresult-section-04)

## 28. نتیجه معمول Provider

در نمونه‌های مستند فعلی، Providerهای Structured Data یک Result Model برمی‌گردانند و داده Provider در:

```
result.Content.providerResult

```

قرار دارد.

این مقدار در نمونه‌های Wiki به‌صورت JSON String است:

```
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

```
const firstRow =
    JSON.parse(result.Content.providerResult)[0];

```

### 28.1. نتیجه خالی

**[راهکار عمومی JavaScript]**

```
const rows =
    JSON.parse(
        result.Content.providerResult || '[]'
    );

if (rows.length === 0) {
    console.log('No data');
    return;
}

```

## 29. Result معمول و Result فایل یکی نیستند

Structured Result:

```
result
  └── Content
       └── providerResult
            └── JSON string

```

File Result:

```
result
  └── Content
       └── File

```

برای File نباید `providerResult` را `JSON.parse` کنید.

## 43. سناریو 8 — `text/plain`

REST Provider می‌تواند Response Content Type از نوع `text/plain` داشته باشد.

در Contract جاری، Result متنی داخل:

```
result.Content.providerResult

```

قرار می‌گیرد و `providerResult` یک JSON String از آرایه Recordها است که برای `text/plain` فیلدی با نام `Result` دارد:

```
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
