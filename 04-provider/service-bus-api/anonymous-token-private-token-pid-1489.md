---
id: 1489
title: "Anonymous، کاربران مجاز، Token و privateToken"
englishTitle: "Authentication: Anonymous, Tokens & API Keys"
category: "04-provider/service-bus-api"
wikiPath: "1450/1486/1489"
wikiUrl: "https://wiki.graphbpms.ir/1450/1486/1489"
breadcrumbs: ["مدیریت منابع داده Provider","Service Bus و انتشار API","Anonymous، کاربران مجاز، Token و privateToken"]
priority: 3
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# Anonymous، کاربران مجاز، Token و privateToken

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `Service Bus و انتشار API` ❯ `Anonymous، کاربران مجاز، Token و privateToken`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1486/1489](https://wiki.graphbpms.ir/1450/1486/1489)

در این آموزش

-   [97\. Anonymous یا Token-Based Access](#wiki-servicebus-auth-section-01)
-   [98\. دریافت Token](#wiki-servicebus-auth-section-04)

## 97\. Anonymous یا Token-Based Access

دو مدل اصلی دسترسی وجود دارد:

### 97.1. Anonymous

اگر گزینه `Anonymous` فعال باشد، Endpoint بدون Token قابل اجرا است و هر Consumer که به URL دسترسی شبکه‌ای داشته باشد می‌تواند آن را فراخوانی کند.

### 97.2. سرویس محدود به کاربران مجاز

اگر `Anonymous` غیرفعال باشد، سرویس نیازمند Token است. در UI می‌توان Userهای مجاز را انتخاب کرد تا فقط Token همان کاربران اجازه اجرای سرویس را داشته باشد.

Token در Header با نام زیر ارسال می‌شود:

```text
privateToken
```

## 98\. دریافت Token

برای سرویس‌های غیر Anonymous، ابتدا Token از Endpoint Login دریافت می‌شود:

```text
POST /SharedApi/Login
Content-Type: application/json; charset=utf-8
```

Body:

```json
{
  "userName": "???",
  "password": "???",
  "networkUser": true
}
```

ساختار خروجی:

```json
{
  "HasError": false,
  "ErrorCode": 0,
  "Message": null,
  "MessageException": null,
  "Content": {
    "Token": {
      "PrivateToken": "???"
    }
  }
}
```

پس از دریافت Token:

```text
Header:
privateToken: <TOKEN>
```

به درخواست Service Bus اضافه می‌شود.
