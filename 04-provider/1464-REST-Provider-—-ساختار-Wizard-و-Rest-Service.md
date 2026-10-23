---
id: 1464
graphDocId: 1474
title: "REST Provider — ساختار Wizard و Rest Service"
subTitle: null
module: "04-provider"
wikiPath: "1450/1462/1464"
wikiUrl: "https://wiki.graphbpms.ir/1450/1462/1464"
parentPageUrl: "https://wiki.graphbpms.ir/1450/1462/1464"
breadcrumbs: ["مدیریت منابع داده Provider","اتصال به Web Service","REST Provider — ساختار Wizard و Rest Service"]
parentId: 1462
parentTitle: "اتصال به Web Service"
url: "1464"
priority: 2
language: 1
showInNavigation: true
isStandalonePage: true
summary: "مسیر موضوعی: مدیریت منابع داده Provider > ۳. اتصال به Web Service. اتصال به Web Service. GraphBPMS • Provider REST Provider — ساختار Wizard و Rest Service در این آموزش 14. Provider از نوع REST 14.1. گام «اطلاعات منبع» 14.2. گام «مدیریت Rest Service» و فیلدهای آن 14.3. Query Parameter در URL و placeholderها 14.4. متد ارسال و نوع Accept 14. Provider از نوع REST نکته: بخش REST در Wizard ساخت Provider چندمرحله‌ای است و هر مرحله نقش مشخصی در Contract سرویس دارد. محورهای اصلی: 14. Provider از نوع REST؛ 14.1. گام «اطلاعات منبع»؛ 14.2. گام «مدیریت Rest Service» و فیلدهای آن؛ 14.3. Query Parameter در URL و placeholderها؛ 14.4. متد ارسال و نوع Accept."
updatedAt: "2026-10-18T16:08:51"
---

# REST Provider — ساختار Wizard و Rest Service

> [!NOTE]
> 🧭 **مسیر در دانشنامه:** `مدیریت منابع داده Provider` ❯ `اتصال به Web Service` ❯ `REST Provider — ساختار Wizard و Rest Service`  
> 🔗 **لینک نسخه برخط:** [https://wiki.graphbpms.ir/1450/1462/1464](https://wiki.graphbpms.ir/1450/1462/1464)

در این آموزش

-   [14\. Provider از نوع REST](#wiki-rest-core-section-01)
-   [14.1. گام «اطلاعات منبع»](#wiki-rest-core-section-02)
-   [14.2. گام «مدیریت Rest Service» و فیلدهای آن](#wiki-rest-core-section-03)
-   [14.3. Query Parameter در URL و placeholderها](#wiki-rest-core-section-04)
-   [14.4. متد ارسال و نوع Accept](#wiki-rest-core-section-05)

## 14\. Provider از نوع REST

**نکته:** بخش REST در Wizard ساخت Provider چندمرحله‌ای است و هر مرحله نقش مشخصی در Contract سرویس دارد. در نسخه فعلی UI، این مراحل معمولاً شامل «اطلاعات منبع»، «مدیریت Rest Service»، «لیست پارامترها»، «لیست فیلدها»، «تست وب سرویس» و «جزئیات منبع داده» هستند.

Provider از نوع REST برای اتصال به APIهای مبتنی بر HTTP/HTTPS به‌کار می‌رود. در این نوع Provider شما آدرس سرویس، متد ارسال، هدرهای موردنیاز، نحوه ارسال پارامترها، ساختار پاسخ و حتی فیلدهای محاسباتی خروجی را تعریف می‌کنید. به همین دلیل مستندسازی این بخش باید کامل باشد و تنها به یک معرفی کوتاه محدود نماند.

## 14.1. گام «اطلاعات منبع»

در اولین گام، نوع منبع داده روی `Rest` قرار می‌گیرد و عنوان منبع داده تعیین می‌شود. این عنوان نام داخلی Provider در سامانه است و در مصرف‌های بعدی مانند Form، Service Bus یا Custom JS با همین هویت قابل شناسایی خواهد بود.

![شروع ایجاد Provider از نوع Rest در «مدیریت منبع داده». در این مرحله نوع منبع داده روی Rest قرار می‌گیرد و عنوان منبع داده ثبت می‌شود.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=26194352-DCB3-487A-934A-86F59CB02A3D)

شروع ایجاد Provider از نوع Rest در «مدیریت منبع داده». در این مرحله نوع منبع داده روی Rest قرار می‌گیرد و عنوان منبع داده ثبت می‌شود.

![گام «مدیریت Rest Service»: نمونه تنظیم آدرس سرویس، متد ارسال، Raw Body، نوع محتوای قابل پذیرش (accept) و Allow auto redirect برای یک سرویس REST.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=56799B68-A812-478E-8BFF-50ACCBC9A2E7)

گام «مدیریت Rest Service»: نمونه تنظیم آدرس سرویس، متد ارسال، Raw Body، نوع محتوای قابل پذیرش (accept) و Allow auto redirect برای یک سرویس REST.

![فهرست متدهای قابل انتخاب در Request Method. بسته به نسخه/استقرار، متدهایی مانند GET، POST، PUT، DELETE و HEAD در دسترس هستند.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=55278C0A-3CB0-42CE-A9F6-FDE9E6A70EE5)

فهرست متدهای قابل انتخاب در Request Method. بسته به نسخه/استقرار، متدهایی مانند GET، POST، PUT، DELETE و HEAD در دسترس هستند.

![فهرست مقادیر قابل انتخاب برای Accept Header؛ از جمله application/json، application/json; charset=utf-8 و application/xml.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=57183A03-DAE4-44F7-8425-677528613F86)

فهرست مقادیر قابل انتخاب برای Accept Header؛ از جمله application/json، application/json; charset=utf-8 و application/xml.

## 14.2. گام «مدیریت Rest Service» و فیلدهای آن

این گام بخش اصلی تنظیمات REST Provider است و Contract اصلی درخواست HTTP را مشخص می‌کند. توضیح فیلدهای این مرحله به‌صورت زیر است:

| فیلد | توضیح |
| --- | --- |
| **آدرس سرویس** | آدرس کامل Endpoint مقصد. این آدرس می‌تواند ساده باشد یا شامل متغیرهای درون URL و Query String شود. |
| **متد ارسال** | نوع فراخوانی HTTP مانند GET، POST، PUT، DELETE یا HEAD. |
| **Raw Body** | اگر فعال شود، بدنه درخواست به‌صورت خام مدیریت می‌شود و رفتار مرحله پارامترها نیز متناسب با آن تغییر می‌کند. |
| **نوع محتوای قابل پذیرش (accept)** | مقدار هدر Accept که مشخص می‌کند سرویس ترجیحاً چه نوع پاسخی به سامانه برگرداند. |
| **Allow auto redirect** | در صورت فعال بودن، دنبال‌کردن Redirectهای HTTP برای درخواست مجاز خواهد بود. |

## 14.3. Query Parameter در URL و placeholderها

یکی از بخش‌هایی که قبلاً توضیح کافی نداشت، پارامترهای Query در URL است. در خود سامانه نیز راهنما برای این موضوع وجود دارد:

برای قرار دادن متغیر، نام متغیر دلخواه را داخل علامت { } بگذارید

یعنی اگر بخواهید بخشی از URL یا Query String در زمان اجرا مقداردهی شود، باید یک placeholder داخل آکلاد قرار دهید و سپس در گام پارامترها برای همان نام، پارامتر متناظر تعریف کنید. برای مثال:

https://agent.a2a.ae/SharedApi/Login?rememberMe={rememberMeValue}

در این مثال، `rememberMeValue` یک پارامتر URL است. بنابراین در مرحله پارامترها باید ردیفی تعریف شود که نام پارامتر آن `rememberMeValue`، نوع آن مثلاً `Boolean`، مقدار آن مثلاً `true` و محل قرارگیری آن `url` باشد.

**نتیجه عملی:** placeholder داخل URL و ردیف پارامتر متناظر باید هم‌نام باشند؛ در غیر این صورت سامانه نمی‌تواند مقدار را در آدرس سرویس جای‌گذاری کند.

## 14.4. متد ارسال و نوع Accept

فهرست Methodهای قابل انتخاب و نیز مقادیر Accept از طریق DropDownهای همان صفحه در دسترس هستند. انتخاب صحیح این دو مورد برای سازگاری با سرویس مقصد ضروری است. برای نمونه، در سناریوهای API محور معمولاً `POST` به‌همراه `application/json` یا `application/json; charset=utf-8` استفاده می‌شود.
