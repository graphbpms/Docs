---
id: 1490
title: "Static Backend Parameters و قرارداد ورودی API"
englishTitle: "Static Backend Parameters & API Contracts"
category: "04-provider/service-bus-api"
wikiPath: "1450/1486/1490"
wikiUrl: "https://wiki.graphbpms.ir/1450/1486/1490"
breadcrumbs: ["مدیریت منابع داده Provider","Service Bus و انتشار API","Static Backend Parameters و قرارداد ورودی API"]
priority: 4
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# Static Backend Parameters و قرارداد ورودی API

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `Service Bus و انتشار API` ❯ `Static Backend Parameters و قرارداد ورودی API`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1486/1490](https://wiki.graphbpms.ir/1450/1486/1490)

در این آموزش

-   [99\. پارامترهای ثابت Backend](#wiki-servicebus-static-section-01)

## 99\. پارامترهای ثابت Backend

برای Service Bus می‌توان مقدار ثابت برای Parameterهای Provider نهایی تعریف کرد.

مثال:

```text
FromUserId = 1000
```

رفتار این پارامترها:

-   مقدار Static در Backend روی اجرای Provider اعمال می‌شود؛
-   Consumer نیازی به دانستن یا ارسال این Parameter ندارد؛
-   حتی اگر Consumer همان نام Parameter را در Request بفرستد، مقدار ثابت تعریف‌شده در Service Bus برای Provider نهایی اعمال می‌شود؛
-   این Parameter در سند تولیدشده به‌عنوان ورودی قابل ارسال نمایش داده نمی‌شود.

این قابلیت برای ساخت APIهای محدودشده بسیار کاربردی است. برای مثال Provider اصلی ممکن است `FromUserId` بگیرد، اما یک Service Bus خاص همیشه Provider را با `FromUserId = 1000` اجرا کند و Consumer فقط سایر پارامترها را ارسال کند.
