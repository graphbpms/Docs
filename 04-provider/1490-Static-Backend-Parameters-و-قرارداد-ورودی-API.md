---
id: 1490
graphDocId: 1527
title: "Static Backend Parameters و قرارداد ورودی API"
subTitle: null
module: "04-provider"
wikiPath: "1450/1486/1490"
wikiUrl: "https://wiki.graphbpms.ir/1450/1486/1490"
parentPageUrl: "https://wiki.graphbpms.ir/1450/1486/1490"
breadcrumbs: ["مدیریت منابع داده Provider","Service Bus و انتشار API","Static Backend Parameters و قرارداد ورودی API"]
parentId: 1486
parentTitle: "Service Bus و انتشار API"
url: "1490"
priority: 4
language: 1
showInNavigation: true
isStandalonePage: true
summary: "مسیر موضوعی: مدیریت منابع داده Provider > ۷. Service Bus و انتشار API. Service Bus و انتشار API. GraphBPMS • Provider Static Backend Parameters و قرارداد ورودی API در این آموزش 99. پارامترهای ثابت Backend 99. پارامترهای ثابت Backend برای Service Bus می‌توان مقدار ثابت برای Parameterهای Provider نهایی تعریف کرد. مثال: FromUserId = 1000 رفتار این پارامترها: مقدار Static در Backend روی اجرای Provider اعمال می‌شود؛ Consumer نیازی به دانستن یا ارسال این Parameter ندارد؛. محورهای اصلی: 99. پارامترهای ثابت Backend."
updatedAt: "2026-10-18T16:08:51"
---

# Static Backend Parameters و قرارداد ورودی API

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `مدیریت منابع داده Provider` ❯ `Service Bus و انتشار API` ❯ `Static Backend Parameters و قرارداد ورودی API`  
> 🔗 **لینک نسخه برخط:** [https://wiki.graphbpms.ir/1450/1486/1490](https://wiki.graphbpms.ir/1450/1486/1490)

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
