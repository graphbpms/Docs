---
id: 1489
title: "Anonymous، کاربران مجاز، Token و privateToken"
englishTitle: "Anonymous، کاربران مجاز، Token و privateToken"
category: "09-anonymous-token-privatetoken"
wikiUrl: "https://wiki.graphbpms.ir/1489"
breadcrumbs: ["Anonymous، کاربران مجاز، Token و privateToken"]
priority: 3
updatedAt: "2026-10-25T21:33:55"
---

# Anonymous، کاربران مجاز، Token و privateToken

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `Anonymous، کاربران مجاز، Token و privateToken`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1489](https://wiki.graphbpms.ir/1489)

# Anonymous، کاربران مجاز، Token و privateToken
در این آموزش
- [97. Anonymous یا Token-Based Access](#wiki-servicebus-auth-section-01)
- [98. دریافت Token](#wiki-servicebus-auth-section-04)

## 97. Anonymous یا Token-Based Access

دو مدل اصلی دسترسی وجود دارد:

### 97.1. Anonymous

اگر گزینه `Anonymous` فعال باشد، Endpoint بدون Token قابل اجرا است و هر Consumer که به URL دسترسی شبکه‌ای داشته باشد می‌تواند آن را فراخوانی کند.

### 97.2. سرویس محدود به کاربران مجاز

اگر `Anonymous` غیرفعال باشد، سرویس نیازمند Token است. در UI می‌توان Userهای مجاز را انتخاب کرد تا فقط Token همان کاربران اجازه اجرای سرویس را داشته باشد.

Token در Header با نام زیر ارسال می‌شود:

```
privateToken

```

## 98. دریافت Token

برای سرویس‌های غیر Anonymous، ابتدا Token از Endpoint Login دریافت می‌شود:

```
POST /SharedApi/Login
Content-Type: application/json; charset=utf-8

```

Body:

```
{
  "userName": "???",
  "password": "???",
  "networkUser": true
}

```

ساختار خروجی:

```
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

```
Header:
privateToken: <TOKEN>

```

به درخواست Service Bus اضافه می‌شود.
