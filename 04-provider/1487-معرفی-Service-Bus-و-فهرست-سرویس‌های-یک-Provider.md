---
id: 1487
graphDocId: 1435
title: "معرفی Service Bus و فهرست سرویس‌های یک Provider"
subTitle: null
module: "04-provider"
wikiPath: "1450/1486/1487"
wikiUrl: "https://wiki.graphbpms.ir/1450/1486/1487"
parentPageUrl: "https://wiki.graphbpms.ir/1450/1486/1487"
breadcrumbs: ["مدیریت منابع داده Provider","Service Bus و انتشار API","معرفی Service Bus و فهرست سرویس‌های یک Provider"]
parentId: 1486
parentTitle: "Service Bus و انتشار API"
url: "1487"
priority: 1
language: 1
showInNavigation: true
isStandalonePage: true
summary: "مسیر موضوعی: مدیریت منابع داده Provider > ۷. Service Bus و انتشار API. Service Bus و انتشار API. GraphBPMS • Provider معرفی Service Bus و فهرست سرویس‌های یک Provider در این آموزش 92. Service Bus چیست؟ 93. فهرست Service Busهای یک Provider 92. Service Bus چیست؟ تصاویر Service Bus، تنظیمات API و اسناد تولیدشده فهرست Service Busهای یک Provider در حالت بدون رکورد؛ نقطه ورود برای افزودن سرویس جدید. نمونه سند تولیدشده Service Bus شامل درخواست، خروجی، خطاها و دریافت Token. محورهای اصلی: 92. Service Bus چیست؟؛ 93. فهرست Service Busهای یک Provider."
updatedAt: "2026-10-18T16:08:51"
---

# معرفی Service Bus و فهرست سرویس‌های یک Provider

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `مدیریت منابع داده Provider` ❯ `Service Bus و انتشار API` ❯ `معرفی Service Bus و فهرست سرویس‌های یک Provider`  
> 🔗 **لینک نسخه برخط:** [https://wiki.graphbpms.ir/1450/1486/1487](https://wiki.graphbpms.ir/1450/1486/1487)

در این آموزش

-   [92\. Service Bus چیست؟](#wiki-servicebus-intro-section-01)
-   [93\. فهرست Service Busهای یک Provider](#wiki-servicebus-intro-section-02)

## 92\. Service Bus چیست؟

تصاویر Service Bus، تنظیمات API و اسناد تولیدشده

![فهرست Service Busهای یک Provider در حالت بدون رکورد؛ نقطه ورود برای افزودن سرویس جدید.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=F53365B0-081D-481F-BD1A-BE4D26C44FCA)

فهرست Service Busهای یک Provider در حالت بدون رکورد؛ نقطه ورود برای افزودن سرویس جدید.

![نمونه سند تولیدشده Service Bus شامل درخواست، خروجی، خطاها و دریافت Token.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=5F978940-EF58-492F-841E-9A60BDF0881C)

نمونه سند تولیدشده Service Bus شامل درخواست، خروجی، خطاها و دریافت Token.

![نمای فشرده Service Bus متصل به Provider در Grid مدیریت منابع داده.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=24674780-BF54-494A-BAD7-6BF4A298EE28)

نمای فشرده Service Bus متصل به Provider در Grid مدیریت منابع داده.

![Provider دارای چند Service Bus؛ نمایش سرویس‌های وابسته در زیر رکورد Provider.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=D6AC3C64-76E6-42EB-AFAD-B9BFA658805A)

Provider دارای چند Service Bus؛ نمایش سرویس‌های وابسته در زیر رکورد Provider.

![نمونه سند Service Bus از نوع GET با خروجی application/json.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=422EC7B3-80B6-4AC8-8C4B-7757F07778B8)

نمونه سند Service Bus از نوع GET با خروجی application/json.

![نمای Service Bus زیر Provider پس از پاک‌سازی نشانه‌گذاری دستی.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=56A65B23-0C42-4F88-9B40-50ABD14E31CD)

نمای Service Bus زیر Provider پس از پاک‌سازی نشانه‌گذاری دستی.

![فرم Service Bus با خروجی application/json و حالت Anonymous.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=ED13A9F6-90B5-4058-A1C7-762D170DDC16)

فرم Service Bus با خروجی application/json و حالت Anonymous.

![فرم Service Bus با خروجی text/plain و کاربران مجاز.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=94093D77-FB0D-486C-99A1-ED2BBC75771F)

فرم Service Bus با خروجی text/plain و کاربران مجاز.

![تعریف Static Input Parameter در Service Bus؛ نمونه FromUserId با مقدار ثابت.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=D7442601-B11A-4949-8CF7-0E3B20F8837B)

تعریف Static Input Parameter در Service Bus؛ نمونه FromUserId با مقدار ثابت.

![نمای یک Service Bus متصل به Provider در Grid.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=2C7DD331-04F4-498C-BAED-239C94B55D46)

نمای یک Service Bus متصل به Provider در Grid.

![لیست دو Service Bus مستقل روی یک Provider.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=AA56830D-22D0-457D-8FBE-EDFCDDC7AD4F)

لیست دو Service Bus مستقل روی یک Provider.

![نمونه کامل سند Service Bus با بخش دریافت Token و خطاهای دسترسی.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=5F978940-EF58-492F-841E-9A60BDF0881C)

نمونه کامل سند Service Bus با بخش دریافت Token و خطاهای دسترسی.

![فرم Service Bus با انتخاب text/plain در Response Content Type.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=94093D77-FB0D-486C-99A1-ED2BBC75771F)

فرم Service Bus با انتخاب text/plain در Response Content Type.

![پنجره لیست Web Service/Service Bus برای Provider.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=F53365B0-081D-481F-BD1A-BE4D26C44FCA)

پنجره لیست Web Service/Service Bus برای Provider.

![نمای Service Bus زیر Provider پس از پاک‌سازی نشانه‌گذاری دستی.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=56A65B23-0C42-4F88-9B40-50ABD14E31CD)

نمای Service Bus زیر Provider پس از پاک‌سازی نشانه‌گذاری دستی.

![نمونه سند استفاده از Web Service تولیدشده توسط GraphBPMS.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=422EC7B3-80B6-4AC8-8C4B-7757F07778B8)

نمونه سند استفاده از Web Service تولیدشده توسط GraphBPMS.

![Service Bus از نوع GET با خروجی application/json و Static Parameter.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=ED13A9F6-90B5-4058-A1C7-762D170DDC16)

Service Bus از نوع GET با خروجی application/json و Static Parameter.

![Service Bus از نوع POST با خروجی application/json.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=AA56830D-22D0-457D-8FBE-EDFCDDC7AD4F)

Service Bus از نوع POST با خروجی application/json.

![لیست Service Busهای ساخته‌شده برای یک Provider.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=D6AC3C64-76E6-42EB-AFAD-B9BFA658805A)

لیست Service Busهای ساخته‌شده برای یک Provider.

![نمای Provider با یک Service Bus فعال در Grid.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=2C7DD331-04F4-498C-BAED-239C94B55D46)

نمای Provider با یک Service Bus فعال در Grid.

Service Bus در این بخش لایه‌ای برای انتشار Provider به‌صورت یک API قابل مصرف است. هر Provider می‌تواند یک یا چند Service Bus / Web Service مستقل داشته باشد. هر سرویس نام، Method، نوع خروجی، دسترسی و سند مصرف خودش را دارد.

به این ترتیب:

```text
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

## 93\. فهرست Service Busهای یک Provider

در صفحه مدیریت Provider، ستون تعداد Web Serviceها و بخش مربوط به «لیست وب سرویس‌ها» وجود دارد. با باز کردن این بخش، تمام Service Busهای ساخته‌شده برای همان Provider نمایش داده می‌شوند.

هر سرویس دارای شناسه مستقل و نام لاتین مستقل است و از همان فهرست می‌توان آن را ایجاد، ویرایش و سند مصرف آن را مشاهده کرد.
