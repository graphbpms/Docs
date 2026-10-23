---
id: 1489
graphDocId: 1507
title: "Anonymous، کاربران مجاز، Token و privateToken"
subTitle: null
module: "04-provider"
wikiPath: "1450/1486/1489"
wikiUrl: "https://wiki.graphbpms.ir/1450/1486/1489"
parentPageUrl: "https://wiki.graphbpms.ir/1450/1486/1489"
breadcrumbs: ["مدیریت منابع داده Provider","Service Bus و انتشار API","Anonymous، کاربران مجاز، Token و privateToken"]
parentId: 1486
parentTitle: "Service Bus و انتشار API"
url: "1489"
priority: 3
language: 1
showInNavigation: true
isStandalonePage: true
summary: "مسیر موضوعی: مدیریت منابع داده Provider > ۷. Service Bus و انتشار API. Service Bus و انتشار API. GraphBPMS • Provider Anonymous، کاربران مجاز، Token و privateToken در این آموزش 97. Anonymous یا Token-Based Access 98. دریافت Token 97. Anonymous یا Token-Based Access دو مدل اصلی دسترسی وجود دارد: 97.1. Anonymous اگر گزینه Anonymous فعال باشد، Endpoint بدون Token قابل اجرا است و هر Consumer که به URL دسترسی شبکه‌ای داشته باشد می‌تواند آن را فراخوانی کند. 97.2. محورهای اصلی: 97. Anonymous یا Token-Based Access؛ 97.1. Anonymous؛ 97.2. سرویس محدود به کاربران مجاز؛ 98. دریافت Token."
updatedAt: "2026-10-18T16:08:51"
---

# Anonymous، کاربران مجاز، Token و privateToken

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `مدیریت منابع داده Provider` ❯ `Service Bus و انتشار API` ❯ `Anonymous، کاربران مجاز، Token و privateToken`  
> 🔗 **لینک نسخه برخط:** [https://wiki.graphbpms.ir/1450/1486/1489](https://wiki.graphbpms.ir/1450/1486/1489)

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
