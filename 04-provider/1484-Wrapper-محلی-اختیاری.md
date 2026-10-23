---
id: 1484
graphDocId: 1526
title: "Wrapper محلی اختیاری"
subTitle: null
module: "04-provider"
wikiPath: "1450/1480/1484"
wikiUrl: "https://wiki.graphbpms.ir/1450/1480/1484"
parentPageUrl: "https://wiki.graphbpms.ir/1450/1480/1484"
breadcrumbs: ["مدیریت منابع داده Provider","طراحی معماری و Production","Wrapper محلی اختیاری"]
parentId: 1480
parentTitle: "طراحی معماری و Production"
url: "1484"
priority: 4
language: 1
showInNavigation: true
isStandalonePage: true
summary: "مسیر موضوعی: مدیریت منابع داده Provider > ۶. طراحی معماری و Production. طراحی معماری و Production. GraphBPMS • Provider Wrapper محلی اختیاری در این آموزش 77. Wrapper برای Structured Provider 77. Wrapper برای Structured Provider [راهکار عمومی JavaScript] اگر پروژه Providerهای Structured زیادی دارد: function runDataProvider( providerId, parameters ) { return absoluteMethodHelper .runProvider( providerId, parameters؛ ؛ [] ) .then(result => { return JSON.parse( result.Content.providerResult؛ ؛. محورهای اصلی: 77. Wrapper برای Structured Provider؛ محدودیت؛ [راهکار عمومی JavaScript]."
updatedAt: "2026-10-18T16:08:51"
---

# Wrapper محلی اختیاری

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `مدیریت منابع داده Provider` ❯ `طراحی معماری و Production` ❯ `Wrapper محلی اختیاری`  
> 🔗 **لینک نسخه برخط:** [https://wiki.graphbpms.ir/1450/1480/1484](https://wiki.graphbpms.ir/1450/1480/1484)

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
