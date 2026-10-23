---
id: 1481
title: "انتخاب نوع مناسب Provider"
englishTitle: "Provider Architecture Selection Guide"
category: "04-provider/architecture-production"
wikiPath: "1450/1480/1481"
wikiUrl: "https://wiki.graphbpms.ir/1450/1480/1481"
breadcrumbs: ["مدیریت منابع داده Provider","طراحی معماری و Production","انتخاب نوع مناسب Provider"]
priority: 1
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# انتخاب نوع مناسب Provider

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `طراحی معماری و Production` ❯ `انتخاب نوع مناسب Provider`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1480/1481](https://wiki.graphbpms.ir/1450/1480/1481)

در این آموزش

-   [74\. Decision Guide](#wiki-decision-guide-section-01)

## 74\. Decision Guide

اگر فقط داده می‌خوانید:

```text
Query
```

اگر Stored Procedure آماده دارید:

```text
StoredProcedure
```

اگر Function دیتابیس دارید:

```text
Function
```

اگر لیست دستی کوچک دارید:

```text
OptionalOptions
```

اگر SQL پیچیده یا Command دارید:

```text
EnterpriseEditor
```

اگر داده از Form/Entity داخلی GraphBPMS می‌خواهید:

```text
SystemicEntity
```

اگر SOAP/WSDL دارید:

```text
WSDL
```

اگر HTTP API دارید:

```text
REST
```
