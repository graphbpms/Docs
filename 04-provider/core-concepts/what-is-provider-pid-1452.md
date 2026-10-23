---
id: 1452
title: "Provider چیست و مسیر دسترسی"
englishTitle: "Provider Fundamentals & Architecture"
category: "04-provider/core-concepts"
wikiPath: "1450/1451/1452"
wikiUrl: "https://wiki.graphbpms.ir/1450/1451/1452"
breadcrumbs: ["مدیریت منابع داده Provider","آشنایی و شروع کار","Provider چیست و مسیر دسترسی"]
priority: 1
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# Provider چیست و مسیر دسترسی

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `آشنایی و شروع کار` ❯ `Provider چیست و مسیر دسترسی`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1451/1452](https://wiki.graphbpms.ir/1450/1451/1452)

در این آموزش

-   [بخش اول — شناخت Provider](#part-01)
-   [1\. Provider چیست؟](#wiki-provider-overview-section-03)
-   [2\. مسیر دسترسی و صفحه مدیریت منابع داده](#wiki-provider-overview-section-04)

## بخش اول — شناخت Provider

## 1\. Provider چیست؟

Provider یا «منبع داده» در GraphBPMS لایه‌ای برای اتصال اجزای پلتفرم به داده‌های داخلی یا خارجی است. این داده می‌تواند از پایگاه داده، REST، WSDL/SOAP یا سایر انواع منبع داده‌ای که در ماژول پشتیبانی می‌شوند تأمین شود.

```text
Input Parameters
      ↓
Provider
      ↓
Execution / Data Source
      ↓
Output Fields
```

Provider باعث می‌شود منطق دسترسی به داده از Form/UI جدا شود، یک منبع داده در چند نقطه قابل استفاده باشد، پارامترها و خروجی‌ها قرارداد مشخص داشته باشند، و تست منبع داده قبل از مصرف انجام شود.

## 2\. مسیر دسترسی و صفحه مدیریت منابع داده

نماهای مدیریت منابع داده و شروع ساخت Provider

![انتخاب خانواده Web Service و نوع WSDL/REST هنگام ساخت Provider.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=D4390E88-AF2F-4475-B839-7C3E6A0B63C4)

انتخاب خانواده Web Service و نوع WSDL/REST هنگام ساخت Provider.

![صفحه اصلی مدیریت منابع داده و انواع Providerهای موجود.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=B4BC02E5-B75A-4B08-B604-D46331CF104A)

صفحه اصلی مدیریت منابع داده و انواع Providerهای موجود.

![انتخاب Database Connection هنگام ایجاد Query Provider.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=5BD26296-61EF-4DDC-A4C3-8BEA07CAB6B9)

انتخاب Database Connection هنگام ایجاد Query Provider.

![فرم اطلاعات پایه Query Provider با انتخاب Database.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=66550FB6-81BD-4830-936F-877B454ABE2A)

فرم اطلاعات پایه Query Provider با انتخاب Database.

![انتخاب نوع Provider بانک اطلاعاتی یا Web Service پس از پاک‌سازی نشانه‌گذاری دستی.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=89B8EAA9-C976-41FC-8D01-A218B9C87574)

انتخاب نوع Provider بانک اطلاعاتی یا Web Service پس از پاک‌سازی نشانه‌گذاری دستی.

مسیر عمومی:

```text
مدیریت منابع اطلاعاتی
    └── مدیریت منابع داده
```

در UI جاری ستون‌هایی مانند شناسه، عنوان، نوع، تعداد Web Service/Service Bus مرتبط، تاریخ ایجاد و عملیات دیده می‌شوند. عملیات رکورد با توجه به سطح دسترسی می‌تواند شامل مشاهده جزئیات، ویرایش، مدیریت سطح دسترسی، ارتباط Service Bus و نمایش Providerهای سیستمی باشد.

`ProviderId` شناسه محیطی است. شناسه Development الزاماً در Test/Production یکسان نیست. در مستند عمومی از `PROVIDER_ID` استفاده کنید.
