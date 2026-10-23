---
id: 1482
title: "مرز Provider با Public API"
englishTitle: "Provider Boundaries & Public API Isolation"
category: "04-provider/architecture-production"
wikiPath: "1450/1480/1482"
wikiUrl: "https://wiki.graphbpms.ir/1450/1480/1482"
breadcrumbs: ["مدیریت منابع داده Provider","طراحی معماری و Production","مرز Provider با Public API"]
priority: 2
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# مرز Provider با Public API

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `طراحی معماری و Production` ❯ `مرز Provider با Public API`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1480/1482](https://wiki.graphbpms.ir/1450/1480/1482)

در این آموزش

-   [75\. Provider با Shared API فرق دارد](#wiki-provider-public-api-section-01)

## 75\. Provider با Shared API فرق دارد

Provider:

```text
Consumer داخل GraphBPMS
   → Provider
   → DB / REST / WSDL
```

Shared/Public Graph API:

```text
External Application
   → GraphBPMS Public API
   → Login / RunProcess / ...
```

برای Integration خارجی که قرار است خود GraphBPMS را کنترل کند، Public API مستندشده مطرح است. برای اینکه Form/Custom JS یا ماژول داخلی GraphBPMS داده‌ای را از منبع بخواند یا عملیاتی را اجرا کند، Provider مطرح است.
