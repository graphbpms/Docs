---
id: 1483
graphDocId: 1506
title: "Internal Implementation Notes"
subTitle: null
module: "04-provider"
wikiPath: "1450/1480/1483"
wikiUrl: "https://wiki.graphbpms.ir/1450/1480/1483"
parentPageUrl: "https://wiki.graphbpms.ir/1450/1480/1483"
breadcrumbs: ["مدیریت منابع داده Provider","طراحی معماری و Production","Internal Implementation Notes"]
parentId: 1480
parentTitle: "طراحی معماری و Production"
url: "1483"
priority: 3
language: 1
showInNavigation: true
isStandalonePage: true
summary: "مسیر موضوعی: مدیریت منابع داده Provider > ۶. طراحی معماری و Production. طراحی معماری و Production. GraphBPMS • Provider Internal Implementation Notes در این آموزش 76. Endpointهای داخلی مشاهده‌شده 76. Endpointهای داخلی مشاهده‌شده در کد UI فعلی Endpointهایی مانند موارد زیر دیده می‌شوند: /Provider/RunProvider /Provider/SaveProviderConfig /Provider/GetProvidersList /Provider/ExecuteProcedureOrFunction /Provider/GetProcedureOrFunctionParamter /Provider/GetRestFieldsSchema /Provider/ExecuteRestService…. محورهای اصلی: 76. Endpointهای داخلی مشاهده‌شده؛ هشدار."
updatedAt: "2026-10-18T16:08:51"
---

# Internal Implementation Notes

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `مدیریت منابع داده Provider` ❯ `طراحی معماری و Production` ❯ `Internal Implementation Notes`  
> 🔗 **لینک نسخه برخط:** [https://wiki.graphbpms.ir/1450/1480/1483](https://wiki.graphbpms.ir/1450/1480/1483)

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
