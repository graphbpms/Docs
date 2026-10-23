---
id: 1465
title: "پارامترهای REST — Position، Content Type و Constant/Default"
englishTitle: "REST Parameters, Headers & Defaults"
category: "04-provider/web-services"
wikiPath: "1450/1462/1465"
wikiUrl: "https://wiki.graphbpms.ir/1450/1462/1465"
breadcrumbs: ["مدیریت منابع داده Provider","اتصال به Web Service","پارامترهای REST — Position، Content Type و Constant/Default"]
priority: 3
language: 1
isCanonical: true
updatedAt: "2026-09-26T01:20:18"
---

# پارامترهای REST — Position، Content Type و Constant/Default

> [!NOTE]
> 🧭 **سلسله‌مراتب موضوعی:** `مدیریت منابع داده Provider` ❯ `اتصال به Web Service` ❯ `پارامترهای REST — Position، Content Type و Constant/Default`  
> 🔗 **نسخه برخط در سامانه دانشنامه:** [https://wiki.graphbpms.ir/1450/1462/1465](https://wiki.graphbpms.ir/1450/1462/1465)

در این آموزش

-   [14.5. گام «لیست پارامترها» و Request Content Type](#wiki-rest-params-section-01)
-   [14.6. رفتار محل قرارگیری پارامترها (Parameter Location)](#wiki-rest-params-section-02)

## 14.5. گام «لیست پارامترها» و Request Content Type

پس از تنظیم اطلاعات اصلی سرویس، وارد مرحله تعریف پارامترهای ورودی می‌شوید. در این گام ابتدا «نوع محتوای درخواست» یا `Request Content Type` مشخص می‌شود. این انتخاب، نحوه ساخت Payload و همچنین گزینه‌های قابل انتخاب برای محل قرارگیری پارامترها را تعیین می‌کند.

-   **application/json**: برای ارسال داده به‌صورت JSON.
-   **application/json; charset=utf-8**: همان JSON با تعیین صریح charset.
-   **application/x-www-form-urlencoded**: برای ارسال داده‌های فرم URL-Encoded.
-   **multipart/form-data**: برای ارسال فایل و فیلدهای همراه آن.

هر ردیف پارامتر ورودی معمولاً شامل این اجزاست:

-   **نوع داده** مانند String، Boolean یا File
-   **مقدار** یا مقدار پیش‌فرض / نمونه
-   **نام پارامتر**
-   **محل قرارگیری** مانند url، header، body یا form-data

![فهرست نوع محتوای درخواست (Request Content Type) در گام پارامترها. مقادیر متداول شامل application/json، application/json; charset=utf-8، application/x-www-form-urlencoded و multipart/form-data هستند.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=38BB1801-A392-47A2-A21D-7FC5634AC4F5)

فهرست نوع محتوای درخواست (Request Content Type) در گام پارامترها. مقادیر متداول شامل application/json، application/json; charset=utf-8، application/x-www-form-urlencoded و multipart/form-data هستند.

![نمونه تعریف پارامترهای ورودی برای درخواست multipart/form-data؛ شامل Authorization در Header، فایل در form-data و پارامترهای model، language و response\_format.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=8F60630C-3D56-46D2-A2EB-EB0BA845FBAF)

نمونه تعریف پارامترهای ورودی برای درخواست multipart/form-data؛ شامل Authorization در Header، فایل در form-data و پارامترهای model، language و response\_format.

![در درخواست multipart/form-data محل قرارگیری هر پارامتر می‌تواند از نوع url، header یا form-data باشد. این تصویر منوی انتخاب محل قرارگیری برای پارامتر فایل را نشان می‌دهد.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=8E9B0402-B472-4DAF-9B92-5A56DD7B58E0)

در درخواست multipart/form-data محل قرارگیری هر پارامتر می‌تواند از نوع url، header یا form-data باشد. این تصویر منوی انتخاب محل قرارگیری برای پارامتر فایل را نشان می‌دهد.

![نمونه تعریف پارامترها برای application/json; charset=utf-8. در این حالت پارامترها معمولاً در body یا header قرار می‌گیرند؛ مانند Authorization در header و model/language در body.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=663CF068-F705-4997-BD10-B1356C513528)

نمونه تعریف پارامترها برای application/json; charset=utf-8. در این حالت پارامترها معمولاً در body یا header قرار می‌گیرند؛ مانند Authorization در header و model/language در body.

![نمونه تعریف پارامترها برای application/x-www-form-urlencoded. در این حالت نیز پارامترها در body یا header ثبت می‌شوند و سامانه payload را متناسب با این Content-Type می‌سازد.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=5C62BD03-DC7E-4980-8BC1-4C5599ACD9A5)

نمونه تعریف پارامترها برای application/x-www-form-urlencoded. در این حالت نیز پارامترها در body یا header ثبت می‌شوند و سامانه payload را متناسب با این Content-Type می‌سازد.

![راهنمای استفاده از Query Parameter در URL. برای قراردادن متغیر در آدرس سرویس باید نام متغیر دلخواه داخل آکلاد قرار گیرد؛ مانند rememberMe={rememberMeValue}.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=C8BFCFC3-6262-41A5-9D48-C4948264592E)

راهنمای استفاده از Query Parameter در URL. برای قراردادن متغیر در آدرس سرویس باید نام متغیر دلخواه داخل آکلاد قرار گیرد؛ مانند rememberMe={rememberMeValue}.

![نمونه کامل تعریف Query Parameter متناظر با placeholder موجود در URL. در این مثال پارامتر rememberMeValue با نوع Boolean، مقدار true و محل قرارگیری url تعریف شده است.](https://portal.graphbpms.ir/Archive/GetFileAnonymous?id=38786251-5D3D-47CD-9F51-711B622143EF)

نمونه کامل تعریف Query Parameter متناظر با placeholder موجود در URL. در این مثال پارامتر rememberMeValue با نوع Boolean، مقدار true و محل قرارگیری url تعریف شده است.

## 14.6. رفتار محل قرارگیری پارامترها (Parameter Location)

محل قرارگیری پارامتر به Content-Type و ماهیت پارامتر وابسته است:

| سناریو | محل‌های رایج | توضیح |
| --- | --- | --- |
| **Query / Path like URL** | url | برای مقادیری که باید داخل آدرس یا Query String جای‌گذاری شوند. |
| **Header** | header | برای هدرهایی مانند Authorization. |
| **JSON Request** | body | برای فیلدهایی که داخل بدنه JSON قرار می‌گیرند. |
| **Form URL Encoded** | body | برای کلید/مقدارهایی که در بدنه فرم‌مانند ارسال می‌شوند. |
| **Multipart** | form-data | برای فایل‌ها و فیلدهای همراه آن در فرم چندبخشی. |

در نمونه‌های تصویری، دیده می‌شود که:

-   پارامتر `Authorization` در `header` تعریف شده است.
-   پارامتر `file` در سناریوی multipart از نوع `File` و محل `form-data` دارد.
-   پارامترهای `model` و `language` بسته به Content-Type در `body` یا `form-data` قرار گرفته‌اند.
-   پارامتر `rememberMeValue` برای پرکردن Query String در محل `url` ثبت شده است.

**توضیح تکمیلی الزامی — Constant / Default در REST Provider**

اگر برای یک Parameter مقدار Default/Constant ذخیره شده باشد، Caller مجبور نیست آن Parameter را در هر Call دوباره ارسال کند. اگر Caller برای همان Parameter یک مقدار مؤثر ارسال کند، مقدار ارسال‌شده استفاده می‌شود؛ و فقط در صورت نبود مقدار مؤثر، Default ذخیره‌شده جایگزین می‌شود.

| مقدار ارسالی Caller | Default ذخیره‌شده | مقدار نهایی |
| --- | --- | --- |
| ارسال نشده | `fa` | `fa` |
| `en` | `fa` | `en` |

این رفتار با **Static Backend Parameter در Service Bus** یکی نیست؛ در Service Bus مقدار Static برای اجرای Backend قفل می‌شود.
