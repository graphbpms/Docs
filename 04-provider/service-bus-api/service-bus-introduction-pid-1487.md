---
id: 1487
title: "معرفی Service Bus و فهرست سرویس‌های یک Provider"
englishTitle: "Enterprise Service Bus Overview"
category: "04-provider/service-bus-api"
wikiPath: "1487"
wikiUrl: "https://wiki.graphbpms.ir/1487"
breadcrumbs: ["مدیریت منابع داده Provider","Service Bus و انتشار API","معرفی Service Bus و فهرست سرویس‌های یک Provider"]
priority: 1
isCanonical: true
updatedAt: "2026-10-25T21:33:55"
---

# معرفی Service Bus و فهرست سرویس‌های یک Provider

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `Service Bus و انتشار API` ❯ `معرفی Service Bus و فهرست سرویس‌های یک Provider`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1487](https://wiki.graphbpms.ir/1487)

# معرفی Service Bus و فهرست سرویس‌های یک Provider
در این آموزش
- [92. Service Bus چیست؟](#wiki-servicebus-intro-section-01)
- [93. فهرست Service Busهای یک Provider](#wiki-servicebus-intro-section-02)

## 92. Service Bus چیست؟

تصاویر Service Bus، تنظیمات API و اسناد تولیدشدهفهرست Service Busهای یک Provider در حالت بدون رکورد؛ نقطه ورود برای افزودن سرویس جدید.نمونه سند تولیدشده Service Bus شامل درخواست، خروجی، خطاها و دریافت Token.نمای فشرده Service Bus متصل به Provider در Grid مدیریت منابع داده.Provider دارای چند Service Bus؛ نمایش سرویس‌های وابسته در زیر رکورد Provider.نمونه سند Service Bus از نوع GET با خروجی application/json.نمای Service Bus زیر Provider پس از پاک‌سازی نشانه‌گذاری دستی.فرم Service Bus با خروجی application/json و حالت Anonymous.فرم Service Bus با خروجی text/plain و کاربران مجاز.تعریف Static Input Parameter در Service Bus؛ نمونه FromUserId با مقدار ثابت.نمای یک Service Bus متصل به Provider در Grid.لیست دو Service Bus مستقل روی یک Provider.نمونه کامل سند Service Bus با بخش دریافت Token و خطاهای دسترسی.فرم Service Bus با انتخاب text/plain در Response Content Type.پنجره لیست Web Service/Service Bus برای Provider.نمای Service Bus زیر Provider پس از پاک‌سازی نشانه‌گذاری دستی.نمونه سند استفاده از Web Service تولیدشده توسط GraphBPMS.Service Bus از نوع GET با خروجی application/json و Static Parameter.Service Bus از نوع POST با خروجی application/json.لیست Service Busهای ساخته‌شده برای یک Provider.نمای Provider با یک Service Bus فعال در Grid.

Service Bus در این بخش لایه‌ای برای انتشار Provider به‌صورت یک API قابل مصرف است. هر Provider می‌تواند یک یا چند Service Bus / Web Service مستقل داشته باشد. هر سرویس نام، Method، نوع خروجی، دسترسی و سند مصرف خودش را دارد.

به این ترتیب:

```
Provider
   ↓
Service Bus / API
   ↓
HTTP GET / POST
   ↓
Consumer داخلی یا خارجی
   ↓
اجرای همان Provider در Backend

```

طراح می‌تواند برای یک Provider چند API مختلف بسازد؛ برای مثال یکی عمومی و GET، دیگری محدود به کاربران مشخص و POST.

## 93. فهرست Service Busهای یک Provider

در صفحه مدیریت Provider، ستون تعداد Web Serviceها و بخش مربوط به «لیست وب سرویس‌ها» وجود دارد. با باز کردن این بخش، تمام Service Busهای ساخته‌شده برای همان Provider نمایش داده می‌شوند.

هر سرویس دارای شناسه مستقل و نام لاتین مستقل است و از همان فهرست می‌توان آن را ایجاد، ویرایش و سند مصرف آن را مشاهده کرد.
