---
id: 1482
graphDocId: 1477
title: "مرز Provider با Public API"
subTitle: null
module: "04-provider"
wikiPath: "1450/1480/1482"
wikiUrl: "https://wiki.graphbpms.ir/1450/1480/1482"
parentPageUrl: "https://wiki.graphbpms.ir/1450/1480/1482"
breadcrumbs: ["مدیریت منابع داده Provider","طراحی معماری و Production","مرز Provider با Public API"]
parentId: 1480
parentTitle: "طراحی معماری و Production"
url: "1482"
priority: 2
language: 1
showInNavigation: true
isStandalonePage: true
summary: "مسیر موضوعی: مدیریت منابع داده Provider > ۶. طراحی معماری و Production. طراحی معماری و Production. GraphBPMS • Provider مرز Provider با Public API در این آموزش 75. Provider با Shared API فرق دارد 75. Provider با Shared API فرق دارد Provider: Consumer داخل GraphBPMS → Provider → DB / REST / WSDL Shared/Public Graph API: External Application → GraphBPMS Public API → Login / RunProcess / ... برای Integration خارجی که قرار است خود GraphBPMS را کنترل کند، Public API مستندشده مطرح است. محورهای اصلی: 75. Provider با Shared API فرق دارد."
updatedAt: "2026-10-18T16:08:51"
---

# مرز Provider با Public API

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `مدیریت منابع داده Provider` ❯ `طراحی معماری و Production` ❯ `مرز Provider با Public API`  
> 🔗 **لینک نسخه برخط:** [https://wiki.graphbpms.ir/1450/1480/1482](https://wiki.graphbpms.ir/1450/1480/1482)

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
