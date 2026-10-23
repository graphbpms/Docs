---
id: 1488
title: "ایجاد/ویرایش Service Bus، GET/POST و نوع Response"
englishTitle: "Service Bus Endpoints (GET / POST)"
category: "04-provider/service-bus-api"
wikiPath: "1450/1486/1488"
wikiUrl: "https://wiki.graphbpms.ir/1450/1486/1488"
breadcrumbs: ["مدیریت منابع داده Provider","Service Bus و انتشار API","ایجاد/ویرایش Service Bus، GET/POST و نوع Response"]
priority: 2
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# ایجاد/ویرایش Service Bus، GET/POST و نوع Response

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `Service Bus و انتشار API` ❯ `ایجاد/ویرایش Service Bus، GET/POST و نوع Response`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1486/1488](https://wiki.graphbpms.ir/1450/1486/1488)

در این آموزش

-   [94\. تنظیمات ایجاد یا ویرایش Service Bus](#wiki-servicebus-settings-section-01)
-   [95\. GET و POST](#wiki-servicebus-settings-section-02)
-   [96\. نوع خروجی JSON یا text/plain](#wiki-servicebus-settings-section-03)

## 94\. تنظیمات ایجاد یا ویرایش Service Bus

تنظیمات اصلی شامل موارد زیر هستند:

| تنظیم | کاربرد |
| --- | --- |
| نام لاتین | نام Endpoint و شناسه قابل استفاده در URL |
| فعال | فعال/غیرفعال بودن سرویس |
| Anonymous | اجازه اجرای سرویس بدون Login/Token |
| کاربران مجاز | در حالت غیر Anonymous، فهرست Userهایی که Token آن‌ها اجازه اجرای سرویس دارد |
| Request Method | انتخاب `GET` یا `POST` |
| Response Content Type | انتخاب `application/json` یا `text/plain` |
| توضیحات | متن توضیحی که در سند تولیدشده نمایش داده می‌شود |
| پارامترهای ورودی پیش‌فرض | مقدارهای ثابت Backend برای Parameterهای Provider |

## 95\. GET و POST

Service Bus می‌تواند با Methodهای `GET` یا `POST` ساخته شود. انتخاب Method باید بر اساس قرارداد مصرف API انجام شود.

برای `POST`، سند سرویس ساختار Body را نمایش می‌دهد. نمونه Body می‌تواند به یکی از دو شکل زیر ارسال شود:

```json
[
  { "Name": "FromUserId", "Value": "???" }
]
```

یا:

```json
{
  "FromUserId": "???"
}
```

در `GET` نیز پارامترهای قابل دریافت مطابق Contract همان سرویس در سند نهایی مشخص می‌شوند.

## 96\. نوع خروجی JSON یا text/plain

برای هر Service Bus می‌توان نوع محتوای پاسخ را انتخاب کرد:

```text
application/json
text/plain
```

اگر `application/json` انتخاب شود، سند تولیدشده ساختار خروجی Provider را به شکل JSON نمایش می‌دهد.

نمونه:

```json
[
  {
    "UserID": "(Int32)???",
    "FullName": "(String)???"
  }
]
```

اگر `text/plain` انتخاب شود، پاسخ بیرونی سرویس به شکل متن ارائه می‌شود و Consumer باید آن را مطابق Contract همان API تفسیر کند.
