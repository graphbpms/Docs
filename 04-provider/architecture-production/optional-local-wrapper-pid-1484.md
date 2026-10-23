---
id: 1484
title: "Wrapper محلی اختیاری"
englishTitle: "Client SDK & Local Wrappers"
category: "04-provider/architecture-production"
wikiPath: "1450/1480/1484"
wikiUrl: "https://wiki.graphbpms.ir/1450/1480/1484"
breadcrumbs: ["مدیریت منابع داده Provider","طراحی معماری و Production","Wrapper محلی اختیاری"]
priority: 4
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# Wrapper محلی اختیاری

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `طراحی معماری و Production` ❯ `Wrapper محلی اختیاری`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1480/1484](https://wiki.graphbpms.ir/1450/1480/1484)

در این آموزش

-   [77\. Wrapper برای Structured Provider](#wiki-wrapper-section-01)

## 77\. Wrapper برای Structured Provider

**\[راهکار عمومی JavaScript\]**

اگر پروژه Providerهای Structured زیادی دارد:

```js
function runDataProvider(
    providerId,
    parameters
) {
    return absoluteMethodHelper
        .runProvider(
            providerId,
            parameters || []
        )
        .then(result => {
            return JSON.parse(
                result.Content.providerResult || '[]'
            );
        });
}
```

استفاده:

```js
runDataProvider(
    PROVIDER_ID,
    [
        {
            name: 'StateId',
            value: 1
        }
    ]
).then(rows => {
    console.log(rows);
});
```

### محدودیت

این Wrapper فقط برای Structured Data مناسب است. برای File Provider استفاده نشود، چون File در:

```js
result.Content
```

قرار دارد.
