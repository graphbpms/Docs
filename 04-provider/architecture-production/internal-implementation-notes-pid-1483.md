---
id: 1483
title: "Internal Implementation Notes"
englishTitle: "Internal Implementation & Kernel Notes"
category: "04-provider/architecture-production"
wikiPath: "1450/1480/1483"
wikiUrl: "https://wiki.graphbpms.ir/1450/1480/1483"
breadcrumbs: ["مدیریت منابع داده Provider","طراحی معماری و Production","Internal Implementation Notes"]
priority: 3
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# Internal Implementation Notes

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `طراحی معماری و Production` ❯ `Internal Implementation Notes`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1480/1483](https://wiki.graphbpms.ir/1450/1480/1483)

در این آموزش

-   [76\. Endpointهای داخلی مشاهده‌شده](#wiki-internal-notes-section-01)

## 76\. Endpointهای داخلی مشاهده‌شده

در کد UI فعلی Endpointهایی مانند موارد زیر دیده می‌شوند:

```text
/Provider/RunProvider
/Provider/SaveProviderConfig
/Provider/GetProvidersList
/Provider/ExecuteProcedureOrFunction
/Provider/GetProcedureOrFunctionParamter
/Provider/GetRestFieldsSchema
/Provider/ExecuteRestService
/Provider/GetServiceMethodParamsList
/Provider/ExecuteWebService
/Provider/GetSystemicEntities
/Provider/GetGbForm
```

### هشدار

این Endpointها **API عمومی رسمی برای Custom Development تلقی نشوند**.

تا وقتی مستند Public API مستقلی برای آن‌ها وجود ندارد:

-   مستقیم فراخوانی نشوند؛
-   `absoluteMethodHelper` و APIهای مستند محصول ترجیح داده شوند.
