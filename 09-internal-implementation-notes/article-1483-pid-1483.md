---
id: 1483
title: "Internal Implementation Notes"
englishTitle: "Internal Implementation Notes"
category: "09-internal-implementation-notes"
wikiUrl: "https://wiki.graphbpms.ir/1483"
breadcrumbs: ["Internal Implementation Notes"]
priority: 3
updatedAt: "2026-10-25T21:33:55"
---

# Internal Implementation Notes

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `Internal Implementation Notes`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1483](https://wiki.graphbpms.ir/1483)

# Internal Implementation Notes
در این آموزش
- [76. Endpointهای داخلی مشاهده‌شده](#wiki-internal-notes-section-01)

## 76. Endpointهای داخلی مشاهده‌شده

در کد UI فعلی Endpointهایی مانند موارد زیر دیده می‌شوند:

```
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

- مستقیم فراخوانی نشوند؛

- `absoluteMethodHelper` و APIهای مستند محصول ترجیح داده شوند.
