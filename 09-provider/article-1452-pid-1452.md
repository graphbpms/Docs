---
id: 1452
title: "Provider چیست و مسیر دسترسی"
englishTitle: "Provider چیست و مسیر دسترسی"
category: "09-provider"
wikiUrl: "https://wiki.graphbpms.ir/1452"
breadcrumbs: ["Provider چیست و مسیر دسترسی"]
priority: 1
updatedAt: "2026-10-25T21:33:55"
---

# Provider چیست و مسیر دسترسی

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `Provider چیست و مسیر دسترسی`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1452](https://wiki.graphbpms.ir/1452)

# Provider چیست و مسیر دسترسی
در این آموزش
- [بخش اول — شناخت Provider](#part-01)
- [1. Provider چیست؟](#wiki-provider-overview-section-03)
- [2. مسیر دسترسی و صفحه مدیریت منابع داده](#wiki-provider-overview-section-04)

## بخش اول — شناخت Provider

## 1. Provider چیست؟

Provider یا «منبع داده» در GraphBPMS لایه‌ای برای اتصال اجزای پلتفرم به داده‌های داخلی یا خارجی است. این داده می‌تواند از پایگاه داده، REST، WSDL/SOAP یا سایر انواع منبع داده‌ای که در ماژول پشتیبانی می‌شوند تأمین شود.

```
Input Parameters
      ↓
Provider
      ↓
Execution / Data Source
      ↓
Output Fields

```

Provider باعث می‌شود منطق دسترسی به داده از Form/UI جدا شود، یک منبع داده در چند نقطه قابل استفاده باشد، پارامترها و خروجی‌ها قرارداد مشخص داشته باشند، و تست منبع داده قبل از مصرف انجام شود.

## 2. مسیر دسترسی و صفحه مدیریت منابع داده

نماهای مدیریت منابع داده و شروع ساخت Providerانتخاب خانواده Web Service و نوع WSDL/REST هنگام ساخت Provider.صفحه اصلی مدیریت منابع داده و انواع Providerهای موجود.انتخاب Database Connection هنگام ایجاد Query Provider.فرم اطلاعات پایه Query Provider با انتخاب Database.انتخاب نوع Provider بانک اطلاعاتی یا Web Service پس از پاک‌سازی نشانه‌گذاری دستی.

مسیر عمومی:

```
مدیریت منابع اطلاعاتی
    └── مدیریت منابع داده

```

در UI جاری ستون‌هایی مانند شناسه، عنوان، نوع، تعداد Web Service/Service Bus مرتبط، تاریخ ایجاد و عملیات دیده می‌شوند. عملیات رکورد با توجه به سطح دسترسی می‌تواند شامل مشاهده جزئیات، ویرایش، مدیریت سطح دسترسی، ارتباط Service Bus و نمایش Providerهای سیستمی باشد.

`ProviderId` شناسه محیطی است. شناسه Development الزاماً در Test/Production یکسان نیست. در مستند عمومی از `PROVIDER_ID` استفاده کنید.
