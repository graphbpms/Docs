---
id: 1490
title: "Static Backend Parameters و قرارداد ورودی API"
englishTitle: "Static Backend Parameters و قرارداد ورودی API"
category: "09-static-backend-parameters-api"
wikiUrl: "https://wiki.graphbpms.ir/1490"
breadcrumbs: ["Static Backend Parameters و قرارداد ورودی API"]
priority: 4
updatedAt: "2026-10-25T21:33:55"
---

# Static Backend Parameters و قرارداد ورودی API

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `Static Backend Parameters و قرارداد ورودی API`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1490](https://wiki.graphbpms.ir/1490)

# Static Backend Parameters و قرارداد ورودی API
در این آموزش
- [99. پارامترهای ثابت Backend](#wiki-servicebus-static-section-01)

## 99. پارامترهای ثابت Backend

برای Service Bus می‌توان مقدار ثابت برای Parameterهای Provider نهایی تعریف کرد.

مثال:

```
FromUserId = 1000

```

رفتار این پارامترها:

- مقدار Static در Backend روی اجرای Provider اعمال می‌شود؛

- Consumer نیازی به دانستن یا ارسال این Parameter ندارد؛

- حتی اگر Consumer همان نام Parameter را در Request بفرستد، مقدار ثابت تعریف‌شده در Service Bus برای Provider نهایی اعمال می‌شود؛

- این Parameter در سند تولیدشده به‌عنوان ورودی قابل ارسال نمایش داده نمی‌شود.

این قابلیت برای ساخت APIهای محدودشده بسیار کاربردی است. برای مثال Provider اصلی ممکن است `FromUserId` بگیرد، اما یک Service Bus خاص همیشه Provider را با `FromUserId = 1000` اجرا کند و Consumer فقط سایر پارامترها را ارسال کند.
