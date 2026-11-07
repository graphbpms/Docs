---
id: 1496
title: "مرجع سریع توسعه‌دهنده"
englishTitle: "Developer Quick Reference Cheat Sheet"
category: "04-provider/faq-reference"
wikiPath: "1496"
wikiUrl: "https://wiki.graphbpms.ir/1496"
breadcrumbs: ["مرجع سریع توسعه‌دهنده"]
priority: 4
isCanonical: true
updatedAt: "2026-10-25T21:33:55"
---

# مرجع سریع توسعه‌دهنده

> [!NOTE]
> 🧭 **سلسلهمراتب موضوعی:** `مرجع سریع توسعه‌دهنده`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1496](https://wiki.graphbpms.ir/1496)

# مرجع سریع توسعه‌دهنده
در این آموزش
- [91. Cheat Sheet](#wiki-cheatsheet-section-01)

## 91. Cheat Sheet

### بدون Parameter

```
absoluteMethodHelper
    .runProvider(PROVIDER_ID)

```

### با Parameter

```
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

```
.then(result => {
    const rows =
        JSON.parse(
            result.Content.providerResult || '[]'
        );
})

```

### File Input

```
{
    name: 'file',
    value: myFile
}

```

### Blob → File

```
const myFile =
    new File(
        [myBlob],
        'file.bin'
    );

```

### File Output

```
.then(result => {
    const file = result.Content;

    saveAs(
        file,
        file.name
    );
})

```

### Error

```
.catch(error => {
    console.error(error);
})

```

### Paging

```
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
