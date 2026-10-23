---
id: 1496
title: "مرجع سریع توسعه‌دهنده"
englishTitle: "Developer Quick Reference Cheat Sheet"
category: "04-provider/faq-reference"
wikiPath: "1450/1492/1496"
wikiUrl: "https://wiki.graphbpms.ir/1450/1492/1496"
breadcrumbs: ["مدیریت منابع داده Provider","FAQ و مرجع توسعه‌دهنده","مرجع سریع توسعه‌دهنده"]
priority: 4
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# مرجع سریع توسعه‌دهنده

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `FAQ و مرجع توسعه‌دهنده` ❯ `مرجع سریع توسعه‌دهنده`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1492/1496](https://wiki.graphbpms.ir/1450/1492/1496)

در این آموزش

-   [91\. Cheat Sheet](#wiki-cheatsheet-section-01)

## 91\. Cheat Sheet

### بدون Parameter

```js
absoluteMethodHelper
    .runProvider(PROVIDER_ID)
```

### با Parameter

```js
absoluteMethodHelper
    .runProvider(
        PROVIDER_ID,
        [
            {
                name: 'Id',
                value: 1
            }
        ]
    )
```

### Structured Output

```js
.then(result => {
    const rows =
        JSON.parse(
            result.Content.providerResult || '[]'
        );
})
```

### File Input

```js
{
    name: 'file',
    value: myFile
}
```

### Blob → File

```js
const myFile =
    new File(
        [myBlob],
        'file.bin'
    );
```

### File Output

```js
.then(result => {
    const file = result.Content;

    saveAs(
        file,
        file.name
    );
})
```

### Error

```js
.catch(error => {
    console.error(error);
})
```

### Paging

```js
providerDesigner
    .runProviderPaging(
        PROVIDER_ID,
        parameters,
        pageIndex,
        pageSize,
        globalFilter,
        filters,
        orders
    )
```
