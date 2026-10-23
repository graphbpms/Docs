---
id: 1467
graphDocId: 1538
title: "Test و Provider Details"
subTitle: null
module: "04-provider"
wikiPath: "1450/1462/1467"
wikiUrl: "https://wiki.graphbpms.ir/1450/1462/1467"
parentPageUrl: "https://wiki.graphbpms.ir/1450/1462/1467"
breadcrumbs: ["مدیریت منابع داده Provider","اتصال به Web Service","Test و Provider Details"]
parentId: 1462
parentTitle: "اتصال به Web Service"
url: "1467"
priority: 5
language: 1
showInNavigation: true
isStandalonePage: true
summary: "مسیر موضوعی: مدیریت منابع داده Provider > ۳. اتصال به Web Service. اتصال به Web Service. GraphBPMS • Provider Test و Provider Details در این آموزش 14.9. گام «تست وب سرویس» 14.10. صفحه «جزئیات منبع داده» 14.11. جمع‌بندی بخش REST 23. چرا Test مهم است؟ 24. اطلاعات صفحه Provider Details 14.9. گام «تست وب سرویس» پس از تکمیل ورودی‌ها و خروجی‌ها، امکان تست Provider وجود دارد. اگر سرویس ورودی نداشته باشد، سامانه پیام «این سرویس دارای پارامتر ورودی نمی‌باشد» نمایش می‌دهد. محورهای اصلی: 14.9. گام «تست وب سرویس»؛ 14.10. صفحه «جزئیات منبع داده»؛ 14.11. جمع‌بندی بخش REST؛ 23. چرا Test مهم است؟؛ 24. اطلاعات صفحه Provider Details."
updatedAt: "2026-10-18T16:08:51"
---

# Test و Provider Details

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `مدیریت منابع داده Provider` ❯ `اتصال به Web Service` ❯ `Test و Provider Details`  
> 🔗 **لینک نسخه برخط:** [https://wiki.graphbpms.ir/1450/1462/1467](https://wiki.graphbpms.ir/1450/1462/1467)

در این آموزش

-   [14.9. گام «تست وب سرویس»](#wiki-rest-test-details-section-01)
-   [14.10. صفحه «جزئیات منبع داده»](#wiki-rest-test-details-section-02)
-   [14.11. جمع‌بندی بخش REST](#wiki-rest-test-details-section-03)
-   [23\. چرا Test مهم است؟](#wiki-rest-test-details-section-04)
-   [24\. اطلاعات صفحه Provider Details](#wiki-rest-test-details-section-05)

## 14.9. گام «تست وب سرویس»

پس از تکمیل ورودی‌ها و خروجی‌ها، امکان تست Provider وجود دارد. اگر سرویس ورودی نداشته باشد، سامانه پیام «این سرویس دارای پارامتر ورودی نمی‌باشد» نمایش می‌دهد. در سناریوهای دارای پارامتر نیز این گام برای اعتبارسنجی Contract، بررسی Connectivity و صحت پاسخ بسیار مهم است.

## 14.10. صفحه «جزئیات منبع داده»

در مرحله نهایی، جمع‌بندی تنظیمات Provider نمایش داده می‌شود: عنوان منبع داده، نوع منبع داده، لیست پارامترها و لیست فیلدهای خروجی. این صفحه آخرین فرصت برای مرور تنظیمات قبل از ذخیره نهایی است.

![گام «تست وب سرویس». اگر سرویس پارامتر ورودی نداشته باشد، سامانه پیام مناسب نمایش می‌دهد و امکان تست مستقیم فراهم می‌شود.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=69155F77-847E-4191-807E-91D580F47470)

گام «تست وب سرویس». اگر سرویس پارامتر ورودی نداشته باشد، سامانه پیام مناسب نمایش می‌دهد و امکان تست مستقیم فراهم می‌شود.

![صفحه نهایی جزئیات Provider از نوع Rest شامل عنوان منبع داده، نوع منبع، لیست پارامترها و لیست فیلدهای خروجی.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=AACE45FF-C3D8-478F-AFC2-41E21E8B1649)

صفحه نهایی جزئیات Provider از نوع Rest شامل عنوان منبع داده، نوع منبع، لیست پارامترها و لیست فیلدهای خروجی.

## 14.11. جمع‌بندی بخش REST

-   در گام Rest Service، آدرس سرویس، متد، Raw Body، Accept و Redirect کنترل می‌شود.
-   برای پارامترهای URL باید از placeholder داخل `{ }` استفاده کرد و در گام پارامترها برای همان نام، ردیف متناظر ساخت.
-   نوع محتوای درخواست تعیین می‌کند پارامترها در body، header، form-data یا url قرار گیرند.
-   در پاسخ سرویس می‌توان فیلدها را از body یا header استخراج کرد.
-   با Output Field Script می‌توان فیلدهای محاسباتی سفارشی مانند `IsNotOk` ایجاد کرد.
-   تست سرویس و صفحه جزئیات، دو مرحله مهم برای کنترل نهایی Contract پیش از ذخیره هستند.

## 23\. چرا Test مهم است؟

قبل از Save نهایی Provider باید موارد زیر بررسی شوند:

-   Query/Service بدون خطا اجرا شود؛
-   پارامترها درست تشخیص داده شوند؛
-   مقدار Test معتبر باشد؛
-   Output Schema درست باشد؛
-   Typeها و Nullable بودن خروجی صحیح باشند؛
-   Paging درست کار کند؛
-   File Input/Output واقعاً با Endpoint سازگار باشد.

## 24\. اطلاعات صفحه Provider Details

صفحه Details بسته به نوع Provider می‌تواند نشان دهد:

-   نام Provider
-   نوع Provider
-   بانک اطلاعاتی
-   Query
-   Auto Transaction
-   Procedure/Function
-   WSDL Method
-   REST URL
-   REST Method
-   Raw Body
-   Accept
-   Request Content Type
-   Response Content Type
-   Array بودن Response
-   Paging
-   Input Parameters
-   Output Fields
-   نمونه JavaScript

این صفحه یکی از بهترین نقاط برای کنترل نهایی Provider قبل از استفاده در Form/Custom JS است.
